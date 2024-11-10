pipeline {
  agent any
  stages {
    stage('Git') {
      steps {
        git url: 'https://github.com/kimcity0205/Jenkinsid.git', branch: 'main'
      }
    }
    stage('Docker hub') {
      steps {
        sh '''
          sudo docker build -t kimcity0205/report:image1 .
          sudo docker push kimcity0205/report:image1
        '''
      }
    }
    stage('Dp and Svc') {
      steps {
        // Ansible로 Kubernetes 배포 작업 수행
        sh '''
          ansible-playbook /var/lib/jenkins/nodtest.yml
          ansible-playbook /var/lib/jenkins/testdeploy.yml
        '''
      }
    }
  }
}