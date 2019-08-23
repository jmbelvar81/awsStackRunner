# AWS STACKS RUNNER
---

## Objective of the Proyect

Create a framework/set of tools to allow a "C-U-D" Model to manage the AWS Stacks using:

- AWS CLI [In Future migrate to Python or other SDK Programming Language]
- Separate Logic and Configuration  using Configuration Files [No More Fixed Params in Your Custom Scripts]
- Create a Simple Orchestrator Tool to Define the order to run several Stacks defined in the Framework Configuration


## Directories Structure

### *ORCHESTRATOR [WIP]* 

This directory contains:

  - order.config  : The file with the list of stacks to execute. (It defines the order used in the next script)
  - run-stacks.sh : This script execute the main script to deploy all  stacks indicated in the order.config list  

*Note*:

  - *Probably will be DEPRECATED and REPLACED for a Jenkinsfile Skeleton - Under Study*


### *Stacks Directories For Each APP - [ Under Stacks Main Directory]* 

Each Application/Stack  is configured under the Stacks main directory and it includes the next main directories, described in each case:

#### Params

Include the **params.json** file with the required parameters for the stack

**Note:** 
    - The region.json was deprecated. the value is included on additional.json in *Configs* directory

#### Tags

Include the **tags.json** with teh required tags to include in the stack

#### Configs

It contains the files with additional configuration to include:

  - additional: All options for create/update stack non included in other places but joined in a json file
  - rollback: The json with Rollback Policies as AWS support it for AWS CLI *[UNDER DEVELOPMENT - Non Used Currently]*

#### Templates

It contains the *Cloudformation templates* used to deploy the required stack.

*Notes:*

  - CoreStackName: This parameter is required in the main template to validate if it has dependencies of other Stack 
		   [Currently I'm studying change it to an array but in theory every Stack depends ONLY FROM ONE STACK DIRECTLY]

### DEPLOYMENT

This directory includes the next components:

  - **Scripts directory**: It contains the main script that execute the *"C-U-D" [Create-Update-Delete]* Scripts. 

Every script in *"CUD"* scripts , use the other described directories to locate the required params, templates, etc... depending of the given *"stack name"* in each case

### Example of *"Configuration using this project"* to *"manage(Create/Update/Delete/Orchestrate)"* two Cloudformation Stacks

```
.
├── deployment
│   └── scripts
│       ├── cptemplates.sh
│       ├── cstack.sh
│       ├── dstack.sh
│       ├── main.sh
│       └── ustack.sh
├── orchestrator
│   ├── order.config
│   └── run-stacks.sh
├── README.md
└── stacks
    └── mystack1
        ├── configs
        │   ├── additional.json
        │   └── rollback.json
        ├── params
        │   ├── params.json
        │   
        ├── tags
        │   └── tags.json
        └── templates
            └── master-cf.yaml
    └── mystack2
        ├── configs
        │   ├── additional.json
        │   └── rollback.json
        ├── params
        │   ├── params.json
        │   
        ├── tags
        │   └── tags.json
        └── templates
            └── master-cf.yaml

```

