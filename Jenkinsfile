 pipeline{
   agent any
   tools {
        maven 'maven'
    }
    stages {
      stage('Checkout SCM') {
    	  steps{
            sh 'mvn clean deploy -s settings.xml'
	        }
        }      
      stage('Docker Build'){
        steps{
            script {
            docker.withRegistry( 'https://registry.hub.docker.com', 'docker'  ) {
              dockerImage = docker.build 'careem785/webapp_4-5-23'
              dockerImage.push('latest')
                  }
             }
          }
      }
      stage('Helm Chart'){
        steps{
            dir('charts') {
              withCredentials([usernamePassword(credentialsId: 'jfrog', usernameVariable: 'username', passwordVariable: 'password')]) {
              sh '/usr/local/bin/helm package webapp'
              sh '/usr/local/bin/helm push-artifactory webapp-1.0.tgz https://kubekrm.jfrog.io/artifactory/dpthelm-helm-local/ --username $username --password $password'
                    }
             }
          }
      }
    }
  }