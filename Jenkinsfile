pipeline {
  agent any
  environment {
    AWS_SECRET_ACCESS_KEY=credentials('AWS_SECRET_ACCESS_KEY')
    AWS_ACCESS_KEY_ID=credentials('AWS_ACCESS_KEY_ID')
  }
  stages {
    stage('Stage 1') {
      steps {
        echo "======== executing stage ========"
        echo 'Hello world!'
      }
    }
    stage("getting code") {
      steps {
        echo "======== executing stage ========"
        git url: 'https://github.com/Malek-Zaag/CICD-pipeline-with-Ansible-and-AWS.git', branch: 'main', credentialsId: 'Github-creds'       
        sh "ls -ltr"
        sh "java --version"
      }
    }

    stage("checking terraform and provisionning servers") {
        steps {
            echo "======== executing stage ========"
            sh "terraform --version"
            dir ("./Infrastructure/EC2/") {
                sh "terraform init --upgrade"
                sh "whoami"
                sh "env"
                sh "terraform plan"
            }
        }
    }

    stage ("Adding public IPs to file") {
      steps {
        dir ("./Infrastructure/EC2/") {
              sh "terraform output > file.txt"
              sh "cut -d '=' -f 2 file.txt"
            }
      }
    }

    stage("applying ansible playbooks for configuring files") {
       steps {
           echo "======== executing stage ========"
           sh "ansible --version"        
      }
    }
  }
}