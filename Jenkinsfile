pipeline {
    agent any

    tools {
        jdk 'jdk17'
        nodejs 'nodejs16'
    }

    environment {
        SCANNER_HOME = tool name: 'sonar_scanner'
    }

    stages {
        stage('git pull') {
            steps {
                git branch: 'main', url: 'https://github.com/HenryKum23/Bank_App.git'
            }
        }

        stage('Owaps fs scan') {
            steps {
                dependencyCheck additionalArguments: '--scan ./', odcInstallation: 'DC'
                dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }

        stage('trivy fs scan') {
            steps {
                sh 'trivy fs . > trivyfs.txt'
            }
        }

        stage('sonarqube analysis') {
            steps {
                withSonarQubeEnv('sonar') {
                    sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=full-stack-bank \
                    -Dsonar.projectKey=full-stack-bank '''
                }
            }
        }

        stage('install nodejs dependencies backend') {
            steps {
                dir('/var/lib/jenkins/workspace/fullstack-bank-app/app/backend/') {
                    sh 'npm install'

                }
            }
        } 

        stage('install nodejs dependencies frontend') {
            steps {
                dir('/var/lib/jenkins/workspace/fullstack-bank-app/app/frontend/') {
                    sh 'npm install'

                }
            }
        }

        stage('docker container deploy') {
            steps {
                sh 'npm run compose:up -d'
            }
        }

        stage('run command to tag local images') {
            steps {
                sh 'docker tag app_backend henrykum23/fullstackbank_backend:latest'
                sh 'docker tag app_frontend henrykum23/fullstackbank_frontend:latest'
                sh 'docker tag postgres:15.1 henrykum23/full-stack-bank:database'
            }
        }

        stage("TRIVY SCAN"){
            steps{
                sh "trivy image henrykum23/fullstackbank_backend:latest > trivyimage.txt"
                sh "trivy image henrykum23/fullstackbank_frontend:latest > trivyimage.txt"
                sh "trivy image henrykum23/full-stack-bank:database > trivyimage.txt" 
            }
        }

        stage('run command to push images') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'fullstack-bank-app-id', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh 'echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin'
                        sh 'docker push henrykum23/fullstackbank_backend:latest'
                        sh 'docker push henrykum23/fullstackbank_frontend:latest'
                        sh 'docker push henrykum23/full-stack-bank:database'
                    }
                }   
            }
        }  
        
    }
}
