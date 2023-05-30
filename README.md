# Harnessing the Power of the Pulumi Automation API

In today's cloud-centric world, Infrastructure as Code (IaC) has become a necessity for robust, scalable and reliable system administration. Among the various IaC tools, Pulumi has set itself apart with its support for general-purpose programming languages instead of domain-specific languages (DSLs) or declarative syntax. It pushes the boundaries of IaC even further with the Pulumi Automation API.

## What is the Pulumi Automation API?

The Pulumi Automation API is an advanced feature of the [Pulumi IaC tool](https://www.pulumi.com/docs/) that was developed to solve a number of problems associated with the management, deployment, and automation of cloud resources. Essentially, it's a robust and fully-featured software library for creating and managing Pulumi stacks, and can be used within your software code like any other package or library. The value of the Pulumi Automation API lies in its ability to offer:

- **More Flexibility**: The Automation API offers more flexibility than traditional IaC tools, as it can be embedded into software applications directly. This enables the use of loops, conditionals, and other programming constructs that aren't always easy (or even possible) to use in DSLs or declarative syntax.
- **Better Abstractions**: Developers can create custom abstractions that match their business logic, making it easier to manage complex infrastructure requirements.
- **Advanced Automation**: It can also be used for advanced automation use-cases, including creating custom deployment workflows, integrating with CI/CD pipelines, automated testing and more.
- **Language Choice**: Since Pulumi supports multiple languages (like Python, JavaScript/TypeScript, Go, etc.), you can choose the one that best suits your project needs or the one your team is most comfortable with.

## Using a Pulumi Program versus a Pulumi Automation API Program

A Pulumi program is a more traditional way of defining and managing infrastructure as code using Pulumi's libraries. It is typically written in a general-purpose programming language and executed via Pulumi's CLI using commands such as `pulumi up`, `pulumi preview`, `pulumi destroy`, `pulumi stack init` etc.

A Pulumi Automation API program also allows defining and managing infrastructure as code, but where it differs is that it can orchestrate the entire lifecycle of infrastructure directly within embedded environments (such as web servers), eliminating the need to interact with a CLI.

Simply put, the CLI is excellent for manually deploying infrastructure and working with individual stacks, but for complex deployments, customized workflows, or integration with other software, the Automation API shines for the greater flexibility and control that it offers.

Let's explore a practical Python example for both options to illustrate the difference.

### Pulumi Program (Python)

```
import pulumi
import pulumi_aws as aws

queue = aws.sqs.Queue("queue",
    content_based_deduplication=True,
    fifo_queue=True)

```

This program would be accompanied by additional project files such as the `Pulumi.yaml` file which is responsible for defining [the configuration of the project](https://www.pulumi.com/docs/concepts/projects/). To run this program, you'd use the Pulumi CLI, first running `pulumi up` to create the stack and deploy the resources, then `pulumi destroy` to tear down the resources when you're done.

### Pulumi Automation API Program (Python)

```
import sys
import json
import pulumi
from pulumi import automation as auto
import pulumi_aws as aws


# This is the pulumi program in "inline function" form
# For this example, to run our program, we can run python main.py
def pulumi_program():
    # Create a FIFO SQS queue

    queue = aws.sqs.Queue("queue",
        content_based_deduplication=True,
        fifo_queue=True)
    

# To destroy our program, we can run python main.py destroy
destroy = False
args = sys.argv[1:]
if len(args) > 0:
    if args[0] == "destroy":
        destroy = True

project_name = "inline_sqs_project"
stack_name = "dev"

# create or select a stack matching the specified name and project.
# this will set up a workspace with everything necessary to run our inline program (pulumi_program)
stack = auto.create_or_select_stack(stack_name=stack_name,
                                    project_name=project_name,
                                    program=pulumi_program)

print("successfully initialized stack")

# for inline programs, we must manage plugins ourselves
print("installing plugins...")
stack.workspace.install_plugin("aws", "v4.0.0")
print("plugins installed")

# set stack configuration specifying the AWS region to deploy
print("setting up config")
stack.set_config("aws:region", auto.ConfigValue(value="eu-central-1"))
print("config set")

print("refreshing stack...")
stack.refresh(on_output=print)
print("refresh complete")

if destroy:
    print("destroying stack...")
    stack.destroy(on_output=print)
    print("stack destroy complete")
    sys.exit()

print("updating stack...")
up_res = stack.up(on_output=print)
print(f"update summary: \n{json.dumps(up_res.summary.resource_changes, indent=4)}")
```

In this example, we're doing the same thing as in the first example, but we're doing it all within our Python program, without needing to use the Pulumi CLI. We define the infrastructure in a function, create a Pulumi project and stack, set up the config, and deploy the stack, all programmatically. By defining our infrastructure as a function, we open up a lot more flexibility and control over the infrastructure lifecycle, including but not limited to things like:
- dynamically creating resources
- using inputs/outputs
- managing multiple stacks
- catching errors
- cleaning up resources

## Conclusion

In an age where software and infrastructure are becoming increasingly intertwined, tools that bridge the gap between these two worlds offer immense value. Pulumi's Automation API is a great example of such a tool, offering flexibility, power, and a high degree of customization. Whether you're managing complex infrastructure requirements, building custom deployment workflows, or just tired of wrestling with DSLs, the Automation API could be the game-changer you need.
