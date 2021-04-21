import groovy.json.JsonSlurperClassic
import groovy.json.JsonSlurper
import groovy.json.*
node {


    def BUILD_NUMBER=env.BUILD_NUMBER	

    def jenkins_url="http://3.137.211.35:8080/blue/organizations/jenkins/SF-poc-devops/detail/main/"
    def final_url=jenkins_url+BUILD_NUMBER+"/pipeline"
    def RUN_ARTIFACT_DIR="tests/${BUILD_NUMBER}"
    def SFDC_USERNAME
    def TEST_LEVEL

    def HUB_ORG_uat=env.HUB_ORG_DH_uat
	//def HUB_ORG_dev=env.HUB_ORG_DH_dev
	// def HUB_ORG_prod=env.HUB_ORG_DH_prod
   def SFDC_HOST = env.SFDC_HOST_DH
    def JWT_KEY_CRED_ID = env.JWT_CRED_ID_DH
    def CONNECTED_APP_CONSUMER_KEY_uat=env.CONNECTED_APP_CONSUMER_KEY_DH_uat
	// def CONNECTED_APP_CONSUMER_KEY_dev=env.CONNECTED_APP_dev
	// def CONNECTED_APP_CONSUMER_KEY_prod=env.CONNECTED_APP_prod
	

    println 'KEY IS' 
    println JWT_KEY_CRED_ID
    println HUB_ORG_DH_prod
    println SFDC_HOST
    println CONNECTED_APP_prod
    def toolbelt = tool 'toolbelt'
    def BRANCH_NAME = env.BRANCH_NAME
    
   
	    stage('checkout source') {
        		// when running in multi-branch job, one must issue this command
        		checkout scm
	    }
 
	// UAT
	
	 try{

	     stage('UAT Deployment') {
 	
       		if (env.BRANCH_NAME == "UAT")  {

		    withCredentials([file(credentialsId: JWT_KEY_CRED_ID, variable: 'jwt_key_file')]) {
        		stage('Dev:Authorization and Deployment') {
            	if (isUnix()) {
                	rc = sh returnStatus: true, script: "${toolbelt} force:auth:jwt:grant --clientid ${CONNECTED_APP_CONSUMER_KEY_uat} --username ${HUB_ORG_uat} --jwtkeyfile ${jwt_key_file} --setdefaultdevhubusername --instanceurl ${SFDC_HOST}"
            	}else{
                	 rc = bat returnStatus: true, script: "\"${toolbelt}\" force:auth:jwt:grant --clientid ${CONNECTED_APP_CONSUMER_KEY_uat} --username ${HUB_ORG_uat} --jwtkeyfile \"${jwt_key_file}\" --setdefaultdevhubusername --instanceurl ${SFDC_HOST}"
            	}
            	if (rc != 0) { error 'hub org authorization failed' }

				println rc
				
			stage('Convert Source Code to Metadata format') {
  if (isUnix()) {
	
	  rc = sh returnStatus: true, script: "\"${toolbelt}\" force:source:convert -d MDAPI_MetaData"
	    println rc
  }
	else
	{
	   rmsg = bat returnStdout: true, script: "\"${toolbelt}\" force:source:convert -d MDAPI_MetaData"
		println rmsg
	}
	  
	   
	  
}
	
}
				
		
			
					// Run unit tests in package install scratch org.

 
stage('Run Tests In Package UAT Org') {
  if (isUnix()) {
	
	  rc = sh returnStatus: true, script: "\"${toolbelt}\" force:apex:test:run -l RunLocalTests -d MDAPI_MetaData/. -u ${HUB_ORG_uat}"
	    println rc
  }
	else
	{
	  rc = bat returnStatus: true, script: "\"${toolbelt}\" force:apex:test:run -l RunLocalTests -d MDAPI_MetaData/. -u ${HUB_ORG_uat}"
           println rc
	  
	    if (rc != 0) {
        error 'Salesforce unit test run in dev org failed.'
    }
	  
}
	
}
				
				// need to pull out assigned username
				if (isUnix()) {
					rmsg = bat returnStdout: true, script: "\"${toolbelt}\" force:mdapi:deploy -d MDAPI_MetaData/. -u ${HUB_ORG_uat}"
				}else{
			   	rmsg = bat returnStdout: true, script: "\"${toolbelt}\" force:mdapi:deploy -d MDAPI_MetaData/. -u ${HUB_ORG_uat}"
				}
			  
            	printf rmsg
            	println('Hello from a Job DSL script!')
            	println(rmsg)
		mail bcc: '', body: 'UAT stage is successful-'+final_url,  cc: 'gaurav007869@gmail.com', from: '', replyTo: '', subject: 'Successful job', to: 'patel.himanshu@yash.com,saurabh.aglave@yash.com,gaurav.sh@yash.com'
			}
		    }
		}
	     }
	catch (err) {
        		echo "Caught: ${err}"
        		currentBuild.result = 'FAILURE'
	   mail bcc: '', body: 'UAT stage has Failed with error - '+err+'-'+final_url,  cc: 'gaurav007869@gmail.com', from: '', replyTo: '', subject: 'Failed job', to: 'patel.himanshu@yash.com,saurabh.aglave@yash.com,gaurav.sh@yash.com'
			}
  
   try{

	     stage('Dev Deployment') {
 	
       		if (env.BRANCH_NAME == "Dev")  {

		   withCredentials([file(credentialsId: JWT_KEY_CRED_ID, variable: 'jwt_key_file'),string(credentialsId: 'CONNECTED_APP_dev', variable: 'CONNECTED_APP_dev'), string(credentialsId: 'HUB_ORG_DH_dev', variable: 'HUB_ORG_DH_dev'), string(credentialsId: 'SFDC_HOST_DH', variable: 'SFDC_HOST_DH')]) {
        		stage('Dev:Authorization and Deployment') {
            	if (isUnix()) {
                	rc = sh returnStatus: true, script: "${toolbelt} force:auth:jwt:grant --clientid ${CONNECTED_APP_dev} --username ${HUB_ORG_DH_dev} --jwtkeyfile ${jwt_key_file} --setdefaultdevhubusername --instanceurl ${SFDC_HOST}"
            	}else{
                	 rc = bat returnStatus: true, script: "\"${toolbelt}\" force:auth:jwt:grant --clientid ${CONNECTED_APP_dev} --username ${HUB_ORG_DH_dev} --jwtkeyfile \"${jwt_key_file}\" --setdefaultdevhubusername --instanceurl ${SFDC_HOST}"
            	}
            	if (rc != 0) { error 'hub org authorization failed' }

				println rc
				
			stage('Convert Source Code to Metadata format') {
  if (isUnix()) {
	
	  rc = sh returnStatus: true, script: "\"${toolbelt}\" force:source:convert -d MDAPI_MetaData"
	    println rc
  }
	else
	{
	   rmsg = bat returnStdout: true, script: "\"${toolbelt}\" force:source:convert -d MDAPI_MetaData"
		println rmsg
	}
	  
	   
	  
}
	
}
				
		
			
					// Run unit tests in package install scratch org.

 stage('Run Tests In Package Dev Org') {
  if (isUnix()) {
	
	  rc = sh returnStatus: true, script: "\"${toolbelt}\" force:apex:test:run -l RunLocalTests -d MDAPI_MetaData/. -u ${HUB_ORG_DH_dev}"
	    println rc
  }
	else
	{
	  rc = bat returnStatus: true, script: "\"${toolbelt}\" force:apex:test:run -l RunLocalTests -d MDAPI_MetaData/. -u ${HUB_ORG_DH_dev}"
           println rc
	  
	    if (rc != 0) {
        error 'Salesforce unit test run in dev org failed.'
    }
	  
}
	
}

	

				// need to pull out assigned username
				if (isUnix()) {
					rmsg = bat returnStdout: true, script: "\"${toolbelt}\" force:mdapi:deploy -d MDAPI_MetaData/. -u ${HUB_ORG_DH_dev}"
				}else{
			  	rmsg = bat returnStdout: true, script: "\"${toolbelt}\" force:mdapi:deploy -d MDAPI_MetaData/. -u ${HUB_ORG_DH_dev}"
					
					def jsonSlurper = new JsonSlurper()
					   while(1) {
						   rmsg = bat returnStdout: true, script: "\"${toolbelt}\" force:mdapi:deploy:report -u ${HUB_ORG_DH_dev} --json"
						   rmsg = jsonSlurper.parseText(rmsg)
						   if(rmsg.status == 1) {
							printf rmsg
							break
						                        }     
						   else{
							printf rmsg
							sleep(5000)
						       }  
				
					            }
				        }
            	printf rmsg
            	println('Hello from a Job DSL script!')
            	println(rmsg)
		mail bcc: '', body: 'Dev stage is successful-'+final_url,  cc: 'gaurav007869@gmail.com', from: '', replyTo: '', subject: 'Successful job', to: 'patel.himanshu@yash.com,saurabh.aglave@yash.com,gaurav.sh@yash.com'
			}
		    }
		}
	     }
	catch (err) {
        		echo "Caught: ${err}"
        		currentBuild.result = 'FAILURE'
	   mail bcc: '', body: 'Dev stage has Failed with error - '+err+'-'+final_url,  cc: 'gaurav007869@gmail.com', from: '', replyTo: '', subject: 'Failed job', to: 'patel.himanshu@yash.com,saurabh.aglave@yash.com,gaurav.sh@yash.com'
			}

	
		     try{

		      stage('Prod Deployment') {
 
       				if (env.BRANCH_NAME == "main")  {

    					 withCredentials([file(credentialsId: JWT_KEY_CRED_ID, variable: 'jwt_key_file'),string(credentialsId: 'CONNECTED_APP_prod', variable: 'CONNECTED_APP_prod'),string(credentialsId: 'HUB_ORG_DH_prod', variable: 'HUB_ORG_DH_prod'), string(credentialsId: 'SFDC_HOST_DH', variable: 'SFDC_HOST_DH')]) {
        					stage('Prod:Authorization and Deployment') {
           						 if (isUnix()) {
               							 rc = sh returnStatus: true, script: "${toolbelt} force:auth:jwt:grant --clientid ${CONNECTED_APP_prod} --username ${HUB_ORG_DH_prod} --jwtkeyfile ${jwt_key_file} --setdefaultdevhubusername --instanceurl ${SFDC_HOST}"
           							 }else{
                 						 rc = bat returnStatus: true, script: "\"${toolbelt}\" force:auth:jwt:grant --clientid ${CONNECTED_APP_prod} --username ${HUB_ORG_DH_prod} --jwtkeyfile \"${jwt_key_file}\" --setdefaultdevhubusername --instanceurl ${SFDC_HOST}"
            							}
          			  if (rc != 0) { error 'hub org authorization failed' }

					println rc
					}
							
								stage('Convert Source Code to Metadata format') {
  if (isUnix()) {
	
	  rc = sh returnStatus: true, script: "\"${toolbelt}\" force:source:convert -d MDAPI_MetaData"
	    println rc
  }
	else
	{
	   rmsg = bat returnStdout: true, script: "\"${toolbelt}\" force:source:convert -d MDAPI_MetaData"
		println rmsg
	}
	  
	   
	  
}
	
}		
					
			
					// Run unit tests in package install scratch org.

 
stage('Run Tests In Package Prod Org') {
  if (isUnix()) {
	
	  rc = sh returnStatus: true, script: "\"${toolbelt}\" force:apex:test:run -l RunLocalTests -d MDAPI_MetaData/. -u ${HUB_ORG_DH_prod}"
	    println rc
  }
	else
	{
	  rc = bat returnStatus: true, script: "\"${toolbelt}\" force:apex:test:run -l RunLocalTests -d MDAPI_MetaData/. -u ${HUB_ORG_DH_prod}"
           println rc
	  
	    if (rc != 0) {
        error 'Salesforce unit test run in pacakge in prod failed.'
    }
	  
}
	
}
			
					// need to pull out assigned username
					if (isUnix()) {
						 input "Deploy to prod?"
						rmsg = bat returnStdout: true, script: "\"${toolbelt}\" force:mdapi:deploy -d MDAPI_MetaData/. -u ${HUB_ORG_DH_prod}"
						//rmsg="Prod deployment pretend"
					}else{
				
  // send to email
						timeout(time: 10, unit: "MINUTES") {
						mail bcc: '', body: 'Please go to the link to approve or Reject the deployment-'+final_url,  cc: 'saurabh.aglave@yash.com', from: '', replyTo: '', subject: 'Prod deployment approval request', to: 'gs14701@gmail.com'
						input "Deploy to prod?"
		     				 rmsg = bat returnStdout: true, script: "\"${toolbelt}\" force:mdapi:deploy -d MDAPI_MetaData/. -u ${HUB_ORG_DH_prod}"
				
			 			 // rmsg="Prod deployment pretend"
					}
					} 
          		  printf rmsg
           		  println('Hello from a Job DSL script!')
            		  println(rmsg)
			  mail bcc: '', body: 'prod deployment stage is successful',  cc: 'patel.himanshu@yash.com', from: '', replyTo: '', subject: 'successful job', to: 'raghav.soni@yash.com'

     			}
		}
 	
		}
	
 catch (err) {
        		echo "Caught: ${err}"
        		currentBuild.result = 'FAILURE'
			mail bcc: '', body: 'Prod deployment stage is Failed',  cc: 'raghav.soni@yash.com', from: '', replyTo: '', subject: 'Failed job', to: 'patel.himanshu@yash.com'
	}
}
