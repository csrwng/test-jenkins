#!/usr/bin/env groovy

void setBuildStatus (String commitId, String context, String message, String state) {
  step([
      $class: "GitHubCommitStatusSetter",
      commitShaSource: [$class: "ManuallyEnteredShaSource", sha: commitId ],
      contextSource: [$class: "ManuallyEnteredCommitContextSource", context: context ],
      errorHandlers: [[$class: "ChangingBuildStatusErrorHandler", result: "UNSTABLE"]],
      statusResultSource: [ $class: "ConditionalStatusResultSource", results: [[$class: "AnyBuildResult", message: message, state: state]] ]
  ]);
}

void setBuildStatusWithBackref (String commitId, String context, String message, String state, String backref) {
  step([
      $class: "GitHubCommitStatusSetter",
      commitShaSource: [$class: "ManuallyEnteredShaSource", sha: commitId ],
      contextSource: [$class: "ManuallyEnteredCommitContextSource", context: context ],
      errorHandlers: [[$class: "ChangingBuildStatusErrorHandler", result: "UNSTABLE"]],
      statusBackrefSource: [ $class: "ManuallyEnteredBackrefSource", backref: backref ],
      statusResultSource: [ $class: "ConditionalStatusResultSource", results: [[$class: "AnyBuildResult", message: message, state: state]] ]
  ]);
}

def commitId=""

node {
  stage("Checkout") {
    checkout scm
    sh "git rev-parse HEAD > commitid"
    commitId=readFile("commitid").trim()
  }
  stage("Display env") {
    sh "env"
  }
  stage("Set Preview URL") {
    setBuildStatusWithBackref(commitId, "ci/preview", "The application is running", "SUCCESS", "http://www.google.com")
  }
  stage("Output build URL") {
    echo "The build URL is ${env.BUILD_URL}"
  }
  stage("Approve") {
    setBuildStatusWithBackref(commitId, "ci/test", "Approve here", "PENDING", "${env.BUILD_URL}input")
    input "Do you approve?"
  }
  setBuildStatus(commitId, "ci/test", "Manually approved", "SUCCESS")
}
