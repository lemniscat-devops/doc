The manifest file is a YAML file that contains the definition of the product to deploy. It contains the following sections:

- [**variables**](#variables): The variables needed to deploy the product.
- [**capabilities**](#capabilities): The capabilities can be activated during the product deployment.
- [**requirements**](#requirements): The plugins needed to execute tasks describe in the manifest file.

## Variables

The variables are the static parameters needed to deploy the product. They are defined in the manifest file, and can be used in the tasks to execute.
For example, you can define a variable to define the name of the product, and use this variable in the tasks to create the product.

The variables are defined in the `variables` section of the manifest file, and are defined as a dictionary with the following structure:

```yaml
variables:
  - name: location
    value: West Europe
  ...
```

You can define in your variables a value based on another variablse. For example, you can define a variable `location` based on the environment variable `location`.
To do that, you can use the following syntax:

```yaml
variables:
  - name: location
    value: ${{ environment_location }}
  ...
```

You can also concatenate variables to define a new variable. For example, you can define a variable `resourceGroupId` based on the subscriptionId and the resource group name.

```yaml
variables:
  - name: resourceGroupId
    value: '/subscriptions/${{ env_subscriptionId }}/resourceGroups/${{ rgName }}'
  ...
```

## Pre and Post phases

The pre and post phases are the steps that need to be executed before and after the capabilities. They are defined in the manifest file, and can be used to execute tasks before and after the capabilities.

The pre and post phases are defined in the `pre` and `post` sections of the manifest file, and are defined as a dictionary with the following structure:

```yaml
pre:
  tasks:
    - task: powershell
      displayName: 'Transform user email to user trigram'
      steps: 
        - run
      parameters:
        type: inline
        script: |
          $reg = "${{ developerEmail }}" | Select-String -Pattern "${{ regex_developerEmail }}"
          $trigram = "$($reg.Matches.Groups[1])$($reg.Matches.Groups[2])"
          Write-Host "[lemniscat.pushvar] trigram=$trigram"
...
post:
  tasks:
    - task: powershell
      displayName: 'Generate backstage catalog-info'
      steps: 
        - run
      parameters:
        type: inline
        script: |
          python -m pip install Jinja2
          python ${{ templatePath }}/scripts/backstage/catalog-info-render.py "$($result.path)/vars.json" "${{ backstagePath }}/catalog-info/catalog-info.j2" "${{ backstagePath }}/catalog-info/catalog-info.yaml"
      storeVariablesInFile:
        format: json
        withSecrets: true
```

### Definition

You can define as many tasks as you want for the pre and post phases. For example, you can define a task to transform the user email to the user trigram in the pre phase, and a task to generate the backstage catalog-info in the post phase.
You can also define templates for your tasks in the pre and post phases.
During the execution of the runtime, the tasks are executed in the same order as defined in the manifest file.

## Capabilities

The capabilities are the DevOps steps that can be activated during the deployment of a product. It's designed to be sure that all the DevOps aspects are covered during the design of a product.
For each capability, you can define the [solutions](#solutions) that need to be executed to activate the capability.
For example, for capability code you can define Github and Gitlab as [solutions](#solutions) to activate the capability when the product is deployed.

For each capability, you can define the other capability that need to be executed before. For example, you can define the `operate` capability to be executed before the `code` capability.
To do that ou must define the `dependsOn` parameter in the capability definition. `dependsOn` is a list of capabilities that need to be executed before the current capability. This parameter is optional.

The capabilities are defined in the `capabilities` section of the manifest file, and are defined as a dictionary with the following structure:

```yaml
capabilities:
  code:
    dependsOn:
    - operate
    solutions:
    - solution: github
      ...
    - solution: gitlab
      ...
  build:
    solutions:
    - solution: jenkins
      ...
    - solution: azuredevops
      ...
  test:
    solutions:
    - solution: sonarqube
      ...
    - solution: jenkins
      ...
  release:
    solutions: 
    - solution: artifactory
      ...
    - solution: azure-container-registry
      ...
  deploy:
    solutions:
    - solution: azuredevops
      ...
    - solution: argocd
      ...
  operate:
    solutions:
    - solution: azure
      ...
    - solution: aws
      ...
  monitor:
    solutions:
    - solution: grafana
      ...
    - solution: datadog
      ...
  plan:
    solutions:
    - solution: Jira
      ...
    - solution: PagerDuty
      ...
```

### Definition

Here, the list of capabilities that you can define in the manifest file:

- `code`: The capability to manage the code. For example, you can define the solutions to create a git repository, add collaborators, clone the repository, ...
- `build`: The capability to build. For example, you can define the solutions to build with Jenkins, Azure DevOps, ...
- `test`: The capability to test. For example, you can define the solutions to test with SonarQube, Jenkins, ...
- `release`: The capability to release. For example, you can define the solutions to release with Artifactory, Azure Container Registry, ...
- `deploy`: The capability to deploy. For example, you can define the solutions to deploy with Azure DevOps, ArgoCD, ...
- `operate`: The capability to operate. For example, you can define the solutions to operate with Azure, AWS, ...
- `monitor`: The capability to monitor. For example, you can define the solutions to monitor with Grafana, Datadog, ...
- `plan`: The capability to plan. For example, you can define the solutions to plan with Jira, PagerDuty, ...

Of course, you don't have to define all the capabilities in the manifest file. You can define only the capabilities that you need to or can activate during the deployment of the product.

You can't define the same capability twice in the manifest file. If you define the same capability twice, the runtime will raise an error.
You can't define a capability that is not in the list above. If you define a capability that is not in the list above, the runtime will raise an error.

## Solutions

The solutions are the tools that can be used to execute the [capabilities](#capabilities). For example, you can use Jenkins to execute the build capability, or you can use Ansible to execute the deployment capability.
For each solution, you can define a workflow with the [tasks](#tasks) that need to be executed to activate the capability.
For example, for Azure (in operate capability), you can define the [tasks](#tasks) that need to be executed to deploy infrastructure with Terraform.

The solutions are defined in the capability section of the manifest file, and are defined as a dictionary with the following structure:

```yaml
capabilities:
  code :
    solutions:
    - solution: github
      tasks:
        - task: github
          steps: 
            - run
          parameters:
            action: createRepository
            name: ${{ product_name }}
            description: ${{ product_description }}
            visibility: ${{ domain_visibility }}
            organization: ${{ domain_organization }}
            token: ${{ github_token }}
            ...
    - solution: gitlab
      tasks:
        - task: gitlab
          steps: 
            - run
          parameters:
            action: createRepository
            name: ${{ product_name }}
            description: ${{ product_description }}
            visibility: ${{ domain_visibility }}
            organization: ${{ domain_organization }}
            token: ${{ gitlab_token }}
            ...
  ...
```	

### Definition

You can define as many solutions as you want for a capability. For example, you can define Github and Gitlab as solutions for the code capability.
You can't define the same solution twice for a capability. If you define the same solution twice for a capability, the runtime will raise an error.

To define a solution, you need to define the following parameters:

- `solution: <solutionName>`, with `<solutionName>` the name of the solution to use to activate the capability.

For each solution, you can define the [tasks](#tasks) that need to be executed to activate the capability. The tasks are defined in the `tasks` section of the solution, and are defined as a dictionary with the following structure:

```yaml
solutions:
- solution: github
  tasks:
    ...
```

## Tasks

The tasks are the actions that need to be executed to activate the capability. For example, you can define a task to execute a script, or a task to execute a terraform command.
For each task, you need to tag in witch [step](#step-concept) it needs to be executed, and the parameters that need to be used to execute the task.
You can define many tags for a task, and the task will be executed in the same step as the tag.
In the same step, the tasks are executed in the same order as defined in the manifest file.

The tasks are defined in the [solution](#solutions) section of the manifest file, and are defined as a dictionary with the following structure:

```yaml
capabilities:
  code :
    solutions:
    - solution: github
      tasks:
        - task: github
          displayName: 'Create repository'
          steps: 
            - run
          parameters:
            action: createRepository
            name: ${{ product_name }}
            description: ${{ product_description }}
            visibility: ${{ domain_visibility }}
            organization: ${{ domain_organization }}
            token: ${{ github_token }}
            ...
        - task: github
          displayName: 'Add collaborators'
          steps: 
            - run
          parameters:
            action: addCollaborators
            name: ${{ product_name }}
            collaborators: ${{ product_collaborators }}
            token: ${{ github_token }}
        - task: github
          displayName: 'clone repository'
          steps: 
            - run
          parameters:
            action: clone
            name: ${{ product_name }}
            token: ${{ github_token }}
        - task: copy
          displayName: 'Copy sample code'
          steps: 
            - pre
          parameters:
            source: ${{ product_source }}
            destination: ${{ currentpath }}/${{ product_name }}
        - task: git
          displayName: 'Add files'
          steps: 
            - pre
          parameters:
            action: add
            path: ${{ currentpath }}/${{ product_name }}
        - task: git
          displayName: 'Commit files'
          steps: 
            - pre
          parameters:
            action: commit
            path: ${{ currentpath }}/${{ product_name }}
            message: 'Initial commit'
        - task: git
          displayName: 'Push files'
          steps: 
            - pre
          parameters:
            action: push
            path: ${{ currentpath }}/${{ product_name }}
            token: ${{ github_token }}
      ...
```

In order to factorize tasks in your solution, you can define templates for your tasks. For example, you can define a template to create and initialze a repository.
To do that, you can use the following syntax in your manifest file:

```yaml
capabilities:
  code :
    - solutions: github
      tasks:
        - template: ${{ templates_path }}/createRepository.yaml
          displayName: 'Create and initialize repository'
```

And in the `createRepository.yaml` file, you can define the following tasks:

```yaml
tasks:
  - task: github
    displayName: 'Create repository'
    steps: 
    - run
    parameters:
    action: createRepository
    name: ${{ product_name }}
    description: ${{ product_description }}
    visibility: ${{ domain_visibility }}
    organization: ${{ domain_organization }}
    token: ${{ github_token }}
    ...
  - task: github
    displayName: 'Add collaborators'
    steps: 
    - run
    parameters:
    action: addCollaborators
    name: ${{ product_name }}
    collaborators: ${{ product_collaborators }}
    token: ${{ github_token }}
  - task: github
    displayName: 'clone repository'
    steps: 
    - run
    parameters:
    action: clone
    name: ${{ product_name }}
    token: ${{ github_token }}
  - task: copy
    displayName: 'Copy sample code'
    steps: 
    - pre
    parameters:
    source: ${{ product_source }}
    destination: ${{ currentpath }}/${{ product_name }}
  - task: git
    displayName: 'Add files'
    steps: 
    - pre
    parameters:
    action: add
    path: ${{ currentpath }}/${{ product_name }}
  - task: git
    displayName: 'Commit files'
    steps: 
    - pre
    parameters:
    action: commit
    path: ${{ currentpath }}/${{ product_name }}
    message: 'Initial commit'
  - task: git
    displayName: 'Push files'
    steps: 
    - pre
    parameters:
    action: push
    path: ${{ currentpath }}/${{ product_name }}
    token: ${{ github_token }}
```

### Definition

You can define as many tasks as you want for a solution. For example, you can define a task to create a repository, a task to add collaborators, a task to deploy the infrastructure, ...
During the execution of the runtime, the tasks are executed in the same order as defined in the manifest file.

To define a task, you need to define the following parameters:

- `task: <pluginName>`, with `<pluginName>` the name of the plugin to execute. This parameter is mandatory.
- `displayName: <displayName>`, with `<displayName>` the name of the task to display in the logs. This parameter is optional.
- `steps: <steps>`, with `<steps>` the list of steps where the task needs to be executed. This parameter is mandatory.
  For example, you can define `steps: ['pre', 'run']` to execute the task in the pre and run steps.
- `parameters: <parameters>`, with `<parameters>` the parameters needed to execute the task. This parameter is mandatory.
- `condition: <condition>`, with `<condition>` the condition to execute the task. This parameter is optional. The condition is a boolean expression that needs to be true to execute the task. The condition can be based on the variables defined in the manifest file. The condition engine used is based on `simple_eval` library. So you use a limited set of operators and functions described in the [simple_eval documentation](https://pypi.org/project/simpleeval/).
  For example, you can define `condition: "${{ skipGitRepositoryCreation }} == False"` to execute the task only if the product name is not empty.

You can also define a template for your tasks. For example, you can define a template to create and initialze a repository.

To define a template, you need to define the following parameters:

- `template: <templatePath>`, with `<templatePath>` the path to the template file to use. This parameter is mandatory.
- `displayName: <displayName>`, with `<displayName>` the name of the task to display in the logs. This parameter is optional.

## Requirements

The requirements are the plugins needed to execute tasks describe in the manifest file. They are defined in the `requirements` section of the manifest file, and are defined as a dictionary with the following structure:

```yaml
requirements:
  - name: lemniscat.plugin.azurecli
    version: 1.0.0
  - name: lemniscat.plugin.github
    version: 1.0.0
    ...
  ...
```
