# Harnessing the Power of the Pulumi Automation API

The Pulumi Automation API is an advanced feature of the Pulumi Infrastructure as Code (IaC) tool that was developed to solve a number of problems associated with the management, deployment, and automation of cloud resources.

The Automation API allows developers to define infrastructure programmatically, which is in contrast to traditional IaC approaches that require configuration files or domain-specific languages (DSLs). Essentially, it's a robust and fully-featured software library for creating and managing Pulumi stacks, and can be used within your software code like any other package or library.

## Value of Pulumi Automation API:

- More Flexibility: The Automation API offers more flexibility than traditional IaC tools, as it can be embedded into software applications directly. This enables the use of loops, conditionals, and other programming constructs that aren't always easy (or even possible) to use in DSLs or declarative syntax.
- Better Abstractions: Developers can create custom abstractions that match their business logic, making it easier to manage complex infrastructure requirements.
- Advanced Automation: It can also be used for advanced automation use-cases, including creating custom deployment workflows, integrating with CI/CD pipelines, automated testing and more.
- Language Choice: Since Pulumi supports multiple languages (like Python, JavaScript/TypeScript, Go, etc.), you can choose the one that best suits your project needs or the one your team is most comfortable with.

## Problems it Solves:
- Difficulty with DSLs: Domain-specific languages can be difficult to learn and lack the power and expressiveness of full-fledged programming languages. The Automation API addresses this problem by allowing infrastructure to be defined in general-purpose programming languages.
- Infrastructure Complexity: Managing complex infrastructure requirements can be challenging with traditional IaC tools. By enabling custom abstractions, the Automation API allows developers to encapsulate complexity in ways that are easier to manage.
- Integration and Customization: Integration with existing systems and creation of custom deployment workflows can be complex with traditional IaC tools. The Automation API solves this by allowing developers to programmatically manage Pulumi stacks.

## Using a Pulumi Program versus a Pulumi Automation API Program:

A Pulumi program is a more traditional way of defining and managing infrastructure as code using Pulumi's libraries. It is typically written in a general-purpose programming language and executed using Pulumi's CLI.

On the other hand, a Pulumi Automation API program also allows defining and managing infrastructure, but it can orchestrate the entire lifecycle of infrastructure within your application, bypassing the need for a separate CLI.

Below are examples to illustrate the difference.

### Pulumi Program (Python)

```
import pulumi
import pulumi_aws as aws

queue = aws.sqs.Queue("queue",
    content_based_deduplication=True,
    fifo_queue=True)

```

To run this program, you'd use the Pulumi CLI, first running `pulumi up` to create the stack and deploy the resources, then `pulumi destroy` to tear down the resources when you're done.

### Pulumi Automation API Program (Python)

```
Code Example 2
TBD
```
