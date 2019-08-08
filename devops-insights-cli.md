---

copyright:
  years: 2019
lastupdated: "2019-08-08"

keywords: doi, devops insights, cli, plug-in

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:pre: .pre}
{:table: .aria-labeledby="caption"}

# {{site.data.keyword.DRA_short}} CLI
{: #CLI_devops-insights}

The {{site.data.keyword.DRA_full}} CLI provides a set of commands that can be used to integrate your build with {{site.data.keyword.DRA_short}} (doi). There are two different commands that you must use: CLI usage commands and CLI commands to integrate with {{site.data.keyword.DRA_short}}.
{:shortdesc}


## Before you begin
{: #prerequisites}

* Install the {{site.data.keyword.Bluemix_notm}} CLI. See [Download {{site.data.keyword.Bluemix_notm}} CLI ![External link icon](../icons/launch-glyph.svg)](http://plugins.ng.bluemix.net/ui/home.html){: new_window} for instructions.

* Add the {{site.data.keyword.Bluemix_notm}} CLI plug-in. Run the following command:

```
ibmcloud plugin install doi
```

* Access to a toolchain with the {{site.data.keyword.DRA_short}} tool configured for that toolchain. For more information about toolchains, see [Creating a toolchain from an app](/docs/services/ContinuousDelivery?topic=ContinuousDelivery-toolchains_getting_started#creating_a_toolchain_from_an_app).  

* Set the `TOOLCHAIN_ID` as the environment variable to make it the toolchain's GUID. The toolchain ID is found in the toolchain URL that is shown in the browser. If you're using {{site.data.keyword.deliverypipelinelong}}, you set the `TOOLCHAIN_ID` environment variable to send your build data to a different toolchain. For more information, see [Aggregating data from multiple sources into a single toolchain](/docs/services/DevOpsInsights?topic=DevOpsInsights-aggregating-multiple-sources).


## Login
{: #login}

Use this command to log in to {{site.data.keyword.Bluemix_notm}}. The API_KEY must have access to the toolchain.

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

The value of `logicalappname` and `buildnumber` parameter that is passed to the CLI need to remain the same across all the command invocations.

### Publishing a build record
{: #publishbuildrecord}

 The following command publishes a build record to {{site.data.keyword.DRA_short}}:

```
 ibmcloud doi publishbuildrecord --branch BRANCH --repositoryurl REPOSITORYURL --commitid COMMITID --status STATUS --logicalappname LOGICALAPPNAME --buildnumber BUILDNUMBER [--joburl JOBURL]
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
| <nobr>`-J`, `--joburl`</nobr>         | Optional             | The URL to the job's build logs that is automatically set by the CLI in the {{site.data.keyword.deliverypipelinelong}}. |
{: caption="Table 1. Command options for publishing a build record" caption-side="top"}

 <!--
<dl>
   <dt>-B, --branch</dt>
   <dd>Required, The repository branch on which the build is being performed.</dd>
   <dt>-R, --repositoryurl</dt>
   <dd>Required, The url of the Git repository.</dd>
   <dt>-C, --commitid</dt>
   <dd>Required, The Git commit ID.</dd>
   <dt>-S, --status</dt>
   <dd>Required, The build status. Acceptable values: pass/fail.</dd>
   <dt>-L, --logicalappname</dt>
   <dd>Required, Name of the application.</dd>
   <dt>-N, --buildnumber</dt>
   <dd>Required, Any string that identifies the build.</dd>
   <dt>-J, --joburl</dt>
   <dd>Optional, The url to the job's build logs. This value is automatically set by the CLI in IBM Continuous Delivery pipeline.</dd>
</dl>
-->

**Example**:
```
ibmcloud doi publishbuildrecord  -B master -R "https://github.com/oic/dlms.git" -C dff7884b9168168d91cb9e5aec78e93db0fa80d9 -S pass -L testapp -N master:199
or
ibmcloud doi publishbuildrecord  --branch master --repositoryurl "https://github.com/oic/dlms.git" --commitid dff7884b9168168d91cb9e5aec78e93db0fa80d9 --status pass --logicalappname testapp --buildnumber master:199
```

### Publishing test records
{: #publishtestrecord}

 The following command publishes a test record to {{site.data.keyword.DRA_short}}:

```
 ibmcloud doi publishtestrecord --filelocation FILELOCATION --type TYPE --logicalappname LOGICALAPPNAME --buildnumber BUILDNUMBER [--drilldownurl DRILLDOWNURL] [--env ENV] [--sqtoken SONARQUBE_TOKEN] 
```
The following are the command options for publishing test records. 

| Command Options                       | Required or Optional | Description                                                                                                                                     |
|---------------------------------------|----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------|
| <nobr>`-F`, `--filelocation`</nobr>   | Required             | Location of the results you want to upload. It can be a single file, entire directory, or several files that match a wildcard expression.       |
| <nobr>`-T`, `--type`</nobr>           | Required             | The type of test results that you want to upload.                                                                                               |
| <nobr>`-L`, `--logicalappname`</nobr> | Required             | Name of the application.                                                                                                                        |
| <nobr>`-N`, `--buildnumber`</nobr>    | Required             | Any string that identifies the build.                                                                                                           |
| <nobr>`-U`, `--drilldownurl`</nobr>   | Optional             | A URL where more information about the test results can be found. If this URL is invalid, the option is ignored.                                | 
| <nobr>`-E`, `--env`</nobr>            | Optional             | The environment name to associate with the test results. This option is ignored for unit tests, code coverage tests, and static security scans. |
| <nobr>`-K`, `--sqtoken`</nobr>        | Optional             | This is a SonarQube token. Valid only if the type specified is SonarQube. Used to pull more information from the SonarQube server.              |
{: caption="Table 2. Command options for publishing a build record" caption-side="top"}

<!--
<dl>
   <dt>-F, --filelocation</dt>
   <dd>Required, The location of the test results that you want to upload. It can be a single file, an entire directory, or multiple files that match a wildcard expression.</dd>
   <dt>-T, --type</dt>
   <dd>Required, The type of test results that you want to upload. The value can be code for code coverage, **unittest** for unit tests, **fvt** for FVT tests, **staticsecurityscan** for static security scans, **dynamicsecurityscan** for dynamic app scans, **sonarqube** for SonarQube scans, and **vulnerabilityadvisor** for Vulnerability Advisor scans. In addition to these types, you can also use a custom quality data set tag to upload test results of different type.</dd>
   <dt>-L, --logicalappname</dt>
   <dd>Required, Name of the application.</dd>
   <dt>-N, --buildnumber</dt>
   <dd>Required, Any string that identifies the build.</dd>
   <dt>-U, --drilldownurl</dt>
   <dd>Optional, A URL where more information about the test results can be found. If this URL is invalid, the option is ignored.</dd>
   <dt>-E, --env</dt>
   <dd>Optional, The environment name to associate with the test results. This option is ignored for unit tests, code coverage tests, and static security scans.</dd>
   <dt>-K, --sqtoken</dt>
   <dd>Optional, This is a SonarQube token. This flag is valid only if the type specified is sonarqube. It is used by the CLI to pull additional information from the SonarQube server.</dd> 
</dl>
-->

**Example**:

```
ibmcloud doi publishtestrecord -F "tests/fvt/*.json" -T fvt -L testapp -N master:199
or
ibmcloud doi publishtestrecord --filelocation "tests/fvt/*.json" --type fvt --logicalappname testapp --buildnumber master:199
```

The following test types are supported:

| Type                  | Description                                                          |
|-----------------------|----------------------------------------------------------------------|
| `unittest`            | Unit test results                                                    |
| `fvt`                 | Functional verification test (FVT) results                           |
| `code`                | Code coverage results                                                |
| `sonarqube`           | SonarQube scan results                                               |
| `staticsecurityscan`  | Static security scan results from IBM Application Security on Cloud  |
| `dynamicsecurityscan` | Dynamic security scan results from IBM Application Security on Cloud |
| `vulnerabilityadvisor`| Vulnerability Advisor results from IBM Vulnerability Advisor on Cloud|
{: caption="Table 3. Test record types" caption-side="top"}

### Publishing a deployment record
{: #publishdeployrecord}

 The following command publishes a deployment record to {{site.data.keyword.DRA_short}}:

```
 ibmcloud doi publishdeployrecord --env ENV --status STATUS --logicalappname LOGICALAPPNAME --buildnumber BUILDNUMBER [--joburl JOBURL] [--appurl APPURL] 
```

| Command Options                       | Required or Optional | Description                                                                                                            |
|---------------------------------------|----------------------|------------------------------------------------------------------------------------------------------------------------|
| <nobr>`-E`, `--env`</nobr>            | Required             | The environment where the pipeline job deployed the app.                                                               |
| <nobr>`-S`, `--status`</nobr>         | Required             | The deployment status. This value must be either pass or fail.                                                         |
| <nobr>`-L`, `--logicalappname`</nobr> | Required             | Name of the application.                                                                                               |
| <nobr>`-N`, `--buildnumber`</nobr>    | Required             | Any string that identifies the build.                                                                                  |
| <nobr>`-A`, `--appurl`</nobr>         | Optional             | The URL where the deployed app is running.                                                                             |
| <nobr>`-J`, `--joburl`</nobr>         | Optional             | The URL to the job's build logs automatically set by the CLI in the {{site.data.keyword.deliverypipelinelong}}. |
{: caption="Table 4. Command options for publishing a deployment record" caption-side="top"}

<!--
<dl>
   <dt>-E, --env</dt>
   <dd>Required, The environment where the pipeline job deployed the app.</dd>
   <dt>-S, --status</dt>
   <dd>Required, The deployment status. This value must be either pass or fail.</dd>  
   <dt>-L, --logicalappname</dt>
   <dd>Required, Name of the application.</dd>
   <dt>-N, --buildnumber</dt>
   <dd>Required, Any string that identifies the build.</dd>
   <dt>-J, --joburl</dt>
   <dd>Optional, The URL to the job's deployment logs. This value is automatically set by the CLI in IBM Continuous Delivery pipeline.</dd>
   <dt>-A, --appurl</dt>
   <dd>Optional, The URL where the deployed app is running.</dd>
</dl>
-->

**Example**:

```
ibmcloud doi publishdeployrecord -E "staging" -S pass -L testapp -N master:199
or
ibmcloud doi publishdeployrecord --env "staging" --status pass --logicalappname testapp --buildnumber master:199
```


### Evaluating gates 
{: #evaluategate}

 The following command evaluates a {{site.data.keyword.DRA_short}} gate:

```
 ibmcloud doi evaluategate --policy POLICY --logicalappname LOGICALAPPNAME --buildnumber BUILDNUMBER [--forcedecision] [--ruletype RULETYPE] 
```

The following are command options for evaluating gates:

| Command Options | Required or Optional | Description |
|-----------------|----------------------|-------------|
| <nobr>`-P`, `--policy`</nobr> | Required | The name of the policy that the gate uses to make its decision. |
| <nobr>`-L`, `--logicalappname`</nobr> | Required | Name of the application. |
| <nobr>`-N`, `--buildnumber`</nobr> | Required | Any string that identifies the build. |
| <nobr>`-D`, `--forcedecision`</nobr> | Optional | Set the value to true to exit with an error code if the policy evaluation fails. The value defaults to false if this option isn't specified. |
| <nobr>`-E`, `--ruletype`</nobr> | Optional | A rule type to consider. If you include this option, only rules of this type are considered in the decision-making process. |
{: caption="Table 5. Command options for evaluating gates" caption-side="top"}

<!--
<dl>
   <dt>-P, --policy</dt>
   <dd>Required, The name of the policy that the gate uses to make its decision.</dd>
   <dt>-L, --logicalappname</dt>
   <dd>Required, Name of the application.</dd>
   <dt>-N, --buildnumber</dt>
   <dd>Required, Any string that identifies the build.</dd>
   <dt>-D, --forcedecision</dt>
   <dd>Optional, Set the value to true to cause the process to exit with an error code, if the policy evaluation fails. The value defaults to false if this option is not specified.</dd>
   <dt>-E, --ruletype</dt>
   <dd>Optional, A rule type to consider. If you include this option, only rules of this type are considered in the decision-making process. The value can be code for code coverage, unittest for unit tests, fvt for FVT tests, staticsecurityscan for static security scans, dynamicsecurityscan for dynamic app scans, sonarqube for SonarQube scans, and vulnerabilityadvisor for Vulnerability Advisor scans.</dd>  
</dl>
-->

**Example**:
```
ibmcloud doi evaluategate -P "policyname" -D true -L testapp -N master:199
or
ibmcloud doi evaluategate --policy "policyname" --forcedecision true --logicalappname testapp --buildnumber master:199
```
