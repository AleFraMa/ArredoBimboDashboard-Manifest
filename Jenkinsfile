pipeline {
	agent any
	
	parameters {
        booleanParam(name: 'testonlydeploy', defaultValue: false, description: 'True if you want test only deploy on service with test file')

    }
	
	environment {
		GITLAB_APP_URL = "https://github.com/ArredoBimbo/Dashboard.git" 

		GITLAB_CRED = "utenza-tecnica-github"
		GITHUB_MANU='github-manuel-personale'
		
	}
	

	stages {


		stage('Git Pull') {


			steps {
				dir("app") {
					script {
						git url: "${GITLAB_APP_URL}",credentialsId: "${GITHUB_MANU}",branch:"main"
				}
			}
			}
		}

		stage('Install Dependacy') {
			


			steps {
				dir("app"){
					script {
						sh "ls"
						sh "echo \"export const IP = 'https://arredobimbo.com:8443';\" > ./src/configs/IP.js"
						sh 'cat ./src/configs/IP.js'
						sh "npm install"
					}
				}
			}
		}
		




		stage('Deploy On server GIT') {

			steps {
				script {
					dir ("app") {

							 sh "CI=false sudo npm run deploy"		
					}	
				}
			}
		}


		
	
	
		
}

	post {	
		always {
			deleteDir()
		}
	}

}
