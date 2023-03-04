pipeline{

    agent any

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

        // #stage('docker build and docker push to nexus repo'){

        //     #steps{

        //         #script{

                    
        //         }
        //     }
        // }
    }
}
