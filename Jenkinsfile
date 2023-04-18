
pipeline{
        /* A Declarative Pipeline */
        agent any
        tools{
                maven "mvn"
            }
    stages {
        stage("Cleaning WorkSpace"){
            steps{
                cleanWs()
            }
        } 
            stage('Build'){
                steps{
                    sh 'mvn clean install'
                }
                post{
                    success{
                        echo 'Now Archiving...'
                        archiveArtifacts artifacts: '**/target/*.war' 
                    }
                }
            }
            stage('Deploy to Staging'){
                steps{
                    build job: 'deploy-to-staging'
                }
            }
            stage('Deploy to Production'){
                steps{
                    timeout(time: 5, unit: 'DAYS'){
                        input message: 'Approve Deployement to PRODUCTION?'
                    }
                    build job: 'deploy-to-prod'
                }
                post{
                    success{
                        echo 'Code Deployed to Production!'
                    }
                    failure{
                        echo 'Deployment Failed!'
                    }
                }
            }
        }

}
