#!/usr/bin/env groovy

def bob = "./bob/bob -r ci/common_ruleset2.0.yaml"
@Library('oss-common-pipeline-lib@dVersion-2.0.0-hybrid') _

pipeline {
    agent {
        label env.NODE_LABEL
    }

    options {
        timestamps()
        timeout(time: 05, unit: 'MINUTES')
    }

    environment {
        MAVEN_CLI_OPTS = "-Duser.home=${env.HOME} -B -s ${env.WORKSPACE}/settings.xml"
    }

    stages {
        stage('SonarQube') {
            steps {
                deleteDir()
                script {
                    ci_pipeline_init.clone_project()
                    ci_pipeline_init.clone_ci_repo("common")
                    ci_pipeline_init.setEnvironmentVariables()
                    ci_pipeline_init.setBuildName()
                    ci_pipeline_init.updateJava17Builder("common")
                }
                sh "${bob} --help"
                sh "${bob} -lq"
                echo 'Inject settings.xml into workspace:'
                configFileProvider([configFile(fileId: "${env.SETTINGS_CONFIG_FILE_NAME}", targetLocation: "${env.WORKSPACE}")]) {}

                // Clean, Build and Test
                sh "${bob} clean"
                sh "${bob} init-precodereview"

                withCredentials([usernamePassword(credentialsId: 'SELI_ARTIFACTORY', usernameVariable: 'SELI_ARTIFACTORY_REPO_USER', passwordVariable: 'SELI_ARTIFACTORY_REPO_PASS')]) {
                    sh "${bob} build"
                    sh "${bob} test"

                    // SonarQube
                    withSonarQubeEnv("${env.SQ_SERVER}") {
                        sh "${bob} sonar-enterprise-pcr"
                    }

                    timeout(time: 02, unit: 'MINUTES') {
                        waitUntil {
                            withSonarQubeEnv("${env.SQ_SERVER}") {
                                script {
                                    return ci_pipeline_scripts.getQualityGate()
                                }
                            }
                        }
                    }
                }
            }
        }
    }
}