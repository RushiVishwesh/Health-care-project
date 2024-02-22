pipeline {
    agent { label 'Build_slave' }	
    
    tools {
        maven "maven"
    }
    
    stages {
        stage('SCM Checkout') {
            steps {
                git 'https://github.com/RushiVishwesh/Health-care-project.git'
            }
		}
        stage('Maven build') {
            steps {
                sh "mvn clean package"
            }
		}
        stage("Docker build"){
            steps {
		sh "docker build -t vishweshrushi/health_care_project:${BUILD_NUMBER} ."
		sh "docker image list"
	        sh "docker tag vishweshrushi/health_care_project:${BUILD_NUMBER} vishweshrushi/health_care_project:latest"
            }
        }
	stage('Login dockerhub') {

	    steps {
		sh 'docker login --username vishweshrushi --password dckr_pat_cOv2cbsFFxpW1X1KHX_mDLYTsjE'
	    }
	}
	stage('Push to dockerhub') {

	    steps {
		sh "docker push vishweshrushi/health_care_project:latest"
	    }
        }
        stage('Deploy to kubernetes_cluster') {

            steps {
	        script {
                sshPublisher(publishers: [sshPublisherDesc(configName: 'Kubernetes-cluster', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: 'kubectl apply -f kubernetes.yml', execTimeout: 120000, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '.', remoteDirectorySDF: false, removePrefix: '', sourceFiles: 'kubernetes.yml')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
                }
            }
        }
    }
}
