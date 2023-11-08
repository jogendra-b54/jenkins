pipeline {
    agent { 
        label 'WS'
    } 
    parameters {
        choice(name: 'ENV', choices: ['dev', 'prod'], description: 'Select The Environment')
    }
    options {
        ansiColor('xterm')    // Add's color to the output : Ensure you install AnsiColor Plugin.
    }
    stages {
        stage('Terraform Create Network') {
            steps {
                git branch: 'main', url: 'https://github.com/jogendra-b54/terraform-vpc.git'
                        sh "terrafile -f env-${ENV}/Terrafile"
                        sh "terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars  -reconfigure"
                        sh "terraform plan -var-file=env-${ENV}/${ENV}.tfvars"
                        sh "terraform apply -var-file=env-${ENV}/${ENV}.tfvars -auto-approve"
                }
            }

        stage('Terraform Create ALB') {
            steps {
                git branch: 'main', url: 'https://github.com/jogendra-b54/terraform-loadbalancers.git'
                        sh "terrafile -f env-${ENV}/Terrafile"
                        sh "terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure"
                        sh "terraform plan -var-file=env-${ENV}/${ENV}.tfvars"
                        sh "terraform apply -var-file=env-${ENV}/${ENV}.tfvars -auto-approve"
                }
            }

        stage('Terraform Create Databases') {
            steps {
                        git branch: 'main', url: 'https://github.com/jogendra-b54/terraform-databases.git'
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
                       dir('catalogue') {  git branch: 'main', url: 'https://github.com/jogendra-b54/catalogue.git'
                            sh "cd mutable-infra"
                            sh "terrafile -f env-${ENV}/Terrafile"
                            sh "terraform init -backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure"
                            sh "terraform plan -var-file=env-${ENV}/${ENV}.tfvars  -var APP_VERSION=0.0.8"
                            sh "terraform apply -var-file=env-${ENV}/${ENV}.tfvars  -var APP_VERSION=0.0.8 -auto-approve"
                            }
                        }
                  }
            // stage('Creating-User') {
            //        steps {
            //            dir('USER') {  git branch: 'main', url: 'https://github.com/jogendra-b54/user.git'
            //                 sh "cd mutable-infra"
            //                 sh "terrafile -f env-${ENV}/Terrafile"
            //                 sh "terraform init -backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure"
            //                 sh "terraform plan -var-file=env-${ENV}/${ENV}.tfvars  -var APP_VERSION=0.0.5"
            //                 sh "terraform apply -var-file=env-${ENV}/${ENV}.tfvars  -var APP_VERSION=0.0.5 -auto-approve"
            //                 }
            //             }
            //        }
            // stage('Creating-Cart') {
            //     steps {
            //         dir('CART') {  git branch: 'main', url: 'https://github.com/jogendra-b54/cart.git'
            //                sh "cd mutable-infra"
            //                sh "terrafile -f env-${ENV}/Terrafile"
            //                sh "terraform init -backend-config=env-${ENV}/${ENV}-backend.tfvars  -reconfigure"
            //                 sh "terraform plan -var-file=env-${ENV}/${ENV}.tfvars  -var APP_VERSION=0.0.4"
            //                 sh "terraform apply -var-file=env-${ENV}/${ENV}.tfvars  -var APP_VERSION=0.0.4 -auto-approve"
            //                 }
            //             }
            //         }

            // stage('Creating-Shipping') {
            //     steps {
            //         dir('SHIPPING') {  git branch: 'main', url: 'https://github.com/jogendra-b54/shipping.git'
            //                sh "cd mutable-infra"
            //                 sh "sleep 30"
            //                 sh "terrafile -f env-${ENV}/Terrafile"
            //                 sh "terraform init -backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure"
            //                 sh "terraform apply -var-file=env-${ENV}/${ENV}.tfvars  -var APP_VERSION=0.0.4  -auto-approve"
            //                 }
            //             }
            //         } 
                    
            // stage('Creating-Payment') {
            //     steps {
            //         dir('PAYMENT') {  git branch: 'main', url: 'https://github.com/jogendra-b54/payment.git'
            //                 sh "cd mutable-infra"
            //                 sh "sleep 30"
            //                 sh "terrafile -f env-${ENV}/Terrafile"
            //                 sh "terraform init -backend-config=env-${ENV}/${ENV}-backend.tfvars  -reconfigure"
            //                 sh "terraform plan -var-file=env-${ENV}/${ENV}.tfvars  -var APP_VERSION=0.0.3"
            //                 sh "terraform apply -var-file=env-${ENV}/${ENV}.tfvars  -var APP_VERSION=0.0.3 -auto-approve"
            //                 }
            //             }
            //         }

                } 
            }
                    
            // stage('Creating-Frontend') {
            //     steps {
            //         dir('PAYMENT') {  git branch: 'main', url: 'https://github.com/jogendra-b54/frontend.git'
            //                 sh "cd mutable-infra"
            //                 sh "terrafile -f env-${ENV}/Terrafile"
            //                 sh "terraform init -backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure"
            //                 sh "terraform apply -var-file=env-${ENV}/${ENV}.tfvars  -var APP_VERSION=0.0.3 -auto-approve"
            //              }
            //          }
            //     }
            }    
        }                        

