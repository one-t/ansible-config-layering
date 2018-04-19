pipeline {
  agent {
    docker {
      image 'ansible/centos7-ansible'
    }

  }
  stages {
    stage('Install') {
      steps {
        sh 'pip install ansible-lint'
        sh 'pip install --upgrade ansible'
      }
    }
    stage('Testing') {
      steps {
        sh 'ansible-playbook --version'
        sh 'ansible-playbook -i \'localhost,\' -b common_demo.yml -e \'ansible_connection=local\''
        sh 'ansible-lint common_demo.yml'
      }
    }
  }
}