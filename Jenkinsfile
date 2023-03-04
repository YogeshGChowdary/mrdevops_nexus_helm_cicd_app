pipeline{

    agent any
    environment {

        VERSION = "${env.BUILD_ID}"
    }

    stages{
        
        stage('sonar quality checks'){

            agent{

                docker {
                    image 'maven'
                }
            }
            steps{

                script{

                    withSonarQubeEnv(credentialsId: 'sonar-token') {
                    
                    sh 'mvn clean package sonar:sonar'

                    }
                }
            }
        }
        stage('Quality gate status'){

            steps{

                script{

                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar-token'
                }
            }
        }

        stage('docker build and docker push to Nexus repo'){

            steps{

                script{
                    withCredentials([string(credentialsId: 'sonar-token', variable: 'nexus_creds')]) {
                    sh '''
                    docker build -t 54.166.14.106:8083/springapp:${VERSION} .

                    docker login -u admin -p $nexus_creds 54.166.14.106:8083

                    docker push 54.166.14.106:8083/springapp:${VERSION}

                    docker rmi 54.166.14.106:8083/springapp:${VERSION}
                    '''
                    }
                }
            }
        
        }
    }
}
