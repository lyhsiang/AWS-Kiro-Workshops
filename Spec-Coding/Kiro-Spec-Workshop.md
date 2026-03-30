# 🚀 Kiro Spec Workshop: Hands-On Exercises

## Introduction

In this section, you will learn how to use Kiro in a spec-driven development style. Unlike vibe coding where you give high-level instructions and let Kiro figure everything out, spec coding involves creating detailed specifications that break down complex features into manageable, well-defined tasks. This approach provides better control, documentation, and iterative development for complex projects.

Kiro's Spec feature allows you to formalize the design and implementation process by iterating on requirements, design, and implementation tasks with structured documentation before letting the agent work through the implementation.

This is a fully hands-on workshop. Through a series of exercises, you will design, develop, and deploy the same webshooting game as the vibe coding workshop, but using a more structured, specification-driven approach. By the end, you'll have a fully functional chatbot with text, image, and voice capabilities, secured with user authentication.

### 📋 Prerequisites

- Installed and running Kiro IDE ( https://kiro.dev/downloads )
- AWS account with S3 and CloudFront access permissions
- Configured AWS CLI (for deployment)
- Install uv from Astral ( https://docs.astral.sh/uv/getting-started/installation )

**Login to Kiro**


<img width="360" height="400" alt="Screenshot 2025-11-13 at 11 11 34 AM" src="https://github.com/user-attachments/assets/71931a48-515a-46a4-878c-73b4d987bb87" />


**Logout Kiro** 

- Click account information from the left down corner 
<img width="351" height="169" alt="Screenshot 2025-09-03 at 9 04 27 AM" src="https://github.com/user-attachments/assets/a004b61d-efcf-4159-bd1f-3d578db137e4" />


### Create Project Folder

- On your computer, create a folder named `Kiro_workshop`
- This will become the project workspace for your shooting game application

### Open Project in Kiro

- Launch Kiro IDE
- Click the "Open a project" button
- Select the `Kiro_workshop` folder you just created
- Kiro will initialize the project workspace

<img width="472" height="360" alt="image" src="https://github.com/user-attachments/assets/98371ca9-9c91-48ab-a978-b7ed09e6080c" />

### Access Let's Build Feature

- In Kiro, navigate to the "Let's build" page
- You will see two available options:
  - **Vibe** - Chat first, build later. Explore ideas and iterate as you discover requirements
  - **Spec** - Plan first, build later. Create requirements and design before starting to code
- Set the model to "Auto" (or any other available model )
- Ensure Autopilot is enabled

<img width="472" height="360" alt="image" src="https://github.com/user-attachments/assets/7a6f1648-f717-4e44-a29a-189d096a70d3" />

##  🚀 Exercise 1 - Use Vibe Coding to create a Web base shooting game OR a e-commerce website 

Web base shooting game
```
Please help me to build a web-based shooting game use keyboard control, including a quick instruction page

```

E-commerce website 

```
 Please help me to build a simple e-commerce website with three web pages, including Home page, Product page and Shopping Cart page

```

##  🚀 Exercise 2 - Enable / Add MCP server in Kiro 

Model Context Protocol (MCP) extends Kiro's capabilities by connecting to specialized servers that provide additional tools and context. This guide helps you set up, configure, and use MCP servers with Kiro.

> :warning:
> Before you start this exercise, please make sure you have install the uv command in your environment. To check the uv command whether has been installed, you could use following commnad:

```
$ uv self version
```
> If you didn't install it yet, please check [Astral](https://docs.astral.sh/uv/getting-started/installation) to install uv command.


### 2.1 Click Enable / add more MCP server 

you can easily add MCP server via Kiro public [documentation](https://kiro.dev/docs/mcp/servers/#mcp-server-directory)  or use the following configuration

<img width="368" height="268" alt="image" src="https://github.com/user-attachments/assets/0906b563-d0bf-4102-a0f8-332c7d277f97" />

<!--- :warning: please make sure you have Context7 account and use your own  API-key with your own  https://context7.com/ --->

> :warning:
> The AWS Diagram MCP Server requires additional dependencies. Please install them before proceeding:
> 1. Install Python 3.10+: `uv python install 3.10`
> 2. Install GraphViz: https://www.graphviz.org/ (on macOS: `brew install graphviz`)


```
{
  "mcpServers": {
    "chrome-devtools": {
      "command": "npx",
      "args": [
        "-y",
        "chrome-devtools-mcp@latest"
      ],
      "disabled": false
    },
    "Context7": {
      "command": "npx",
      "args": [
        "-y",
        "@upstash/context7-mcp"
      ],
      "env": {},
      "disabled": false,
      "autoApprove": []
    },
    "awsknowledge": {
      "type": "http",
      "url": "https://knowledge-mcp.global.api.aws",
      "disabled": false
    },
    "awslabs.aws-iac-mcp-server": {
      "command": "uvx",
      "args": [
        "awslabs.aws-iac-mcp-server@latest"
      ],
      "env": {
        "FASTMCP_LOG_LEVEL": "ERROR"
      },
      "disabled": false,
      "autoApprove": []
    },
    "awslabs.aws-pricing-mcp-server": {
      "command": "uvx",
      "args": [
        "awslabs.aws-pricing-mcp-server@latest"
      ],
      "env": {
        "FASTMCP_LOG_LEVEL": "ERROR"
      },
      "disabled": false,
      "autoApprove": []
    },
    "awslabs.aws-diagram-mcp-server": {
      "command": "uvx",
      "args": [
        "awslabs.aws-diagram-mcp-server"
      ],
      "env": {
        "FASTMCP_LOG_LEVEL": "ERROR"
      },
      "disabled": false,
      "autoApprove": [
        "list_icons",
        "generate_diagram",
        "get_diagram_examples"
      ]
    }
  }
}

```

## 🚀 Exercise 3 - Create your own Steering rule 

### 3.1 Add your Development standards into 


<img width="908" height="669" alt="image" src="https://github.com/user-attachments/assets/3988dbdf-0be4-4e72-816b-4cbb5dcec145" />



Create development-standards.md in global agent steering with following contexts 


```
---
inclusion: always
---

# Development Standards

## Dependency Management
- Use latest stable versions of all libraries and dependencies
- Leverage Context7 MCP server to verify compatibility before adding dependencies
- Justify each new dependency with clear business or technical value
- Prefer well-maintained libraries with active communities
- Document version constraints in project files
- Remove unused dependencies regularly
- Use lock files to ensure consistent installations across environments

## Code Quality Standards
- Never create duplicate files with suffixes like `_fixed`, `_clean`, `_backup`, etc.
- Work iteratively on existing files (hooks handle commits automatically)
- Include relevant documentation links in code comments
- Follow language-specific conventions (TypeScript for CDK, Python for Lambda)
- Use meaningful variable and function names
- Keep functions small and focused on single responsibilities
- Implement proper error handling and logging

## File Management
- Maintain clean directory structures
- Use consistent naming conventions across the project
- Avoid temporary or backup files in version control
- Organize code logically by feature or domain
- Keep configuration files at appropriate levels (project vs user)

## Documentation Approach
- Maintain single comprehensive README covering all aspects including deployment
- Reference official sources through MCP servers when available
- Update documentation when upgrading dependencies
- Keep documentation close to relevant code
- Use inline comments for complex business logic
- Document API endpoints and data structures
- Include setup and deployment instructions

## Version Control Integration
- Commit frequently with meaningful messages
- Use feature branches for development
- Keep main branch deployable at all times
- Tag releases appropriately
- Use .gitignore to exclude generated files and secrets

## Quality Assurance
- Write tests for new functionality
- Run tests before committing changes
- Use linting and formatting tools consistently
- Perform code reviews for all changes
- Monitor code coverage and maintain high standards

```

##  🚀 Exercise 4 - Generate Steering document in Kiro 

Steering gives Kiro persistent knowledge about your project through markdown files in .kiro/steering/. Instead of explaining your conventions in every chat, steering files ensure Kiro consistently follows your established patterns, libraries, and standards.

#### 4.1 Click Generate foundation steering files

<img width="735" height="242" alt="Screenshot 2025-11-18 at 8 12 04 AM" src="https://github.com/user-attachments/assets/a676eca5-b4c7-4a6c-90b2-7bea0632489d" />


#### 4.2 Review the outputs markdown file from Agent Steering to make update if need

- Product Overview (product.md): Defines your product's purpose, target users, and business objectives

- Technology Stack (tech.md): Documents frameworks, libraries, and technical constraints

- Project Structure (structure.md): Outlines file organization, naming conventions, and architectural decisions

<img width="908" height="669" alt="image" src="https://github.com/user-attachments/assets/1ab7adb6-1f59-4a2d-ba03-fef8ea563160" />



##  🚀 Exercise 5 - Use Specs coding for more comprehensive implementation 


Web base shooting game
```
Create a more comprehensive modern web-based shooting application with following high-level requirements:

1) A healthy bar or status of enemies for user to understand how many time left to take it down 
2) Allow user to select different character, please refer to 1) Annabelle  2) joker 3) Jason Voorhees.
3) Implement a power-up system , add some random items temporarily power up the character. Such as shooting more fast, or shooting more wide, please create 6 different items, the more special the better, also include explanation of the power up items

```


( Optional )
```
The web application should be based on 3-tier architecture, including 
- React + Canvas / WebGL based in prestntation tier ( Client ) for inputing handling and rendering
- Node.js + Express/ Socket.io based backend with application logics like Game logic , state management etc
- PostgreSQL database for relations data  to store products and shopping cart items
- Redis for session data and real-time leadersboards 

```

E-commerce website 

```
Create a more comprehensive modern E-commerce website  with following high-level requirements:

1) Home page with product list of 20 sample products (use emoji to simulate product pictures)
2) Product page with product description, price, quantity, add-to-cart button, and sample user reviews
3) Shopping Cart page with product to be purchased list, total cost, and action buttons such as quantity update and item delete
```


(Optional)
```
The web application should be based on 3-tier architecture, including 
- React based frontend for rendering UI
- Node.js based backend with application logics like add and remove items in shopping cart
- SQL based database to store products and shopping cart items

```

#### 5.1 Review the Requirements ( requirements.md ) provide by Kiro 

<img width="731" height="630" alt="image" src="https://github.com/user-attachments/assets/b024f8a0-5387-44a3-a9d8-b55adbf6362d" />

#### 5.2 Move to Design phase and review design ( design.md )
<img width="635" height="434" alt="image" src="https://github.com/user-attachments/assets/87efa607-f6d5-4113-9102-037855d1dc6b" />


#### 5.3 Start implementation plan , review the Tasklist ( tasks.md )
<img width="611" height="418" alt="image" src="https://github.com/user-attachments/assets/e2446220-7e17-4e67-a53c-f182cf0f11aa" />


#### 5.4 Start Task by click the :zap:

<img width="568" height="332" alt="image" src="https://github.com/user-attachments/assets/3a3f912d-fe8d-4e63-b7d1-5ff21de23e4e" />


##  🚀 Exercise 6 - Add Deploy on AWS Skill in Kiro

Agent Plugins for AWS equip AI coding agents with skills to help you architect, deploy, and operate on AWS. The **Deploy on AWS** skill analyzes your codebase, recommends AWS services, estimates costs, generates Infrastructure as Code (CDK/CloudFormation), and guides you through deployment.

For more information, please refer to the [Agent Plugins for AWS](https://github.com/awslabs/agent-plugins) repository.

#### 6.1 Install the Deploy on AWS skill

In Kiro, open the Skills panel and add the `deploy` skill from the [deploy-on-aws](https://github.com/awslabs/agent-plugins/tree/main/plugins/deploy-on-aws/skills/deploy) plugin in the `awslabs/agent-plugins` repository.

#### 6.2 Verify the skill is active

The skill triggers on phrases like: "deploy to AWS", "host on AWS", "run this on AWS", "AWS architecture", "estimate AWS cost", "generate infrastructure".

Try asking Kiro:
```
Analyze my current application and recommend an AWS architecture for deployment
```

##  🚀 Exercise 7 - Add Hooks in Kiro 

Agent Hooks are powerful automation tools that streamline your development workflow by automatically executing predefined agent actions when specific events occur in your IDE. With hooks, you eliminate the need to manually request routine tasks and ensure consistency across your codebase.

### 7.1 Click "+" on AGENT HOOKS to add new hooks


<img width="393" height="731" alt="image" src="https://github.com/user-attachments/assets/22af98c2-f7e8-470c-9cf6-ec695a711f30" />


### 7.2 Describe the hook using natural language

We want agent hooks to help us monitoring the CDK resource files change, and keep generating the AWS service diagram by using AWS Diagram MCP Server.

```
1) Analyze the modified CDK files and generate or update AWS service architecture diagrams using the Python diagrams package DSL.
2) Parse the CDK code to identify AWS services, their relationships, and data flow. 
3) If the previous diagram not exist, create a visual representation showing the infrastructure components, connections, and dependencies. Include proper grouping for VPCs, subnets, and logical service boundaries. 
4) delete the previous diagram before create new one. Output the Python diagrams code that can be executed to generate the architecture diagram.
```

After the hook create complete and CDK resource files have change, you should see the similiar diagram as following example:

<img width="1620" height="2106" alt="image" src="https://github.com/user-attachments/assets/0d9e81dc-0ffd-4837-b768-7a852290538c" />

<!--
##  🚀 Exercise 9 - Add a CDK Steering in workspace steering

Create CDK_Best_Practices.md in steering with following contexts 

```
---
inclusion: always
---


# CDK Best Practices

## Basics
- Use projen for project initialization and file management
- Use the latest version of the CDK, found here: https://github.com/aws/aws-cdk/releases
- Use cdk-iam-floyd for IAM policy generation
- Additional CDK apps should have a projen task with a prefix `cdk:`. E.g. `cdk:iam-roles`

## Structure
- All files in the `src/**` directory
- Applications in the `src/` directory
- Stacks in the `src/stacks/**` directory
- Constructs in the `src/constructs/**` directory
- Stages in the `src/stages/**` directory
- Lambda function handlers in a sub-directory of the defining construct, called `handler`
- Pascal-casing for filenames (e.g. `SomeConstruct.ts`)
- Each custom construct should reside in its own file named the same as the construct

## Apps
- SHOULD contain distinct stack/stage instances for each environment
- SHOULD provide each stack/stage with account/region specific values
- Context SHOULD NOT be used for anything, at all

## Stacks
- SHOULD be responsible for importing resources (`Vpc.fromLookup()`, `Bucket.fromBucketName()`, etc.)
- SHOULD be responsible for instantiating constructs

## Constructs
- SHOULD save the incoming constructor props as a private field
- SHOULD create all resources in protected methods, not in the constructor
- SHOULD NOT import resources (e.g. `Vpc.fromLookup()`)
- SHOULD be passed concrete objects representing resources
- Properties representing resource identifiers should use template literal types (e.g. `vpc-${string}`)

## Tests
- All tests in the `test/` directory
- Tests should match construct names (e.g. `SomeConstruct.test.ts`)
- Use fine-grained assertions for constructs
- Use snapshot tests for stacks
- Mock `Code.fromAsset` and `ContainerImage.fromAsset` calls

## Lambda Functions
- Use `NodejsFunction` or `PythonFunction` whenever possible

```
-->

#### You can find more best practices from [here](https://github.com/awsdataarchitect/kiro-best-practices/tree/main/.kiro/steering)

##  🚀 Exercise 8 - Create a new Spec for AWS enviroment deployment 

web-base shooting game 

```
I have a web-based shooting application need to deploy to AWS Cloud. Create a comprehensive AWS architecture with following high-level requirements:
1) Must use AWS serverless services, such as Cloudfront, S3, etc..
2) Follow each service's configuration best practice
3) Prefer to use Infrasturce as Code solution to deploy, such as AWS CDK.
4) Since we are in demo, please do not add any new feature for the game.
```

E-commerce website 
```
I have a E-commerce website need to deploy to AWS Cloud. Create a comprehensive AWS architecture with following high-level requirements:
1) Use Cloudfront + S3 for frontend, ALB + ECS fargate for backend and RDS SQL server for my database
2) Follow each service's configuration best practice
3) Prefer to use Infrasturce as Code solution to deploy, such as AWS CDK.
4) Since we are in demo, please do not add any new feature for the application.
```

## 🚀 Exercise 9 -  Cleanup and Resource Management

```
Create and execute cleanup procedures:

- Document all deployed AWS resources
- Create cleanup scripts for complete resource removal
- Verify all resources are properly deleted
- Document any persistent data that needs backup
- Create re-deployment procedures for future use
```


