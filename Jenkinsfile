pipeline {

    agent any

    // tools {
    //    terraform 'terraform'
    // }
    
    stages {
        stage('checkout') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'jenkins-pipeline-token', url: 'https://github.com/rubesh-git/gocolors-tf-jenkins-project.git']])
                // dir('infrastructure') {}
            }
        }
        stage('init') {
            steps {
                dir('infrastructure') {
                withCredentials([aws(accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'go-colours', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY')]) {
                sh 'echo "yes" | terraform init -reconfigure'    
                }
                }
            }
        }
        stage('plan') {
            steps {
                dir('infrastructure') {
			withCredentials([aws(accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'go-colours', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY')]) {
			    sh 'terraform validate'
                sh 'terraform plan'
				}
                }
            }
        }
        stage('action') {
            steps {
                dir('infrastructure') {
                withCredentials([aws(accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: 'go-colours', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY')]) {
                    sh "terraform ${action} -auto-approve"
                    // Sync the key pair files to the S3 bucket
                    // sh 'aws s3 sync . s3://do-not-delete-go-colours-terraform-backend/ --exclude "*" --include "*.pem"'
					}
                }
                }
            }
        }
    }
