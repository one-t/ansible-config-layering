pipeline {
  agent {
    docker {
      image 'ansible/centos7-ansible'
    }

  }
  stages {
    stage('Test') {
      parallel {
        stage('Test') {
          steps {
            echo 'HI'
          }
        }
        stage('Run the thing') {
          steps {
            sh 'ansible-playbook --version'
          }
        }
        stage('ls') {
          steps {
            sh 'ls'
          }
        }
      }
    }
  }
}