pipeline {
    agent any
    options { timestamps() }
    parameters {
        string(name: 'FORCE_PUBLISH', defaultValue: '', description: 'To re-release a version for a specific Minecraft release')
    }
    stages {
        stage('SCM-SKIP') {
            steps {
                scmSkip(skipPattern:'.*\\[ci skip\\].*')
            }
        }
        stage('Build') {
            tools {
                jdk "OpenJDK 21"
            }
            environment {
                MAVEN_URL = "https://repo.codemc.io/repository/relativitymc/"
            }
            steps {
                sh 'git fetch --tags'
                sh 'git reset --hard'
                withCredentials([usernamePassword(credentialsId: 'nexus-repository', passwordVariable: 'MAVEN_CRED_PSW', usernameVariable: 'MAVEN_CRED_USR')]) {
                    sh './gradlew build publish'
                }
            }
            post {
                failure {
                    cleanWs()
                }
            }
        }
    }
}

