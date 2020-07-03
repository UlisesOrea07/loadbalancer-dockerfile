pipeline {
	agent any
	triggers {
		 pollSCM('* * * * *')
	 }
	stages {
		stage('Cleanup') {
			steps {
				echo 'Removing old configuration...'
                                sh 'rm -rf /var/lib/jenkins/workspace/docker-loadbalancer/*.retry'
			}
		}
		stage('Create image') {
			steps {
				echo 'Creating image...'
                                sh 'cd /var/lib/jenkins/workspace/docker-loadbalancer/ && docker build -t loadbalancer .'
			}
		}
		stage ('Run container'){
			steps{
				echo 'Running container ....'
                                sh 'cd /var/lib/jenkins/workspace/docker-loadbalancer/ && docker run -d -p 3000:3000 --name loadbalancer1 loadbalancer '
			}
		}
		stage ('Push image'){
			steps{
				echo 'Pushing image ....'
				timeout(time:5, unit:'DAYS'){
					input message:'Do you want push this image?'
				}
                                sh 'cd /var/lib/jenkins/workspace/docker-loadbalancer/ && docker tag loadbalancer:latest uliorea/myimages:loadbalancerngixn && docker push uliorea/myimages:loadbalancerngixn'
			}
			post {
				success {
					echo 'Changes applied successfully.'
				}
				failure {
					echo ' Something failed, Check logs.'
				}
			}
		}
	}
}
