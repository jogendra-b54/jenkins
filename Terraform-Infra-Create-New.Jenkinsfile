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

         stage('=============Terraform CREATING BACKEND COMPONENTS============================') {
            parallel {
                stage('Creating-Catalogue') {
                   steps {
                       dir('catalogue') {  git branch: 'main', url: 'https://github.com/jogendra-b54/catalogue.git'
                            sh '''
                                cd mutable-infra
                                echo "\033[44m STARTING CATALOGUE \033[0m"
                                terrafile -f env-${ENV}/Terrafile
                                terraform init -backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                                terraform plan -var-file=env-${ENV}/${ENV}.tfvars  -var APP_VERSION=0.0.8
                                terraform apply -var-file=env-${ENV}/${ENV}.tfvars  -var APP_VERSION=0.0.8 -auto-approve
                            '''
                            }
                        }
                  }
             stage('Creating-User') {
                    steps {
                       dir('user') {  git branch: 'main', url: 'https://github.com/jogendra-b54/user.git'
                             sh  '''
                                 echo "\033[41m=============== STARTING USER ===========\033[0m"
                                 cd mutable-infra
                                 terrafile -f env-${ENV}/Terrafile
                                 terraform init -backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                                 terraform plan -var-file=env-${ENV}/${ENV}.tfvars  -var APP_VERSION=0.0.4
                                 terraform apply -var-file=env-${ENV}/${ENV}.tfvars  -var APP_VERSION=0.0.4 -auto-approve
                             '''
                             }
                         }
                    }
            
            stage('Creating-Cart') {
                    steps {
                        dir('cart') {  git branch: 'main', url: 'https://github.com/jogendra-b54/cart.git'
                            sh ''' 
                                     cd mutable-infra
                                     echo "\033[1;43m =========STARTING CART Component ==== \033[0m"
                                     terrafile -f env-${ENV}/Terrafile
                                     terraform init -backend-config=env-${ENV}/${ENV}-backend.tfvars  -reconfigure
                                     terraform plan -var-file=env-${ENV}/${ENV}.tfvars  -var APP_VERSION=0.0.4
                                     terraform apply -var-file=env-${ENV}/${ENV}.tfvars  -var APP_VERSION=0.0.4 -auto-approve
                            '''
                                }
                            }
                        }

            stage('Creating-Shipping') {
                steps {
                    dir('shipping') {  git branch: 'main', url: 'https://github.com/jogendra-b54/shipping.git'
                         sh '''
                            echo "\033[45m ==========STARTING SHIPPING ============ \033[0m"
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
                    dir('payment') {  git branch: 'main', url: 'https://github.com/jogendra-b54/payment.git'
                            sh '''
                            echo "\033[1;43m =======STARTING PAYMENT======= \033[0m"
                            cd mutable-infra
                            sleep 30
                            terrafile -f env-${ENV}/Terrafile
                            terraform init -backend-config=env-${ENV}/${ENV}-backend.tfvars  -reconfigure
                            terraform plan -var-file=env-${ENV}/${ENV}.tfvars  -var APP_VERSION=0.0.3
                            terraform apply -var-file=env-${ENV}/${ENV}.tfvars  -var APP_VERSION=0.0.3 -auto-approve
                            '''
                            }
                        }
                    }
               }     
            }         

            stage('Creating-Frontend') {
                steps {
                    dir('frontend') {  git branch: 'main', url: 'https://github.com/jogendra-b54/frontend.git'
                            sh '''
                            echo "\033[46m ===========STARTING FRONEND============== \033[0m"
                            cd mutable-infra
                            terrafile -f env-${ENV}/Terrafile
                            terraform init -backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure
                            terraform apply -var-file=env-${ENV}/${ENV}.tfvars  -var APP_VERSION=0.0.3 -auto-approve
                            '''
                         }
                     }
                }
             
              
            }
}
       
                      

