pipeline {
    agent any 
    parameters {
        choice(name: 'ENV', choices: ['dev', 'prod'], description: 'Select The Environment')
    }
    options {
        ansiColor('xterm')    // Add's color to the output : Ensure you install AnsiColor Plugin.
    }
    stages {
        
        stage('Destroying-EKS') {
            steps {
                dir('EKS') {  git branch: 'main', url: 'https://github.com/b54-clouddevops/kubernetes.git'

                        sh ''' 
                            cd eks 
                            make destroy
                        ''' 
                     }
                 }
            }

        stage('Destroy Databases') {
            steps {
                git branch: 'main', url: 'https://github.com/b54-clouddevops/terraform-databases.git'
                sh "terrafile -f env-${ENV}/Terrafile"
                sh "terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure"
                sh "terraform destroy -var-file=env-${ENV}/${ENV}.tfvars -auto-approve"
            }
        } 

        stage('Terraform Destroy Network') {
            steps {
                git branch: 'main', url: 'https://github.com/b54-clouddevops/terraform-vpc.git'
                sh "terrafile -f env-${ENV}/Terrafile"
                sh "terraform init --backend-config=env-${ENV}/${ENV}-backend.tfvars -reconfigure"
                sh "terraform destroy -var-file=env-${ENV}/${ENV}.tfvars -auto-approve"
            }
        }
            

    }
}