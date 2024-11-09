pipeline {
  agent any
  stages {
    stage('git') {
      steps {
        git url: 'https://github.com/kimcity0205/Jenkinsid.git', branch: 'main'
      }
    }
    stage('docker') {
      steps {
        script {
          withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
	  // Docker 로그인 및 푸시 과정
	    sh '''
	      echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin
	      docker build -t kimcity0205/report:image1 .
	      docker push kimcity0205/report:image1
	    '''
	  }
        }
      }
    }
    stage('dp and svc') {
      steps {
        sh '''
          export ANSIBLE_PYTHON_INTERPRETER=/usr/bin/python3
          pip3 install kubernetes
          ansible-playbook -i /etc/ansible/hosts /var/lib/jenkins/podtest.yml
        '''
      }
    }
  }
}
