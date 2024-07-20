pipeline {
    agent {
        label 'Agent-1'
    }
    options {
        timeout(time: 30, unit: 'MINUTES')
        ansiColor('xterm')
        disableConcurrentBuilds()
    }
    environment {
        def appversion = ''
        nexusUrl = 'nexus.srikantheswar.online:8081'
    }
    
    stages {
        stage('read the package') { 
            steps {
                script {
                    def packagejson = readJSON file: 'package.json'
                    appversion = packagejson.version
                    echo "print the ${appversion}"
                }
                
            }
        }
        stage('Build') {
            steps {
                sh """
                npm install
                ls -lrt
                echo "print the version : ${appversion}"
                """
            }
        }
        stage('zip the code') {
            steps {
                sh """
                zip -q -r backend-${appversion}.zip * -x Jenkinsfile -x  backend-${appversion}.zip
                ls -lrt
                echo "print the version : ${appversion}"
                """
            }
        }
        stage('scan the code') {
             environment {
                scannerHome = tool 'sonar'
            }
            steps {
                script {
                     withSonarQubeEnv('sonar') {
                        sh "${scannerHome}/bin/sonar-scanner"
                    }
                }
            }
        }
        stage('nexus artifact uploader') {
            steps {
                script {
                    nexusArtifactUploader(
                    nexusVersion: 'nexus3',
                    protocol: 'http',
                    nexusUrl: "${nexusUrl}",
                    groupId: 'com.expense',
                    version: "${appversion}",
                    repository: 'backend',
                    credentialsId: 'nexus-auth',
                    artifacts: [
                    [artifactId: "backend" ,
                    classifier: '',
                    file: 'backend-' + ${appversion} + '.zip',
                    type: 'zip']
        ]
     )
                }
            }
        }
    }
    post {
        always {
            echo "i will run pipeline always"
        }
        success {
            echo "i will run pipeline is success"
        }
        failure {
            echo "i will run pipeline is failure"
        }
    }
}