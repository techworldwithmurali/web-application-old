pipeline {
agent {
    label 'sonar'
}
environment {
    SCANNER_HOME=tool 'sonar-scanner'
}
stages {
    stage ('scm checkout') {
        steps {
            checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'git-cred', url: 'https://github.com/chaiithu/web-application.git']])
        }
    }
    stage ('sonarqube analysis') {
        steps {
            script {
                withSonarQubeEnv('sonar-server') {
                sh """$SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=sonar\
                        -Dsonar.projectKey=sonar """
}
            }
        }
    }
    stage ('quality gate') {
        steps {
            script {
                 waitForQualityGate abortpipeline: false, credentialsid: 'sonar-token'
            }
        }
    }
}
}
