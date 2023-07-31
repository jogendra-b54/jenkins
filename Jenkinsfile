pipeline {
    agent any
    environment {
        ENV_URL = "pipeline.google.com"    // pipeline variable or Global variable
        SSHCRED =  credentials('SSH_CRED')
    }
    stages {
        stage('Stage One'){
             steps {
                sh '''
                         echo DevOps Training
                         echo AWS Training
                         echo Batch54
                         echo Name of the URL is ${ENV_URL}
                         env
                '''
             }
        }
         stage('Stage Two'){
                 environment {
                     ENV_URL= "jenkins-pipeline.stage2.google.com"    // Local variable
                }
             steps {
                echo "This is stage two"
                echo "Name of the URL is ${ENV_URL}"
             }
        }
        stage('Stage Three'){
             steps {

                echo "This is stage Three"
                echo "Name of the URL is ${ENV_URL}"
                // sh echo -e "\e[32m Hai \e[0m"  # this will give error while build because as per jenkins \ backslahsh is an incorect syntax
                sh echo -e "\\e[32m Hai \\e[0m"   //tell jenkins this is escape sequence character which we need to add extra \ to build success
             }
        }

    }

}
