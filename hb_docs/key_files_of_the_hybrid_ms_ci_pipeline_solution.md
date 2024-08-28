# [Key files of the Hybrid MS CI Pipeline solution](https://eteamspace.internal.ericsson.com/display/DGBase/6.+Key+files+of+the+Hybrid+MS+CI+Pipeline+solution)

## Overview

This page outlines the key files that make up the Hybrid MS CI Pipeline Solution.

## Types of files used

**Jenkinsfiles, Bob rulesets, Groovy scripts & Text files**

The Jenkinsfile defines Golden Stages for the pipeline  following ADP guidelines and uses ADP Build  framework BOB - portable, dockerized builder – that contain rules/commands for tasks defined in each stage

For easy reusability and simplification of the Jenkinsfiles a shared library method is used. Allowing for reuse of centrally stored groovy methods between different Jenkins Jobs. Library methods can be found in the  [vars directory](https://gerrit.ericsson.se/gitweb?p=OSS/com.ericsson.oss.ci/oss-common-ci-utils.git;a=tree;f=vars;h=1e07fd714f28477a8dff0654b326d959a7978e96;hb=refs/heads/dVersion-2.0.0-hybrid)  of the CI repo

Shared library method names follow the pattern 'ci_pipeline_xxx'. Whole library method can be called in by ci_pipeline_init or to call only a selected method from the library one can use ci_pipeline_init.clone_project()

<br>

## Files to know

## ***Local CI Files***

There are 4 types of files that are important when working with Hybrid MS CI Pipelines . All files should be placed in the  **root/"ci"** folder

<br>

**All Projects**

File Name: **local_pipeline_env.txt**

Description:

* File will be automatically added for new projects, manually added for migrating projects
* Allows to switch On/OFF a golden stages.
* Depending on the value "true" or "false" for a particular variable that stage will be loaded or not.

Sample: [OSS/com.ericsson.oss.service.common.usermgmt/usermgmt-service/ci/local_pipeline_env.txt](https://gerrit.ericsson.se/gitweb?p=OSS/com.ericsson.oss.service.common.usermgmt/usermgmt-service.git;a=blob_plain;f=ci/local_pipeline_env.txt;hb=refs/heads/master)

<br>

**Projects that load custom stage**

File name: **custom_stages.yaml**

* Optional - must be created by dev team if they wish to execute a custom stage
* File name pattern  custom_stages.yaml  must be stored in the  **root/"ci"** folder - is used to achieve in the post step
* The file maps  custom-stage-marker name with the name of the Jenkinsfile to be load. If custom-stage-marker name is not in file then no additional stage is loaded

Sample:  [usermgmt-service/ci/custom_stages.yam](https://gerrit.ericsson.se/gitweb?p=OSS/com.ericsson.oss.service.common.usermgmt/usermgmt-service.git;a=blob_plain;f=ci/custom_stages.yaml;hb=refs/heads/master) , markerinit.Jenkinsfile is mapped to the stage-marker-init.

File Name: **custom Jenkinsfiles**

* Optional - must be created by dev team if they wish to execute a custom stage
* File name pattern <any_name>.Jenkinsfile must be stored in the  **root/"ci"** folder. - is used to achieve all Jenkinsfiles in the post step
* Must be written in  **scripted** pipeline - some syntax difference from declarative pipeline syntax
* Pipeline definition of the custom stage(s). If multiple stages need to follow at the particular location combine them in one file

See  [Deprecated - Working with Hybrid MS CI Pipeline - Hands on#Howtoaddacustomstage](https://eteamspace.internal.ericsson.com/display/DGBase/Deprecated+-+Working+with+Hybrid+MS+CI+Pipeline+-+Hands+on#DeprecatedWorkingwithHybridMSCIPipelineHandson-Howtoaddacustomstage)

File Name: **local_ruleset.yaml**

* Optional - must be created by dev team if they wish to execute a custom stage
* File name pattern  local_ruleset.yaml  must be stored in the  **root/"ci"** folder. - is used to achieve in the post step
* Bob rulesets used by the custom Jenkinsfiles.
* To reuse values from common-properties.yaml update import path (eg. import: common:  **../**common-properties.yaml)

Sample: [https://gerrit.ericsson.se/#/c/12660130/29/ci/local_ruleset.yaml](https://gerrit.ericsson.se/#/c/12660130/29/ci/local_ruleset.yaml)

<br>

## ***Centrally managed CI Files***

Those are files that are managed by team Hummingbirds are and stored in the OSS/[com.ericsson.oss.ci/oss-common-ci-utils](https://gerrit.ericsson.se/plugins/gitiles/OSS/com.ericsson.oss.ci/oss-common-ci-utils/).

Developers have read only access.

To access the files either

-   see individual Jenkins job's build archived artifacts
-   or individual Jenkins job's build workspace ci directory
-   or alternately use ci-utils gerrit repository (or links below for individual files)

To contribute please contact  [The Hummingbirds](mailto:PDLAEONICC@pdl.internal.ericsson.com).

<br>

***Java Projects***

* [common_precodereview.Jenkinsfile](https://gerrit.ericsson.se/plugins/gitiles/OSS/com.ericsson.oss.ci/oss-common-ci-utils/+/dVersion-2.0.0-hybrid/dsl/jenkinsFiles/common_precodereview.Jenkinsfile)
Holds the pipeline definition of Java microservice precodereview pipelines.

* [common_publish.Jenkinsfile](https://gerrit.ericsson.se/plugins/gitiles/OSS/com.ericsson.oss.ci/oss-common-ci-utils/+/dVersion-2.0.0-hybrid/dsl/jenkinsFiles/common_publish.Jenkinsfile)
Holds the pipeline definition of Java microservice publish pipelines

* [common_ruleset2.0.yaml](https://gerrit.ericsson.se/plugins/gitiles/OSS/com.ericsson.oss.ci/oss-common-ci-utils/+/dVersion-2.0.0-hybrid/dsl/rulesetFiles/common_ruleset2.0.yaml)
Holds the bob rulesets used by common_precodereview.Jenkinsfile and common_publish.Jenkinsfile

***Go microservices***

* [go_precodereview.Jenkinsfile](https://gerrit.ericsson.se/plugins/gitiles/OSS/com.ericsson.oss.ci/oss-common-ci-utils/+/refs/heads/dVersion-2.0.0-hybrid/dsl/jenkinsFiles/go_precodereview.Jenkinsfile)
Holds the pipeline definition of Go microservice precodereview pipelines.

* [go_publish.Jenkinsfile](https://gerrit.ericsson.se/plugins/gitiles/OSS/com.ericsson.oss.ci/oss-common-ci-utils/+/refs/heads/dVersion-2.0.0-hybrid/dsl/jenkinsFiles/go_publish.Jenkinsfile)
Holds the pipeline definition of Go microservice publish pipelines.

* [go_ruleset2.0.yaml](https://gerrit.ericsson.se/plugins/gitiles/OSS/com.ericsson.oss.ci/oss-common-ci-utils/+/refs/heads/dVersion-2.0.0-hybrid/dsl/rulesetFiles/go_ruleset2.0.yaml)
Holds the bob rulesets used by go_precodereview.Jenkinsfile and go_publish.Jenkinsfile.

***Other Projects***

- other tech stacks - python, c++, etc.

- non-ms projects

  * [other_precodereview.Jenkinsfile](https://gerrit.ericsson.se/plugins/gitiles/OSS/com.ericsson.oss.ci/oss-common-ci-utils/+/dVersion-2.0.0-hybrid/dsl/jenkinsFiles/other_precodereview.Jenkinsfile)
Holds the pipeline definition of non-java precodereview pipelines.
  * [other_publish.Jenkinsfile](https://gerrit.ericsson.se/plugins/gitiles/OSS/com.ericsson.oss.ci/oss-common-ci-utils/+/dVersion-2.0.0-hybrid/dsl/jenkinsFiles/other_publish.Jenkinsfile)
Holds the pipeline definition of non-java publish pipelines

  * [other_ruleset2.0.yaml](https://gerrit.ericsson.se/plugins/gitiles/OSS/com.ericsson.oss.ci/oss-common-ci-utils/+/dVersion-2.0.0-hybrid/dsl/rulesetFiles/other_ruleset2.0.yaml)
Holds the bob rulesets used by other_precodereview.Jenkinsfile and other_publish.Jenkinsfile
