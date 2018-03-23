# Handling Events<a name="ecs_cwet_handling"></a>

Amazon ECS sends events on an "at least once" basis\. This means you may receive more than a single copy of a given event\. Additionally, events may not be delivered to your event listeners in the order in which the events occurred\.

To enable proper ordering of events, the `detail` section of each event contains a `version` property\. Events with a higher version property number should be treated as occurring later than events with lower version numbers\. Events with matching version numbers can be treated as duplicates\.

## Example: Handling Events in an AWS Lambda Function<a name="ecs_cwet_handling_example"></a>

The following example shows a Lambda function written in Python 2\.7 that captures both task and container instance state change events, and saves them to one of two Amazon DynamoDB tables:
+ *ECSCtrInstanceState*: Stores the latest state for a container instance\. The table ID is the `containerInstanceArn` value of the container instance\.
+ *ECSTaskState*: Stores the latest state for a task\. The table ID is the `taskArn` value of the task\.

```
import json
import boto3

def lambda_handler(event, context):
    id_name = ""
    new_record = {}

    # For debugging so you can see raw event format.
    print('Here is the event:')
    print(json.dumps(event))

    if event["source"] != "aws.ecs":
       raise ValueError("Function only supports input from events with a source type of: aws.ecs")

    # Switch on task/container events.
    table_name = ""
    if event["detail-type"] == "ECS Task State Change":
        table_name = "ECSTaskState"
        id_name = "taskArn"
        event_id = event["detail"]["taskArn"]
    elif event["detail-type"] == "ECS Container Instance State Change":
        table_name = "ECSCtrInstanceState"
        id_name =  "containerInstanceArn"
        event_id = event["detail"]["containerInstanceArn"]
    else:
        raise ValueError("detail-type for event is not a supported type. Exiting without saving event.")

    new_record["cw_version"] = event["version"]
    new_record.update(event["detail"])

    # "status" is a reserved word in DDB, but it appears in containerPort
    # state change messages.
    if "status" in event:
        new_record["current_status"] = event["status"]
        new_record.pop("status")


    # Look first to see if you have received a newer version of an event ID.
    # If the version is OLDER than what you have on file, do not process it.
    # Otherwise, update the associated record with this latest information.
    print("Looking for recent event with same ID...")
    dynamodb = boto3.resource("dynamodb", region_name="us-east-1")
    table = dynamodb.Table(table_name)
    saved_event = table.get_item(
        Key={
            id_name : event_id
        }
    )
    if "Item" in saved_event:
        # Compare events and reconcile.
        print("EXISTING EVENT DETECTED: Id " + event_id + " - reconciling")
        if saved_event["Item"]["version"] < event["detail"]["version"]:
            print("Received event is more recent version than stored event - updating")
            table.put_item(
                Item=new_record
            )
        else:
            print("Received event is more recent version than stored event - ignoring")
    else:
        print("Saving new event - ID " + event_id)

        table.put_item(
            Item=new_record
        )
```