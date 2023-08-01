pipeline {
    agent any
    environment {
        ENV_URL = "pipeline.google.com"    // pipeline variable or Global variable
        SSHCRED =  credentials('SSH_CRED')
    }
    //  parameters {
    //     string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
    //     text(name: 'BIOGRAPHY', defaultValue: '', description: 'Enter some information about the person')
    //     booleanParam(name: 'TOGGLE', defaultValue: true, description: 'Toggle this value')
    //     choice(name: 'CHOICE', choices: ['One', 'Two', 'Three'], description: 'Pick something')
    //     password(name: 'PASSWORD', defaultValue: 'SECRET', description: 'Enter a password')
    // }
  
  

 //  triggers { pollSCM('*/1 * * * *') }

    stages {
        stage('Stage One'){
             steps {
                sh '''
                         echo DevOps Training
                         echo AWS Training
                         echo Batch54
                         echo Name of the URL is ${ENV_URL}
                         sleep 10
                         env
                '''
             }
        }
         stage('Stage Two'){
                 environment {
                     ENV_URL= "jenkins-pipeline.stage2.google.com"    // Local variable
                }
             
            //      input {
            //         message "Should we continue?"
            //          ok "Yes, we should."
            //          submitter "alice,bob"
            //          parameters {
            //         string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
            //     }
            // }

             steps {
                echo "This is stage two"
                echo "Name of the URL is ${ENV_URL}"
                sleep  10
             }
        }

        stage('Stage THREE'){
            
            when {
                 branch 'dev'
                 changeset "**/*.js" 
            }
            steps {
                sh '''
                echo "This is stage Three"
                echo "Name of the URL is ${ENV_URL}"
                echo -e "\\e[32m Hai "   
                sleep  10
                '''
             }
        }

        stage('Stage FOUR'){
            steps {
                sh '''
                echo "This is stage Four"
                echo "Name of the URL is ${ENV_URL}"
                echo -e "\\e[31m Welcome "   
                sleep  10
                '''
             }
        }

    }



}
