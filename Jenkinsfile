pipeline {
    agent any
    environment {
		DOCKERHUB_CREDENTIALS=credentials('anjalyb')
		registry = "anjalyb/kubernatesproject"
		dockerImage = ''
	}
    stages {
        stage('source'){
            steps{
               git branch: 'main', credentialsId: '4f2ab0c5-5203-419d-a587-366b02bcac48', url: 'https://github.com/AnjalyBa/kube.git' 
                
            }
        }
    stage('build and tag'){
        steps{
            script {
            sh ' docker build -t kube:latest .'
            sh 'docker tag kube:latest anjalyb/kubernatesproject:$BUILD_NUMBER'
            }
            
        }
        
    }
    stage('push it to repo'){
        steps{
            script {
         sh ''' echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin
                 docker push anjalyb/kubernatesproject:$BUILD_NUMBER '''
            }
        }
    }
    stage('deployment'){
        steps{
            sh ''' kubectl create deployment app.$BUILD_NUMBER --image=anjalyb/kubernatesproject:$BUILD_NUMBER  --replicas=3'''
        }
    }
}
}
