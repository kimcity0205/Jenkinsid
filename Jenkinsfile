pipeline {
  agent any
  environment {
    DOCKER_IMAGE = "kimcity0205/report:image1"
  }
  stages {
    stage('Clone Git Repository') {
      steps {
        git url: 'https://github.com/kimcity0205/Jenkinsid.git', branch: 'main'
      }
    }
    stage('Build and Push Docker Image') {
      steps {
        withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
          sh '''
            echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin
            docker build -t ${DOCKER_IMAGE} .
            docker push ${DOCKER_IMAGE}
          '''
        }
      }
    }
    stage('Deploy to Kubernetes with Ansible') {
      steps {
        // Ansible로 Kubernetes 배포 작업 수행
        sh '''
          ansible-playbook -i /etc/ansible/hosts /var/lib/jenkins/podtest.yml
        '''
      }
	  }
  }
}
