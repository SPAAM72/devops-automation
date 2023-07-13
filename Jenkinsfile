[11:33] Patil, Anil

pipeline {

    agent any

 

    tools {

        // Install the Maven version configured as "M3" and add it to the path.

        maven "maven"

    }

 

    stages {

        stage('Build') {

            steps {

              checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/SPAAM72/devops-automation.git']])

              sh "mvn -Dmaven.test.failure.ignore=true clean package" 

            }

        }


        stage('Build docker image'){

            steps{

                script{

                    sh 'docker build -t tathagat23/devops-integration .'

                }

            }

        }


        stage('Push image to Hub'){

            steps{

                script{

                   withCredentials([string(credentialsId: 'dockerhub', variable: 'dockerhub')]) {

                   sh 'docker login -u tathagat27 -p ${dockerhub}'

 

                   }

                   sh 'docker push tathagat27/devops-integration'

                }

            }

        }


         stage('deploy to remote server'){

            steps{

                ansiblePlaybook credentialsId: 'ansibleid', installation: 'ansible', disableHostKeyChecking: true, inventory: 'inventory', playbook: 'playbook.yml'

            }

        }

    }

}
