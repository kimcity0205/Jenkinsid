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
    stage('kube jenkins') {
      steps {
        script {
          // .kube 디렉터리가 없으면 생성
          sh '''
            mkdir -p /var/lib/jenkins/.kube
            sudo chown -R jenkins:jenkins /var/lib/jenkins/.kube
            sudo chmod -R 755 /var/lib/jenkins/.kube
            scp root@211.183.3.100:/root/.kube/config /var/lib/jenkins/.kube/config
          '''
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
    stage('Dp and Svc') {
      steps {
        // Ansible로 Kubernetes 배포 작업 수행
        sh '''
          ansible-playbook -i /etc/ansible/hosts /var/lib/jenkins/podtest.yml
        '''
      }
    }
  }
}
