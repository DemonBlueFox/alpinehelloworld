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
    
    stage ('Test image'){
      agent any
      steps{
        script{
          sh '''
            curl localhost:5000 | grep -q "Hello world!"
          '''
        }
      }
    }
    
    stage ('Delete Container'){
      agent any
      steps{
        script{
          sh '''
            docker stop ${CONTAINER_NAME}
            docker rm ${CONTAINER_NAME}
          '''
        }
      }
    }
    
    stage ('Push image and deploy it in staging env'){
      when{
        expression {GIT_BRANCH == 'origin/master'}
      }
      agent any
      environment{
        HEROKU_API_KEY = credentials('heroku_api_key')
      }
      steps{
        script{
          sh'''
            heroku container:login
            heroku create $(STAGING} || echo "This env already exist"
            heroku container:push -a ${STAGING} web
            heroku container:release -a ${STAGING} web
          '''
        }
      }
    }
    
    stage ('Push image and deploy it in production env'){
      when{
        expression {GIT_BRANCH == 'origin/master'}
      }
      agent any
      environment{
        HEROKU_API_KEY = credentials('heroku_api_key')
      }
      steps{
        script{
          sh'''
            heroku container:login
            heroku create $(PRODUCTION} || echo "This env already exist"
            heroku container:push -a ${PRODUCTION} web
            heroku container:release -a ${PRODUCTION} web
          '''
        }
      }
    }
  }
}
