# rails-console-ecs
This is simple shell script for executing rails console in a ECS Task.
You can exec `rails c` command from outside of ECS Cluster by executing only one command.

## How it works

1. (optionally) Find the Task Definition to run from a Service (Task Definition
   can also be specified on the command line to skip this step)
2. Run a Task using the Task Definition
3. Get private IP by AWS CLI
4. Get docker container ID via SSH
5. Exec `rails c` command in the container via SSH
6. Stop the Task

In case of SIGINT (Ctrl-C), SIGTERM (`kill`) or an error executing any of the
steps listed above, the Task (if it was already started) is stopped. This should
prevent accidentally leaving lots of useless tasks lying around.

## Usage

```
Usage:
    rails-console-ecs [options]

Options:
    -v    Version
    -h    Show help of this command
    -c    Ecs cluster which a task runs in. uses RAILS_C_ECS_CLUSTER by default
    -d    Ecs task definition. uses RAILS_C_TASK_DEFINITION by default, or
          discovers the task definition from the service specified with -s.
    -e    Rails env. uses RAILS_C_ENV by default
    -j    SSH jump host to connect via. uses RAILS_C_JUMPHOST by default.
          You can specify the user with -j user@server. if unset, jump host will
          not be used.
    -p    SSH port to use for connection - defaults to RAILS_C_PORT or 22.
    -r    Region. uses RAILS_C_REGION by default
    -s    Service. uses RAILS_C_SERVICE by default. Ignored if -d (or
          RAILS_C_TASK_DEFINITION) is specified
    -u    SSH user to use. uses RAILS_C_SSHUSER. defaults to ec2-user if not
          specified.
```

## Installation

### Requirements
- [aws-cli](https://github.com/aws/aws-cli)
- [jq](https://github.com/stedolan/jq)

### Install rails-console-ecs

#### Linux

```shell
curl https://raw.githubusercontent.com/mnc/rails-console-ecs/master/rails-console-ecs | sudo tee /usr/bin/rails-console-ecs
sudo chmod +x /usr/bin/rails-console-ecs
```

#### Mac

```shell
curl https://raw.githubusercontent.com/mnc/rails-console-ecs/master/rails-console-ecs | sudo tee /usr/local/bin/rails-console-ecs
sudo chmod +x /usr/local/bin/rails-console-ecs
```

## LICENSE
MIT
