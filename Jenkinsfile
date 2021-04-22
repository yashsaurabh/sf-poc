import groovy.json.JsonSlurperClassic
import net.sf.json.JSONSerializer
import groovy.json.JsonSlurper
import java.io.File
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
 
  
   try{

         stage('Dev Deployment') {
                def jsonSlurper = new JsonSlurper()  
def jsonresp= '{ "name": "John", "ID" : "1"}' //rmsg
def object = jsonSlurper.parseText(jsonresp)   
if (object.name=='John')    //status succesded 
{
print 'hey'; ///prnt rmsg
}
else
{
 
println(object.ID);   //sleep
}
         }        
         }
    catch (err) {
                echo "Caught: ${err}"
                currentBuild.result = 'FAILURE'
       mail bcc: '', body: 'Dev stage has Failed with error - '+err+'-'+final_url,  cc: 'gaurav007869@gmail.com', from: '', replyTo: '', subject: 'Failed job', to: 'patel.himanshu@yash.com,saurabh.aglave@yash.com,gaurav.sh@yash.com'
            }

    
           
}
