pipeline {
    agent any

    tools {
        maven 'Maven 3.3.9'
    }
    stages {
        stage ('Initialize') {
            steps {
                sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                '''
            }
        }

        stage ('Maven Build') {
            steps {
                sh 'mvn -Dmaven.test.failure.ignore=true install'
            }
            post {
                success {
                    archiveArtifacts 'target/*.jar'
                }
            }
        }

        stage ('Docker Build') {
            steps {
                sh 'docker build -t eka:0.1.0 .'
                sh 'docker tag eka:0.1.0 438657604705.dkr.ecr.us-west-2.amazonaws.com/eka:0.1.0'
            }
        }

        stage('Docker push'){
        	steps {
        		docker.withRegistry('https://438657604705.dkr.ecr.us-west-2.amazonaws.com', 'ecr-creds') {
    				docker.image('eka').push('0.1.0')
  				}
  			}
        }
    }
}