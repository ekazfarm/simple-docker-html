pipeline { 
 
  environment { 
    dockerimagename = "ekawafs/nginxweb" 
    dockerImage = "" 
  } 
 
  agent any 
 
  stages { 
 
    stage('Checkout Source') { 
      steps { 
        git 'https://github.com/ekazfarm/simple-docker-html.git' 
      } 
    } 
 
    stage('Build image') { 
      steps{ 
        script { 
          dockerImage = docker.build dockerimagename 
        } 
      } 
    } 
 
    stage('Pushing Image') { 
      environment { 
               registryCredential = 'dockerhublogin' 
           } 
      steps{ 
        script { 
          docker.withRegistry( 'https://registry.hub.docker.com', registryCredential ) { 
            dockerImage.push("latest") 
          } 
        } 
      } 
    } 
 
    stage('Deploying App to Kubernetes') { 
      steps { 
        script { 
          kubernetesDeploy(configs: "application.yml", kubeconfigId: "kubernetes") 
        } 
      } 
    } 
 
  } 
 
}