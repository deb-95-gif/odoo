pipeline {
  environment {
    registry = "1995bhowmick/odoo"
    registrycredential = 'DOCKER_HUB'
    dockerImage = ''
    }
agnet any 
  stages{
    stage ('BuildDocker Image') {
        steps{
        echo "Building Docker Image"
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
            }
          }
        }
     stage ('Push Docker Image') {
        steps{
        echo "Pushing Docker Image"
        script {
         docker.withRegistry( '', registryCredential ) {
              dockerImage.push()
              dockerImage.push('latest')
            }
           }
          }
      }
    stage ('Deploy to Dev') {
        steps{
        echo "Deploying to Dev Environment"
        sh "docker run -d -e POSTGRES_USER=odoo -e POSTGRES_PASSWORD=odoo -e POSTGRES_DB=postgres --name db postgres:10"
       // sh "docker rm -f odoo || true"
        sh "docker run -p 8069:8069 --name odoo --link db:db -t odoo"
        }
    }
  }
}
