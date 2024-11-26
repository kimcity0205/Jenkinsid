pipeline {
  agent any
  stages {
    stage('git scm update') {
      steps {
        git url: 'http://10.10.100.200:8008/echo-cloud/project.git', branch: 'main', credentialsId: 'ecp-token'
      }
    }
    stage('docker build and push') {
      steps {
        sh '''
        sudo docker build -t harbor:blue .
        sudo docker tag harbor:blue 10.10.100.110/test/harbor:blue
        sudo docker push 10.10.100.110/test/harbor:blue
        '''
      }
    }
    stage('deploy and service') {
      steps {
        sh '''
        ansible-playbook /var/lib/jenkins/usernode.yml
        ansible-playbook /var/lib/jenkins/master.yml
        '''
      }
    }
    stage('SonarQube analysis') {
      steps {
        withSonarQubeEnv('sonarQubeToken') {
          sh '''
          cd ./sonar-scanner \
          sonar-scanner -Dsonar.projectKey=echoCloud -Dsonar.sources=. -Dsonar.host.url=http://10.10.100.160:9000 -Dsonar.login=sqp_8e7bc799c333ec95c8dd2e136fd44a908d9a3e69 -Dsonar.exclusions=**/*.java
          '''
        }
      }
    }
  }
}
