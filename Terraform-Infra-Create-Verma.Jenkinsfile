pipeline {
    agent any 
    parameters {
        choice(name: 'ENV', choices: ['dev', 'prod'], description: 'Select The Environment')
    }
    options {
        ansiColor('xterm')    // Add's color to the output : Ensure you install AnsiColor Plugin.
    }
    stages {
        stage('Terraform Create Network') {
            steps {
                git branch: 'main', url: 'https://github.com/b54-clouddevops/terraform-vpc.git'
                        sh "terrafile -f env-${ENV}/Terrafile"
                        sh "terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars  -reconfigure"
                        sh "terraform plan -var-file=env-${ENV}/${ENV}.tfvars"
                        sh "terraform apply -var-file=env-${ENV}/${ENV}.tfvars -auto-approve"
                }
            }

        stage('Terraform Create ALB') {
            steps {
                git branch: 'main', url: 'https://github.com/b54-clouddevops/terraform-loadbalancers.git'
                        sh "terrafile -f env-${ENV}/Terrafile"
                        sh "terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure"
                        sh "terraform plan -var-file=env-${ENV}/${ENV}.tfvars"
                        sh "terraform apply -var-file=env-${ENV}/${ENV}.tfvars -auto-approve"
                }
            }

        stage('Terraform Create Databases') {
            steps {
                        git branch: 'main', url: 'https://github.com/b54-clouddevops/terraform-databases.git'
                        sh "terrafile -f env-${ENV}/Terrafile"
                        sh "terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure"
                        sh "terraform plan -var-file=env-${ENV}/${ENV}.tfvars"
                        sh "terraform apply -var-file=env-${ENV}/${ENV}.tfvars -auto-approve "
                    }
                }

        stage('Backend') {
           parallel {
            stage('Creating-Catalogue') {
                   steps {
                       dir('Catalogue') {  git branch: 'main', url: 'https://github.com/b54-clouddevops/catalogue.git'
                          sh '''
                            cd mutable-infra
                            terrafile -f env-${ENV}/Terrafile
                            terraform init -backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                            terraform plan -var-file=env-${ENV}/${ENV}.tfvars  -var APP_VERSION=0.0.13
                            terraform apply -var-file=env-${ENV}/${ENV}.tfvars  -var APP_VERSION=0.0.13 -auto-approve
                          '''
                            }
                        }
                  }
            stage('Creating-User') {
                   steps {
                       dir('USER') {  git branch: 'main', url: 'https://github.com/b54-clouddevops/user.git'
                          sh '''
                            cd mutable-infra
                            terrafile -f env-${ENV}/Terrafile
                            terraform init -backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                            terraform plan -var-file=env-${ENV}/${ENV}.tfvars  -var APP_VERSION=0.0.6
                            terraform apply -var-file=env-${ENV}/${ENV}.tfvars  -var APP_VERSION=0.0.6 -auto-approve
                          '''
                            }
                        }
                   }
            stage('Creating-Cart') {
                steps {
                    dir('CART') {  git branch: 'main', url: 'https://github.com/b54-clouddevops/cart.git'
                          sh '''
                            cd mutable-infra
                            terrafile -f env-${ENV}/Terrafile
                            terraform init -backend-config=env-${ENV}/${ENV}-backend.tfvars  -reconfigure
                            terraform plan -var-file=env-${ENV}/${ENV}.tfvars  -var APP_VERSION=0.0.3
                            terraform apply -var-file=env-${ENV}/${ENV}.tfvars  -var APP_VERSION=0.0.3 -auto-approve
                          '''
                            }
                        }
                    }

            stage('Creating-Shipping') {
                steps {
                    dir('SHIPPING') {  git branch: 'main', url: 'https://github.com/b54-clouddevops/shipping.git'
                          sh '''
                            cd mutable-infra
                            sleep 30   
                            terrafile -f env-${ENV}/Terrafile
                            terraform init -backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                            terraform apply -var-file=env-${ENV}/${ENV}.tfvars  -var APP_VERSION=0.0.4  -auto-approve
                          '''
                            }
                        }
                    } 
                    
            stage('Creating-Payment') {
                steps {
                    dir('PAYMENT') {  git branch: 'main', url: 'https://github.com/b54-clouddevops/payment.git'
                          sh '''
                            cd mutable-infra
                            sleep 30
                            terrafile -f env-${ENV}/Terrafile
                            terraform init -backend-config=env-${ENV}/${ENV}-backend.tfvars  -reconfigure
                            terraform plan -var-file=env-${ENV}/${ENV}.tfvars  -var APP_VERSION=0.0.4
                            terraform apply -var-file=env-${ENV}/${ENV}.tfvars  -var APP_VERSION=0.0.4 -auto-approve
                          '''
                            }
                        }
                    }

                } 
            }
                    
            stage('Creating-Frontend') {
                steps {
                    dir('PAYMENT') {  git branch: 'main', url: 'https://github.com/b54-clouddevops/frontend.git'
                          sh '''
                            cd mutable-infra
                            terrafile -f env-${ENV}/Terrafile
                            terraform init -backend-config=env-${ENV}/${ENV}-backend.tfvars   -reconfigure
                            terraform apply -var-file=env-${ENV}/${ENV}.tfvars  -var APP_VERSION=0.0.3 -auto-approve
                          '''
                         }
                     }
                }
            }    
        }                        

