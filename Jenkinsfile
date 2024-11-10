pipeline {
  agent any
  environment {
    DOCKER_IMAGE = "kimcity0205/report:image1"
  }
  stages {
    stage('git') {
      steps {
        git url: 'https://github.com/kimcity0205/Jenkinsid.git', branch: 'main'
      }
    }
    stage('Copy k8s from Master') {
      steps {
        script {
          // master 서버에서 kubeconfig 파일을 Jenkins 서버로 복사
          sh "scp root@211.183.3.100:/root/.kube/config /var/lib/jenkins/.kube/config"
        }
      }
    }
    stage('Docker hub') {
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
    stage('dp and svc') {
      steps {
        // Ansible로 Kubernetes 배포 작업 수행
        sh '''
          ansible-playbook -i /etc/ansible/hosts /var/lib/jenkins/podtest.yml
        '''
      }
    }
  }
}
