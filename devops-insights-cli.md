---

copyright:
  years: 2019, 2020
lastupdated: "2020-06-24"

keywords: doi, devops insights, cli, plug-in

---

{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:external: target="_blank" .external}

# {{site.data.keyword.DRA_short}} CLI
{: #CLI_devops-insights}

The {{site.data.keyword.DRA_full}} CLI provides a set of commands that you can use to integrate your build with {{site.data.keyword.DRA_short}}. You must use two different types of commands: CLI usage commands and CLI commands to integrate with {{site.data.keyword.DRA_short}}.
{:shortdesc}


## Before you begin
{: #prerequisites}

* Install the {{site.data.keyword.cloud_notm}} CLI. See [Download {{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-getting-started) for instructions.

* Add the {{site.data.keyword.cloud_notm}} CLI plug-in. Run the following command:

```
ibmcloud plugin install doi
```

* Make sure you can access a toolchain with the {{site.data.keyword.DRA_short}} tool that is configured for that toolchain. For more information about toolchains, see [Creating a toolchain from an app](/docs/services/ContinuousDelivery?topic=ContinuousDelivery-toolchains_getting_started#creating_a_toolchain_from_an_app).  

* Specify the toolchain ID by using one of the following methods:
  - Specify the toolchain ID as a CLI parameter to the command.
  - Set the `TOOLCHAIN_ID` environment variable.
  - Your {{site.data.keyword.contdelivery_full}} pipeline might automatically set the `PIPELINE_TOOLCHAIN_ID` environment variable.

  The CLI needs the value of the toolchain ID. The value of toolchain ID that is specified in the CLI parameter overrides the value of environment variable.

  The toolchain ID is found in the toolchain URL that is shown in the browser. If you're using {{site.data.keyword.deliverypipelinelong}}, you can set the toolchain ID to send your build data to a different toolchain. For more information, see [Aggregating data from multiple sources into a single toolchain](/docs/ContinuousDelivery?topic=ContinuousDelivery-aggregating-multiple-sources).


## Login
{: #login}

Use this command to log in to {{site.data.keyword.cloud_notm}}. The API_KEY must have access to the toolchain.

```
ibmcloud login --apikey API_KEY
```

## CLI usage commands
{: #CLI-usage-commands}

### {{site.data.keyword.DRA_short}} help
{: #ibmcloud-help}

 The following command displays the list of {{site.data.keyword.DRA_short}} commands:

```
 ibmcloud doi --help
```

### {{site.data.keyword.DRA_short}} command help
{: #ibmcloud-command-help}

 The following command displays the details of flags that are needed for a command:

```
 ibmcloud doi <command> --help
```


## Commands to integrate with {{site.data.keyword.DRA_short}}
{: #commands-integrate-insights}

When you use the CLI for a build, you must publish a [build record](#publishbuildrecord). 

The value of the `logicalappname` and `buildnumber` parameters that are passed to the CLI must remain the same across all of the command invocations.

### Publishing a build record
{: #publishbuildrecord}

 The following command publishes a build record to {{site.data.keyword.DRA_short}}:

```
 ibmcloud doi buildrecord-publish --branch BRANCH --repositoryurl REPOSITORYURL --commitid COMMITID --status STATUS --logicalappname LOGICALAPPNAME --buildnumber BUILDNUMBER --toolchainid TOOLCHAINID [--joburl JOBURL]
```

The following are the command options for publishing a build record. 

| Command Options                       | Required or Optional | Description                                                                                                             |
|---------------------------------------|----------------------|-------------------------------------------------------------------------------------------------------------------------|
| <nobr>`-B`, `--branch`</nobr>         | Required             | The repository branch that the build is being performed.                                                                | 
| <nobr>`-R`, `--repositoryurl`</nobr>  | Required             | The URL of the Git repository.                                                                                          |
| <nobr>`-C`, `--commitid`</nobr>       | Required             | The Git commit ID.                                                                                                      |
| <nobr>`-S`, `--status`</nobr>         | Required             | The build status. Acceptable values: `pass` and `fail`.                                                                 |
| <nobr>`-L`, `--logicalappname`</nobr> | Required             | Name of the application.                                                                                                |
| <nobr>`-N`, `--buildnumber`</nobr>    | Required             | Any string that identifies the build.                                                                                   |
| <nobr>`-I`, `--toolchainid`</nobr>    | Required             | If the TOOLCHAIN_ID environment variable is set, this flag is optional. If both the environment variable and the flag are provided, the value of the flag overrides the value of the environment variable. |
| <nobr>`-J`, `--joburl`</nobr>         | Optional             | The URL to the job's build logs that is automatically set by the CLI in the {{site.data.keyword.deliverypipelinelong}}. |
{: caption="Table 1. Command options for publishing a build record" caption-side="top"}

#### Example
```
ibmcloud doi buildrecord-publish  -B master -R "https://github.com/oic/dlms.git" -C dff7884b9168168d91cb9e5aec78e93db0fa80d9 -S pass -L testapp -N master:199 -I b531487c-9c22-4f3b-9d20-5be408d57891
or
ibmcloud doi buildrecord-publish  --branch master --repositoryurl "https://github.com/oic/dlms.git" --commitid dff7884b9168168d91cb9e5aec78e93db0fa80d9 --status pass --logicalappname testapp --buildnumber master:199 --toolchainid b531487c-9c22-4f3b-9d20-5be408d57891
```

### Publishing a test record
{: #publishtestrecord}

 The following command publishes a test record to {{site.data.keyword.DRA_short}}:

```
 ibmcloud doi testrecord-publish --filelocation FILELOCATION --type TYPE --logicalappname LOGICALAPPNAME --buildnumber BUILDNUMBER --toolchainid TOOLCHAINID [--drilldownurl DRILLDOWNURL] [--env ENV] [--sqtoken SONARQUBE_TOKEN] 
```
The following are the command options for publishing test records. 

| Command Options                       | Required or Optional | Description                                                                                                                                     |
|---------------------------------------|----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------|
| <nobr>`-F`, `--filelocation`</nobr>   | Required             | Location of the results you want to upload. It can be a single file, entire directory, or several files that match a wildcard expression.       |
| <nobr>`-T`, `--type`</nobr>           | Required             | The type of test results that you want to upload.                                                                                               |
| <nobr>`-L`, `--logicalappname`</nobr> | Required             | Name of the application.                                                                                                                        |
| <nobr>`-N`, `--buildnumber`</nobr>    | Required             | Any string that identifies the build.                                                                                                           |
| <nobr>`-I`, `--toolchainid`</nobr>    | Required             | If the TOOLCHAIN_ID environment variable is set, this flag is optional. If both the environment variable and the flag are provided, the value of the flag overrides the value of the environment variable. |
| <nobr>`-U`, `--drilldownurl`</nobr>   | Optional             | A URL where more information about the test results can be found. If this URL is invalid, the option is ignored.                                | 
| <nobr>`-E`, `--env`</nobr>            | Optional             | The environment name to associate with the test results. This option is ignored for unit tests, code coverage tests, and static security scans. |
| <nobr>`-K`, `--sqtoken`</nobr>        | Optional             | This command is a SonarQube token. Valid only if the type specified is SonarQube. Used to pull more information from the SonarQube server.      |
{: caption="Table 2. Command options for publishing a build record" caption-side="top"}

#### Example

```
ibmcloud doi testrecord-publish -F "tests/fvt/*.json" -T fvt -L testapp -N master:199 -I b531487c-9c22-4f3b-9d20-5be408d57891
or
ibmcloud doi testrecord-publish --filelocation "tests/fvt/*.json" --type fvt --logicalappname testapp --buildnumber master:199 --toolchainid b531487c-9c22-4f3b-9d20-5be408d57891
```

The following test types are supported:

| Type                   | Description                                                           |
|------------------------|-----------------------------------------------------------------------|
| `unittest`             | Unit test results                                                     |
| `fvt`                  | Functional verification test (FVT) results                            |
| `code`                 | Code coverage results                                                 |
| `sonarqube`            | SonarQube scan results                                                |
| `staticsecurityscan`   | Static security scan results from IBM Application Security on Cloud   |
| `dynamicsecurityscan`  | Dynamic security scan results from IBM Application Security on Cloud  |
| `vulnerabilityadvisor` | Vulnerability Advisor results from IBM Vulnerability Advisor on Cloud |
{: caption="Table 3. Test record types" caption-side="top"}

### Publishing a deployment record
{: #publishdeployrecord}

 The following command publishes a deployment record to {{site.data.keyword.DRA_short}}:

```
 ibmcloud doi deployrecord-publish --env ENV --status STATUS --logicalappname LOGICALAPPNAME --buildnumber BUILDNUMBER --toolchainid TOOLCHAINID [--joburl JOBURL] [--appurl APPURL] 
```

| Command Options                       | Required or Optional | Description                                                                                                     |
|---------------------------------------|----------------------|-----------------------------------------------------------------------------------------------------------------|
| <nobr>`-E`, `--env`</nobr>            | Required             | The environment where the pipeline job deployed the app.                                                        |
| <nobr>`-S`, `--status`</nobr>         | Required             | The deployment status. This value must be either pass or fail.                                                  |
| <nobr>`-L`, `--logicalappname`</nobr> | Required             | Name of the application.                                                                                        |
| <nobr>`-N`, `--buildnumber`</nobr>    | Required             | Any string that identifies the build.                                                                           |
| <nobr>`-I`, `--toolchainid`</nobr>    | Required             | If the TOOLCHAIN_ID environment variable is set, this flag is optional. If both the environment variable and the flag are provided, the value of the flag overrides the value of the environment variable. |
| <nobr>`-A`, `--appurl`</nobr>         | Optional             | The URL where the deployed app is running.                                                                      |
| <nobr>`-J`, `--joburl`</nobr>         | Optional             | The URL to the job's build logs automatically set by the CLI in the {{site.data.keyword.deliverypipelinelong}}. |
{: caption="Table 4. Command options for publishing a deployment record" caption-side="top"}

#### Example

```
ibmcloud doi deployrecord-publish -E "staging" -S pass -L testapp -N master:199 -I b531487c-9c22-4f3b-9d20-5be408d57891
or
ibmcloud doi deployrecord-publish --env "staging" --status pass --logicalappname testapp --buildnumber master:199 --toolchainid b531487c-9c22-4f3b-9d20-5be408d57891
```


### Evaluating gates 
{: #evaluategate}

 The following command evaluates a {{site.data.keyword.DRA_short}} gate:

```
 ibmcloud doi gate-evaluate --policy POLICY --logicalappname LOGICALAPPNAME --buildnumber BUILDNUMBER --toolchainid TOOLCHAINID [--forcedecision] [--ruletype RULETYPE] 
```

The following are command options for evaluating gates:

| Command Options                       | Required or Optional | Description                                                                                                                                  |
|---------------------------------------|----------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| <nobr>`-P`, `--policy`</nobr>         | Required             | The name of the policy that the gate uses to make its decision.                                                                              |
| <nobr>`-L`, `--logicalappname`</nobr> | Required             | Name of the application.                                                                                                                     |
| <nobr>`-N`, `--buildnumber`</nobr>    | Required             | Any string that identifies the build.                                                                                                        |
| <nobr>`-I`, `--toolchainid`</nobr>    | Required             | If the TOOLCHAIN_ID environment variable is set, this flag is optional. If both the environment variable and the flag are provided, the value of the flag overrides the value of the environment variable. |
| <nobr>`-D`, `--forcedecision`</nobr>  | Optional             | Set the value to true to exit with an error code if the policy evaluation fails. The value defaults to false if this option isn't specified. |
| <nobr>`-E`, `--ruletype`</nobr>       | Optional             | A rule type to consider. If you include this option, only rules of this type are considered in the decision-making process.                  |
{: caption="Table 5. Command options for evaluating gates" caption-side="top"}

#### Example

```
ibmcloud doi gate-evaluate -P "policyname" -D true -L testapp -N master:199 -I b531487c-9c22-4f3b-9d20-5be408d57891
or
ibmcloud doi gate-evaluate --policy "policyname" --forcedecision true --logicalappname testapp --buildnumber master:199 --toolchainid b531487c-9c22-4f3b-9d20-5be408d57891
```

### Updating custom data sets and policies
{: #updatepolicies}

 The following command creates and updates custom data sets and policies for a toolchain:

```
 ibmcloud doi policies-update --file FILELOCATION --toolchainid TOOLCHAINID [--dryrun]
```

The following are command options for updating custom data sets and policies:

| Command Options                       | Required or Optional | Description                                                                                                                                  |
|---------------------------------------|----------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| <nobr>`-F`, `--file`</nobr>           | Required             | The location of the JSON file that contains the list of custom data sets and policies to add or update. Both absolute and relative paths are accepted. |
| <nobr>`-I`, `--toolchainid`</nobr>    | Required             | If the TOOLCHAIN_ID environment variable is set, this flag is optional. If both the environment variable and the flag are provided, the value of the flag overrides the value of the environment variable. |
| <nobr>`-D`, `--dryrun`</nobr>         | Optional             | The option to simulate only the changes, with no updates. |                 |
{: caption="Table 6. Command options for updating custom data sets and policies" caption-side="top"}

#### Example

```
ibmcloud doi policies-update -F "policies/policy.json" -I b531487c-9c22-4f3b-9d20-5be408d57891
or
ibmcloud doi policies-update --file "policies/policy.json" --toolchainid b531487c-9c22-4f3b-9d20-5be408d57891
```

#### JSON file structure for the `updatepolicies` command

A valid JSON file structure contains two fields:

```
{
      "custom_datasets": [],
      "policies": []
}
```

* You can specify any number of policies (and custom data sets) for the array.
* If the specified policy (and custom data set) already exists for a toolchain, the policy is updated or created.
* Either the `custom_datasets` or `policies` array can be empty, or both can be empty.
* The only valid values for a `type_of_test` custom data set are `test` and `code`.
* If a custom data set exists for a toolchain, it can be used in the policy rules that are defined within the JSON file. You are not always required to define the custom data set within the JSON file.
* The sample JSON file that is provided for the `policies-update` command lists all of the possible rule types that you can specify in a policy. All of the fields in those rules are required.
* Use only one rule per data set.
* The name field within a rule is optional.

#### Sample JSON file for the `policies-update` command

This sample JSON file contains two custom data sets and two policies. The first policy, `name: "Orders"`, contains all of the rule types that you can use within a policy.

```
{
  "custom_datasets": [
 .  {
      "lifecycle_stage": "integrationtest",
      "type_of_test": "test",
      "label": "Integration Test"
    },
    {
      "lifecycle_stage": "covtest",
      "type_of_test": "code",
      "label": "Coverage Test"
    }
  ],
  "policies": [
    {
      "name": "Orders",
      "description": "Composite Policy.",
      "rules": [
        {
          "name": "rule1",
 .        "description": "Unit Test Rule with regression",
          "stage": "unittest",
          "percentPass": 100,
          "criticalTests": [
            "Get Weather with incomplete zip code"
          ],
          "regressionCheck": true
        },
        {
          "name": "rule2",
          "description": "Unit Test Rule without regression",
          "stage": "integrationtest",
          "percentPass": 98,
          "criticalTests": [
            "'Get Weather with incomplete zip code'"
          ],
        },
        {
          "name": "rule3",
          "description": "Functional test Rule",
          "stage": "fvt",
          "percentPass": 98,
          "criticalTests": [
            "'Get Weather with incomplete zip code'"
          ],
        },
        {
          "name": "rule4",
          "description": "Code Coverage rule",
          "stage": "code",
          "codeCoverage": 98,
        },
        {
          "name": "rule5",
          "description": "Custom dataset rule",
          "stage": "covtest",
          "codeCoverage": 60,
        },
        {
          "name": "rule6",
          "description": "Static Security Scan rule",
          "stage": "staticsecurityscan",
          "highSeverity": 40,
          "mediumSeverity": 5,
          "lowSeverity": 9
        },
        {
          "name": "rule7",
         "description": "Dynamic Security Scan rule",
          "stage": "dynamicsecurityscan",
          "highSeverity": 40,
          "mediumSeverity": 5,
          "lowSeverity": 9
        },
        {
          "name": "rule8",
          "description": "Sonarqube rule",
          "stage": "sonarqube"
        },
        {
          "name": "rule9",
          "description": "Vulnerability rule",
          "stage": "vulnerabilityadvisor"
        }
      ]
    },
    {
      "name": "UI",
      "description": "Policy to check Unit Test.",
      "rules": [
        {
          "name": "Unit Test Rule",
          "description": "Unit Test Rule",
          "stage": "integrationtest",
          "percentPass": 100,
          "criticalTests": []
        }
      ]
    }
  ]
}
```

## FAQs
{: #faq}

 Get answers to frequently asked questions about using the {{site.data.keyword.DRA_short}} CLI.
 
 ### Why does the CLI fail with the "You do not have access to the toolchain" message?
  
 The `API_KEY` environment variable that is used to log in to {{site.data.keyword.cloud_notm}} must be able to access the toolchain. Also, verify that you added the {{site.data.keyword.DRA_short}} tool integration to your toolchain.

 ### The CLI successfully ran, why doesn't the data show up on the dashboard?
 
 Make sure that the value of the `logicalappname` and `buildnumber` parameters that are passed to the CLI are the same across all of the stages for the build. Also, verify that a build record is uploaded for the build. The data for test records that are uploaded for a specific build does not appear on the dashboard without a build record.

 ### How can I determine why the CLI failed?
 
 Before you call the {{site.data.keyword.DRA_short}} CLI, set the `IBMCLOUD_TRACE` environment variable to true to turn on the debug log.
      
  ```
    export IBMCLOUD_TRACE=true
  ```
      
Observe the API calls and the responses that are shown in the log to determine the exact reason for failure.
