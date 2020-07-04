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
                                sh 'cd /var/lib/jenkins/workspace/docker-loadbalancer/&& docker rm loadbalancer1 -f && docker run -d -p 3000:3000 --name loadbalancer1 loadbalancer '
			}
		}
		stage ('Push image'){
			steps{
				echo 'Pushing image ....'
                                sh "cd /var/lib/jenkins/workspace/docker-loadbalancer/ echo $DPASS | sudo docker login -u uliorea --password-stdin && docker tag loadbalancer:latest uliorea/myimages:loadbalancerngixn && docker push uliorea/myimages:loadbalancerngixn"
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
