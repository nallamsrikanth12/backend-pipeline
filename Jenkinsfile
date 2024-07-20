pipeline {
    agent {
        label 'Agent-1'
    }
    options {
        timeout(time: '30' , unit: 'MINUTES')
        ansiColor('xterm')
        disableConcurrentBuilds()
    }
    environment {
        def appversion = ''
    }
    
    stages {
        stage('read the package') { 
            steps {
                script {
                    def props = readJSON file: 'package.json'
                    appversion = package.json.version
                    echo "print the ${appversion}"
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