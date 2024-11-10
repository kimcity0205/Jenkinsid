pipeline {
  agent any
  environment {
    DOCKER_IMAGE = "kimcity0205/report:image1"
  }
  stages {
    stage('Git') {
      steps {
        git url: 'https://github.com/kimcity0205/Jenkinsid.git', branch: 'main'
      }
    }
    stage('Docker hub') {
      steps {
        sh '''
          sudo docker build -t ${DOCKER_IMAGE} .
          sudo docker push ${DOCKER_IMAGE}
        '''
      }
    }
    stage('Dp and Svc') {
      steps {
        // Ansible로 Kubernetes 배포 작업 수행
        sh '''
          ansible-playbook /var/lib/jenkins/podtest.yml
        '''
      }
    }
  }
}
