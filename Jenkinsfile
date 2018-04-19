pipeline {
  agent {
    docker {
      image 'ansible/centos7-ansible'
      args '--rm'
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
            sh 'ansible-playbook -i \'localhost,\' -C common_demo.yml -e \'ansible_connection=local\''
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