#!/usr/bin/env groovy

// A change
void setBuildStatusWithBackref (String context, String message, String state, String backref) {
  step([
      $class: "GitHubCommitStatusSetter",
      reposSource: [$class: "ManuallyEnteredRepositorySource", url: "https://github.com/cw-paas-dev/test-jenkins.git" ],
      contextSource: [$class: "ManuallyEnteredCommitContextSource", context: context ],
      errorHandlers: [[$class: "ChangingBuildStatusErrorHandler", result: "UNSTABLE"]],
      statusBackrefSource: [ $class: "ManuallyEnteredBackrefSource", backref: backref ],
      statusResultSource: [ $class: "ConditionalStatusResultSource", results: [[$class: "AnyBuildResult", message: message, state: state]] ]
  ]);
}

node {
  stage("Set Build Status") {
    checkout scm
    setBuildStatusWithBackref("ci/preview", "The application is running", "SUCCESS", "http://www.google.com")
    sh "env"
  }
}
