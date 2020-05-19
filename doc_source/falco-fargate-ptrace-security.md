# Securing Fargate Tasks with Falco Runtime Security

ECS Fargate `1.4.0` [announced support](https://aws.amazon.com/about-aws/whats-new/2020/04/aws-fargate-launches-platform-version-14/) for the `CAP_SYS_PTRACE` linux capability.

This new feature can be implemented with [Falco](falco.org), an [open source runtime security project](https://github.com/falcosecurity/falco) originally built by Sysdig, Inc and later donated to the Cloud Native Computing Foundation.

Falco uses `ptrace(2)` with Fargate in ECS to detect anomalous behavior at runtime. 

Falco can be configured to send alerts to STDOUT such that ECS can consume the logs. They can then be used to trigger alerts and alarms using AWS logging solutions such as CloudWatch. 

## Dependencies

In order to begin securing a task in ECS a few components need to be installed within the container.

 - Falco userspace daemon
 - Falco pdig tracing utility
 
After these components are installed in the container, Falco can be used in the following ways.

 - Launch an arbitrary process, and begin tracing the original process and all of it's child processes. 
 - Attach to a running process, and begin tracing the process and all of it's child processes. 
 
In order for the Falco `pdig` utility to work, the `CAP_SYS_PTRACE` capability must be enabled in a task definition for a Fargate 1.4 or greater ECS Cluster. 

Add the following section to the container JSON you wish to secure while creating the task definition. 

```json
            "linuxParameters": {
                "capabilities": {
                    "add": [
                        "SYS_PTRACE"
                    ],
                    "drop": null
                }
            },
```

## Installing Falco

_Note:_ The Falco `pdig` components are targetted to be released in an upcoming Falco release. This documentation will need to be updated after the initial release.

For now you can use the `krisnova/falco-trace:latest` container image which is built on debian and contains Falco and `pdig` pre-installed. 
The original work to get this working can be found at [github.com/kris-nova/falco-trace](https://github.com/kris-nova/falcotrace).

Follow the official [Falco installation](https://falco.org) guide for installing Falco with `pdig`. 

Validate that Falco can be ran within the container

```bash 
falco -u 

Sun May 17 13:06:53 2020: Falco initialized with configuration file /etc/falco/falco.yaml
Sun May 17 13:06:53 2020: Loading rules from file /etc/falco/rules.d/application_rules.yaml:
Sun May 17 13:06:53 2020: Loading rules from file /etc/falco/rules.d/falco_rules.local.yaml:
Sun May 17 13:06:53 2020: Loading rules from file /etc/falco/rules.d/falco_rules.yaml:
^CSun May 17 13:06:59 2020: SIGINT received, exiting...
Events detected: 0
Rule counts by severity:
Triggered rules by rule name:
Syscall event drop monitoring:
   - event drop detected: 0 occurrences
   - num times actions taken: 0
```

## Running a process with pdig

The above installation should also include a `ptrace(2)` based launcher utility called `pdig`.

The `pdig` utility can either be used to launch a process or attach to a running process. 

Here are some examples of using `pdig` to launch a process.

Running a daemon with `pdig`

```bash
pdig -a ./app --daemon
```

Running a program in the background, and attaching to the pid at runtime

```bash
./app --pidfile=/var/run/app &
pdig -p $(cat /var/run/app)
```

Attach to pid 1 with `pdig`. This will trace all pids in the container.

```bash
pdig -p 1 && ./myapp &
```

## Logs

In order to consume the Falco logs in ECS the Falco logs must go to STDOUT. 

This can be done a number of ways. One option is to tail the falco log file.

First configure falco to log to a file. One example is [here](https://github.com/kris-nova/falco-trace/blob/master/etc/falco/falco.yaml) or you can edit the file manually.

```bash
emacs /etc/falco/falco.yaml

# Edit the following

file_output:
  enabled: true
  keep_alive: false
  filename: /var/log/falco.log
```

After Falco is logging to a file you can run your application.

```bash
pdig -a ./myapp &
falco -u --daemon 
tail -f /var/log/falco.log &
```

The logs will now be picked up by ECR and can be used with other AWS services. 
