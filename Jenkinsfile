pipeline {
    agent any

    tools {
        maven 'Maven3'
        jdk 'JDK21'
    }

    triggers {
        githubPush()
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Archive WAR') {
            steps {
                archiveArtifacts artifacts: 'target/*.war', fingerprint: true
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                deploy adapters: [tomcat9(credentialsId: 'tomcat-deployer',
                                          path: '',
                                          url: 'http://98.91.197.182:8080')],
                       contextPath: 'myApp',
                       war: 'target/*.war'
            }
        }
    }

    post {
        success { echo 'Pipeline succeeded: app deployed to Tomcat' }
        failure { echo 'Pipeline failed: check the stage logs' }
    }
}
