---

copyright:
  years: 2019
lastupdated: "2019-02-07"

keywords: doi

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

The {{site.data.keyword.DRA_full}} CLI provides a set of commands that can be used to integrate your build with {{site.data.keyword.DRA_short}} (doi). 
{:shortdesc}


## Prerequisites
{: #prerequisites}

* Before you begin, install the {{site.data.keyword.Bluemix_notm}} CLI. See [Download {{site.data.keyword.Bluemix_notm}} CLI ![External link icon](../icons/launch-glyph.svg)](http://plugins.ng.bluemix.net/ui/home.html){: new_window} for instructions.

* Adding the {{site.data.keyword.Bluemix_notm}} CLI plug-in

To install the IBM Cloud DevOps Insights CLI plugin, run the following command:
```
ibmcloud plugin install doi
```

* You should have access to a toolchain with the DevOps Insights tool configured for that toolchain. 

* The following environment variables should be set.

| Environment Variable | Description                                                                        | 
|----------------------|------------------------------------------------------------------------------------|
| LOGICAL_APP_NAME     | The application name.                                                              | 
| TOOLCHAIN_ID         | The toolchain's GUID. This can be found in the toolchain url shown in the browser. | 
| BUILD_NUMBER         | Any string that uniquely identifies the build.                                     |
{: caption="Table 1. Environment variables" caption-side="top"}


## login
{: #login}

Use this command to log in to {{site.data.keyword.Bluemix_notm}}. 

```
cloud-cli login [userID] [pw]
```


## Commands
{: #commands}

There are two different commands that you must use.  

1. 
<table summary="CLI usage commands">
    <thead>
        <th colspan="5">CLI usage commands</th>
    </thead>
    <tbody>
        <tr> 
            <td>[ibmcloud doi help](#help)</td> 
        </tr> 
        <tr> 
            <td>[ibmcloud doi command help](#detailhelp)</td> 
        </tr>
    </tbody> 
 </table> 

2. 
<table summary="CLI commands to integrate with DevOps Insights">
    <thead>
        <th colspan="5">CLI commands to integrate with DevOps Insights</th>
    </thead>
    <tbody>
        <tr> 
            <td>[ibmcloud doi publishbuildrecord](#publishbuildrecord)</td> 
        </tr>
        <tr>
            <td>[ibmcloud doi publishtestrecord](#publishtestrecord)</td> 
        </tr>
        <tr>
            <td>[ibmcloud doi publishdeployrecord](#publishdeployrecord)</td>
        </tr>
        <tr>
            <td>[ibmcloud doi evaluategate](#evaluategate)</td>
        </tr> 
    </tbody> 
 </table> 


## CLI usage commands
{: #CLI-usage-commands}

### ibmcloud doi help
{: #ibmcloud-help}
 This command displays the list of doi commands
```
 ibmcloud doi help
```

### ibmcloud doi command help
{: #ibmcloud-command-help}
 This command displays the details of flags needed for a given command
```
 ibmcloud doi <command> help
```


## CLI commands to integrate with DevOps Insights
{: #CLI-command-integrate-insights}

### ibmcloud doi publishbuildrecord
{: #publishbuildrecord}

 This command publishes a build record to DevOps Insights

```
 ibmcloud doi publishbuildrecord --branch BRANCH --repositoryurl REPOSITORYURL --commitid COMMITID --status STATUS [--joburl JOBURL] [--buildnumber BUILDNUMBER]
```

**Prerequisites**: [Prerequisites](#prerequisites), ibmcloud login

**Command Options**:
<dl>
   <dt>-B, --branch</dt>
   <dd>Required, The repository branch on which the build is being performed.</dd>
   <dt>-R, --repositoryurl</dt>
   <dd>Required, The url of the git repository.</dd>
   <dt>-C, --commitid</dt>
   <dd>Required, The git commit id.</dd>
   <dt>-S, --status</dt>
   <dd>Required, The build status. Acceptable values: pass/fail.</dd>
   <dt>-J, --joburl</dt>
   <dd>Optional, The url to the job's build logs.</dd>
   <dt>-N, --buildnumber</dt>
   <dd>Optional, Any string that identifies the build. This option overrides the value provided by BUILD_NUMBER environment variable.</dd>
</dl>

**Example**:
```
ibmcloud doi publishbuildrecord  -B master -R "https://github.com/oic/dlms.git" -C dff7884b9168168d91cb9e5aec78e93db0fa80d9 -S pass
or
ibmcloud doi publishbuildrecord  --branch master --repositoryurl "https://github.com/oic/dlms.git" --commitid dff7884b9168168d91cb9e5aec78e93db0fa80d9 --status pass
```

### ibmcloud doi publishtestrecord
{: #publishtestrecord}

 This command publishes a test record to DevOps Insights

```
 ibmcloud doi publishtestrecord --filelocation FILELOCATION --type TYPE [--drilldownurl DRILLDOWNURL] [--env ENV] [--sqtoken SONARQUBE_TOKEN] [--buildnumber BUILDNUMBER]
```

**Prerequisites**: [Prerequisites](#prerequisites), ibmcloud login

**Command Options**:
<dl>
   <dt>-F, --filelocation</dt>
   <dd>Required, The location of the test results that you want to upload. This can be a single file, an entire directory, or multiple files that match a wildcard expression.</dd>
   <dt>-T, --type</dt>
   <dd>Required, The type of test results that you want to upload. The value can be code for code coverage, unittest for unit tests, fvt for FVT tests, staticsecurityscan for static security scans, dynamicsecurityscan for dynamic app scans, and sonarqube for SonarQube scans. In addition to these types you can also use a custom quality data set tag to upload test results of different type.</dd>
   <dt>-U, --drilldownurl</dt>
   <dd>Optional, A URL where more information about the test results can be found. If this URL is invalid, the option is ignored.</dd>
   <dt>-E, --env</dt>
   <dd>Optional, The environment name to associate with the test results. This option is ignored for unit tests, code coverage tests, and static security scans.</dd>
   <dt>-K, --sqtoken</dt>
   <dd>Optional, This is a SonarQube token. This flag is valid only if the type specified is sonarqube. It is used by the CLI to pull additional information from the SonarQube server.</dd>
   <dt>-N, --buildnumber</dt>
   <dd>Optional, Any string that identifies the build. This option overrides the value provided by BUILD_NUMBER environment variable.</dd>
</dl>

**Example**:
```
ibmcloud doi publishtestrecord -F "tests/fvt/*.json" -T fvt
or
ibmcloud doi publishtestrecord --filelocation "tests/fvt/*.json" --type fvt
```

### ibmcloud doi publishdeployrecord
{: #publishdeployrecord}
 This command publishes a deploy record to DevOps Insights

```
 ibmcloud doi publishdeployrecord --env ENV --status STATUS --joburl JOBURL [--appurl APPURL] [--buildnumber BUILDNUMBER]
```

**Prerequisites**: [Prerequisites](#prerequisites), ibmcloud login

**Command Options**:
<dl>
   <dt>-E, --env</dt>
   <dd>Required, The environment where the pipeline job deployed the app.</dd>
   <dt>-S, --status</dt>
   <dd>Required, The deployment status. This value must be either pass or fail.</dd>
   <dt>-J, --joburl</dt>
   <dd>Required, The URL to the job's deployment logs.</dd>
   <dt>-A, --appurl</dt>
   <dd>Optional, The URL where the deployed app is running.</dd>
   <dt>-N, --buildnumber</dt>
   <dd>Optional, Any string that identifies the build. This option overrides the value provided by BUILD_NUMBER environment variable.</dd>
</dl>

**Example**:
```
ibmcloud doi publishdeployrecord -E "staging" -S pass -J "https://app.staging.example.com"
or
ibmcloud doi publishdeployrecord --env "staging" --status pass --joburl "https://app.staging.example.com"
```


### ibmcloud doi evaluategate
{: #evaluategate}
 This command evaluates a DevOps Insights gate
```
 ibmcloud doi evaluategate --policy POLICY [--forcedecision] [--ruletype RULETYPE] [--buildnumber BUILDNUMBER]
```

**Prerequisites**: [Prerequisites](#prerequisites), ibmcloud login

**Command Options**:
<dl>
   <dt>-P, --policy</dt>
   <dd>Required, The name of the policy that the gate uses to make its decision.</dd>
   <dt>-D, --forcedecision</dt>
   <dd>Optional, Set the value to true to cause the process to exit with an error code, if the policy evaluation fails. The value defaults to false if this option is not specified.</dd>
   <dt>-E, --ruletype</dt>
   <dd>Optional, A rule type to consider. If you include this option, only rules of this type are considered in the decision-making process. The value can be code for code coverage, unittest for unit tests, fvt for FVT tests, staticsecurityscan for static security scans, dynamicsecurityscan for dynamic app scans, and sonarqube for SonarQube scans.</dd>
   <dt>-N, --buildnumber</dt>
   <dd>Optional, Any string that identifies the build. This option overrides the value provided by BUILD_NUMBER environment variable.</dd>
</dl>

**Example**:
```
ibmcloud doi evaluategate -P "policyname" -D true
or
ibmcloud doi evaluategate --policy "policyname" --forcedecision true
```
