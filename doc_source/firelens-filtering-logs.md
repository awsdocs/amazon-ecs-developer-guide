# Filtering logs using regular expressions<a name="firelens-filtering-logs"></a>

Fluentd and Fluent Bit both support filtering of logs based on their content\. FireLens provides a simple method for enabling this filtering\. In the log configuration `options` in a container definition, you can specify the special keys `exclude-pattern` and `include-pattern` that take regular expressions as their values\. The `exclude-pattern` key causes all logs that match its regular expression to be dropped\. With `include-pattern`, only logs that match its regular expression are sent\. These keys can be used together\.

The following example demonstrates how to use this filter\.

```
{
   "containerDefinitions":[
      {
         "logConfiguration":{
            "logDriver":"awsfirelens",
            "options":{
               "@type":"cloudwatch_logs",
               "log_group_name":"firelens-testing",
               "auto_create_stream":"true",
               "use_tag_as_stream":"true",
               "region":"us-west-2",
               "exclude-pattern":"^[a-z][aeiou].*$",
               "include-pattern":"^.*[aeiou]$"
            }
         }
      }
   ]
}
```