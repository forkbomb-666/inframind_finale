pipeline {
    environment {
        app = ''
    }
    agent any
    tools {
        maven 'maven-3.6.2'
		jdk 'openjdk-8-jdk'
    }
    stages {
        stage('Initalize'){
            steps {
                script {
                    sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                    '''
                }
            }
        }
        stage ('Build') {
            steps {
                script {
                    sh 'mvn clean package'
                }
            }
        }
    }
}