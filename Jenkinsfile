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
        sh '''
        docker build -t kimcity0205/report:image1 .
        docker push kimcity0205/report:image1
        '''
      }
    }
    stage('dp and svc') {
      steps {
        sh '''
        ansible-playbook /var/lib/jenkins/podtest.yml
        '''
      }
    }
  }
}
