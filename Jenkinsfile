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
          sudo docker build -t kimcity0205/report:image .
          sudo docker push kimcity0205/report:image
        '''
      }
    }
    stage('Dp and Svc') {
      steps {
        sh '''
          ansible-playbook /var/lib/jenkins/nodes.yml
          ansible-playbook /var/lib/jenkins/master.yml
        '''
      }
    }
  }
}
