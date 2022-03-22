pipeline {
	agent any
	
	
	
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
						sh "ls -ltrah ./.git"
						sh "cat ./.git/config"
							//sh "git config user.email \"mperna.96@gmail.com\""
							//sh "git config --global user.name \"Manuel Perna\""
						 withCredentials([usernamePassword(credentialsId: 'github-manuel-personale', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                            
                           // sh "PGPASSWORD=${PASSWORD} /usr/pgsql-12/bin/pg_dump -h 58567822-2403-4215-b72a-d266e60cec48.bv72mkuf0ul4s4tm7p1g.private.databases.appdomain.cloud -p 30791  -d ibmclouddb -U admin > ./AKS/sys_uat.sql"
                        
						sh "git config --local user.name 'Manuel-1996'"
						sh "git config --local user.password '${PASSWORD}'"
						//sh "git config --global credential.helper store"
							sh "CI=false npm run deploy"	
							 }
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
