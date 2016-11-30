#!/usr/bin/env groovy

void setBuildStatus (String context, String message, String state) {
  step([
      $class: "GitHubCommitStatusSetter",
      contextSource: [$class: "ManuallyEnteredCommitContextSource", context: context ],
      errorHandlers: [[$class: "ChangingBuildStatusErrorHandler", result: "UNSTABLE"]],
      statusResultSource: [ $class: "ConditionalStatusResultSource", results: [[$class: "AnyBuildResult", message: message, state: state]] ]
  ]);
}

void setBuildStatusWithBackref (String context, String message, String state, String backref) {
  step([
      $class: "GitHubCommitStatusSetter",
      contextSource: [$class: "ManuallyEnteredCommitContextSource", context: context ],
      errorHandlers: [[$class: "ChangingBuildStatusErrorHandler", result: "UNSTABLE"]],
      statusBackrefSource: [ $class: "ManuallyEnteredBackrefSource", backref: backref ],
      statusResultSource: [ $class: "ConditionalStatusResultSource", results: [[$class: "AnyBuildResult", message: message, state: state]] ]
  ]);
}

node {
  stage("Set Preview URL") {
    setBuildStatusWithBackref("ci/preview", "The application is running", "SUCCESS", "http://www.google.com")
  }
  stage("Output build URL") {
    echo "The build URL is ${env.BUILD_URL}"
  }
  stage("Approve") {
    setBuildStatusWithBackref("ci/test", "Approve here", "PENDING", "${env.BUILD_URL}input")
    input "Do you approve?"
  }
}
