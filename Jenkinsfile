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
             }
        }

    }

}
