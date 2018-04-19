pipeline {
  agent {
    docker {
      image 'ansible/centos7-ansible'
    }

  }
  stages {
    stage('Run the thing') {
      steps {
        sh 'ansible-playbook -i \'localhost,\' -b common_demo.yml -e \'ansible_connection=local\''
      }
    }
  }
}