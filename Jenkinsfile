pipeline {
    agent {
        label 'final-challenge-pipeline'
    }

    stages {

        stage('Compilacion') {
            steps {
               sh 'mvn -D skipTests clean install package'
            }
        }

        stage('Unit test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                   junit 'target/surefire-reports/*.xml'
                }
            }
        }

        stage('Build docker image') {
            steps {
                sh 'docker image build -t webapp-empresaxyz.com .'
            }
        }

        stage('Tag docker image') {
            steps {
                sh 'docker image tag webapp-empresaxyz.com korinrovira/webapp-empresaxyz.com:latest'
            }
        }

        stage('Upload docker image') {
            steps {
                withCredentials([string(credentialsId: 'docker-id', variable: 'dockerpwd')]) {
   sh 'docker login -u korinrovira -p ${dockerpwd} '
                sh 'docker image push korinrovira/webapp-empresaxyz.com:latest'
}
                
            }
        }
    }
}
