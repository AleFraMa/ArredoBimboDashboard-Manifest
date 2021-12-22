pipeline {
	agent any
	
	parameters {
        booleanParam(name: 'testonlydeploy', defaultValue: false, description: 'True if you want test only deploy on service with test file')

    }
	
	environment {
		GITLAB_APP_URL = "https://github.com/Manuel-1996/ArredoBimboE-Commerce.git" 

		GITLAB_CRED = "utenza-tecnica-github"
		GITHUB_MANU='github-manuel-personale'
		
	}
	

	stages {


		stage('Git Pull') {

		 	when {
            	expression {
                    return !params.testonlydeploy;
                }
        	}
			steps {
				dir("app") {
					script {
						git url: "${GITLAB_APP_URL}",credentialsId: "${GITHUB_MANU}"
				}
			}
			}
		}

		stage('Install Dependacy') {
			
			when {

            	expression {
                    return !params.testonlydeploy;
                }
        	}

			steps {
				dir("app"){
					script {
						sh "echo \"export const IP = 'https://arredobimbo.com:8443';\" > ./src/configs/IPConfig.js"
						sh 'cat ./src/configs/IPConfig.js'
						sh "npm install --force"
					}
				}
			}
		}
		
		stage('Build Application') {

			when {
            	expression {
                    return !params.testonlydeploy;
                }
        	}

			steps {
				dir("app") {
					script {
						sh "echo build"
						sh "CI=false npm run build"
					}
				}
			}
		}

		stage('Remove Old site') {
			
			when {
            	expression {
                    return !params.testonlydeploy;
                }
        	}
			steps {
				dir("app") {
					script {
						sh "echo dafare"
					}
				}
			}
		}

		stage('Deploy On server Alibaba') {
			when {
           	 	expression {
                    return !params.testonlydeploy;
                }
        	}
			steps {
				script {
					dir ("app") {

							 withCredentials([string(credentialsId: "password-ssh-arredobimbo-server", variable: 'password')]) {

       								sh "sshpass -p \"${password}\" scp -o StrictHostKeyChecking=no -r ./build/* root@arredobimbo.com:/usr/share/nginx/html"
        
   							}		
					}	
				}
			}
		}

		stage('TEST ONLY DEPLOY') {
			when {
            	expression {
                    return params.testonlydeploy;
                }
        	}
			steps {
				script {
					dir ("app") {
						
							sh "echo prova>> prova.txt"
							 withCredentials([string(credentialsId: "password-ssh-arredobimbo-server", variable: 'password')]) {

       								sh "sshpass -p \"${password}\" scp -o StrictHostKeyChecking=no ./prova.txt root@arredobimbo.com:/root"
        
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
