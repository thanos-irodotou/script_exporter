#!groovy
branchName = env.BRANCH_NAME.replace('/', '_')
buildTag = "build.${env.BUILD_NUMBER}"
gcpProject = "bw-prod-platform0"
app = "script_exporter"

node('analytics-backend') {
  stage ('Checkout') {
    checkout([
        $class: 'GitSCM',
        branches: scm.branches,
        doGenerateSubmoduleConfigurations: scm.doGenerateSubmoduleConfigurations,
        extensions: scm.extensions + [[$class: 'CleanCheckout']],
        userRemoteConfigs: scm.userRemoteConfigs
      ])
  }

  stage('Docker build') {
    docker.withRegistry("https://eu.gcr.io", "gcr:${gcpProject}") {
      img = docker.build("${gcpProject}/${app}:${branchName}")
      img.push()
      img.push(buildTag)
    }
  }
}
