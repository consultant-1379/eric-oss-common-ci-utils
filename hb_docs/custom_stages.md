# Custom Stages

## What is a Custom Stage?

A Custom Stage is a stage in the PCR and PUB pipeline that is specific to an individual project.  This allows design teams to have the freedom of innovation in their Hybrid MS CI pipeline.

For examples of Custom Stages that are run in the Hybrid MS CI pipeline today and for steps on how to create your own Custom Stage see respective child pages.
 
 <br>

# How to add a Custom Stage


In the situation where the design team needs to execute additional functionality or they would like to see result of altered golden rule, a custom stage with custom ruleset must be created

For that couple of files need to to created or modify if already exist

1.  Create Custom Jenkinsfile with name pattern  **<name-of-custom-jenkinsfile>.Jenkinsfile**" in the "**ci**" directory.
    *  Add  **stage{}** section to the file.
    *  Jenkinsfile can contain one or more stages.
    *  IMPORTANT_:_ custom Jenkinsfiles must be written in scripted pipeline syntax
    *  Suggestion - declare the ruleset to load either or both custom ruleset file and common ruleset and then use it in the bob command
    *  **Example at the top of custom Jenkinsfile**
        
        <br>
        
        ```bash

        `#!/usr/bin/env groovy`
        
        def bob = "./bob/bob"
        def ruleset = "ci/local_ruleset.yaml"  #Reference to the custom, project specific ruleset
        def ci_ruleset = "ci/common_ruleset2.0.yaml" #Reference to the common ruleset
        
        stage('Loaded Lint') {
        sh "${bob} -r ${ci_ruleset} validate-sdk"
        sh "${bob} -r ${ruleset} lint:helm-chart-check"
        }
        
        ```
        
    *  Couple of samples here  _[ci/lint-generate.Jenkinsfile](https://gerrit.ericsson.se/#/c/14376037/1/ci/lint-generate.Jenkinsfile)_
2.  Create "**local_ruleset.yaml**" and store it in the "**ci**" directory. Normal ruleset syntax applies including the import of common-properties.yaml  
    *  _Sample ruleset: [ci/local_ruleset.yaml](https://gerrit.ericsson.se/gitweb?p=OSS/com.ericsson.oss.air/eric-oss-pm-stats-exporter.git;a=blob_plain;f=ci/local_ruleset.yaml;hb=refs/heads/master)_
        

        **The local ruleset should have only the tasks/rules relevant to the custom jenkinsfile (custom stages) , please refer to common_ruleset file for reference :  [common_ruleset2.0.yaml](https://gerrit.ericsson.se/gitweb?p=OSS/com.ericsson.oss.ci/oss-common-ci-utils.git;a=blob_plain;f=dsl/rulesetFiles/common_ruleset2.0.yaml;hb=refs/heads/dVersion-2.0.0-hybrid). This file contains all the tasks used for golden stages.**

    * **Example custom Rulesetfile**

        ```groovy
        import:
          common:  ../common-properties.yaml
        ```

3.  Create (or modify)  **custom_stages.yaml  _in the_ _"ci"_  _directory_**

    *  Add name of the marker (which denotes location where to load custom file) and name of the Jenkinsfile you just created. Sample  _[ci/custom_stages.yaml](https://gerrit.ericsson.se/#/c/14376037/1/ci/custom_stages.yaml)_

    * For the exact placement of the markers, please refer to the jenkinsfiles outlined in [Centrally managed CI Files](https://eteamspace.internal.ericsson.com/display/DGBase/1.+NM+Hybrid+MS+CI+pipeline). (please look for "**ci_load_custom_stages**" in the Jenkinsfiles)


<br>

## How to add a custom stage with different functionality in PCR and Publish jobs

The same marker name is used in both PreCodeReview and Publish jobs, meaning by default the same functionality will be applied in both jobs  
However, if you need to have different functionality in the PCR and Publish job for the same custom stage

You need to construct if statement based on the condition of 'env.RELEASE'  
  

In the common_publish.Jenkinsfile you will notice there is environment variable RELEASE defined to be used during versioning update. the same variable to be used to specify different functionality

**env.RELEASE in common_publish.Jenkinsfile**
```bash
environment {
	RELEASE = "true"
}
```

Sample: Same custom stage -  different functionality in PCR and Publish jobs

**Custom stage using env.RELEASE**

```bash
stage('Stage in both PCR and Publish') {
	script {
	if (env.RELEASE) {
			<add Publish job specific functionality>
			}
	else {
			<add PCR job specific functionality>
			}
		}
	}
```
Sample: Publish Job specific stage

**Custom stage using env.RELEASE**
```bash
if (env.RELEASE) {
	stage('Stage only in the publish job') {
		<add publish job specific functionality>
		}
	}
```
[sample file](https://gerrit.ericsson.se/gitweb?p=OSS/com.ericsson.oss.service.common.usermgmt/usermgmt-service.git;a=blob;f=ci/markerfossa.Jenkinsfile;hb=256ac6b4cce4363fad9d6a2084c3aec3a9580fae)

## Demo for adding Custom Stages

-   [Custom Stages in Hybrid Pipeline.mp4](https://ericsson.sharepoint.com/:v:/s/TheHummingbirds/Ed6cb8CY40VJgyNwNq9uGw8B31khug60sXBspVeCJQkrqA?e=VBGs8h&nav=eyJyZWZlcnJhbEluZm8iOnsicmVmZXJyYWxBcHAiOiJTdHJlYW1XZWJBcHAiLCJyZWZlcnJhbFZpZXciOiJTaGFyZURpYWxvZy1MaW5rIiwicmVmZXJyYWxBcHBQbGF0Zm9ybSI6IldlYiIsInJlZmVycmFsTW9kZSI6InZpZXcifX0%3D)