pipeline {
    environment {
        app = ''
        image_name = 'drake666/inframind-finale'
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
        stage ('Build Image') {
            steps {
                script {
                    app = docker.build(image_name)
                }
            }
        }
        stage ('Push Image') {
            steps {
                script {
                    docker.withRegistry('', 'dockerhubcreds'){
                        app.push("$BUILD_NUMBER")
                        app.push("latest")
                    }
                }
            }
        }
        stage ('Kubernetes Deployment') {
            def remote  = [:]
            remote.name = "Test06"
            remote.host = "10.134.19.206"
            remote.user = "user"
            remote.password = "tcs#1234"
            remote.allowAnyHosts = true
            sshCommand remote: remote, command: "kubectl run --image=drake666/inframind-finale:latest inframind-finale-v$BUILD_NUMBER --port=9090 --replicas=2"
        }
    }
}