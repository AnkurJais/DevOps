pipeline {

    parameters {
    string(name: 'Email', defaultValue: 'ankur.javatm@gmail.com', description: 'Whom should I send the email?')
	string(name: 'Slave', defaultValue: 'jenkins-slave', description: 'Where you want to execute the build?')
	string(name: 'BRANCH', defaultValue: 'feature/framework01', description: 'Git Branch you want to use as source.')
	string(name: 'GIT_GOAL', defaultValue: 'clone', description: 'Git goal you want to use.')
	string(name: 'GIT_REPO', defaultValue: 'git@github.com:AnkurJais/facebooklearning.git', description: 'Git repository url you want to use.')
	string(name: 'MVN_GOAL', defaultValue: 'clean install exec:java', description: 'Git goal you want to use.')
    }

	agent { 
	      node {
		label 'jenkins-slave'
	      } 
	}
    
	tools{
        maven "Maven 3.5.2" 
		jdk "jdk8"
	}
	
    stages {
    	stage ('Initialize'){
			steps{
		    	sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                ''' 
			}
    	}

	stage("SCM-Workspace Update") {
	    steps {
			script {
				PRO_WORKSPACE = "${WORKSPACE}/${BRANCH}"
				echo "${WORKSPACE}"
				echo "${BRANCH}"
				echo "${PRO_WORKSPACE}"
			}
		}		
	}
    	
    stage('SCM-Clone') {
	  when {
		environment name: "GIT_GOAL", value: "clone"
	  }
      steps {
		script {
		   dir("${PRO_WORKSPACE}") {
			if("${BRANCH}" != "") {
			    git(
			        branch: "${BRANCH}",
       				url: "${GIT_REPO}",
       				credentialsId: "${GIT_CREDENTIAL}"
    			)
			}
			else {
			    git(
       				url: "${GIT_REPO}",
       				credentialsId: "${GIT_CREDENTIAL}"
    			)
			}
		   }
		}
    	  }   	  
    	}
    
        stage('Build') {     		
            steps {
            echo "In Build Step"
		dir("${PRO_WORKSPACE}") {
					sh "echo $JAVA_HOME"
					sh("mvn ${MVN_GOAL} -Dexec.mainClass=learning.automation.facebook.driver.Driver")		        
	                archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
		}
            }
                      
            post{
            	success{
            		mail(from: "ankurjaiswal1812@gmail.com",body: "hello", subject: "Jenkins Email ${BUILD_URL}", to: "${params.Email}")
            	}
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}
