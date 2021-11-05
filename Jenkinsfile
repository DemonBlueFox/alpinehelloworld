pipeline{
  environment{
    IMAGE_NAME = "ldiconcept/alpinehelloworld"
    IMAGE_TAG = "${BUILD_TAG"
    CONTAINER_NAME = "alpinehelloworld"
    STAGING = "ynov-lucas-staging"
    PRODUCTION = "ynov-lucas-production"
  }
  agent none
  
  stages{
    
    stage ('Build Stage'){
      agent any
      steps{
        script{
          sh 'docker build -t ${IMAGE_NAME}:${IMAGE_TAG} .'
        }
      }
    }
      
    stage ('Run Container based on builded image'){
      agent any
      steps{
        script{
          sh '''
            docker run -d --name ${CONTAINER_NAME} -e PORT=5000 -p 5000:5000
          '''
        }
      }
    }
  }
}
