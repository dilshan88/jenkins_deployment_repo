pipeline {

    agent {
        node {
            label 'built-in'
        }
    }

    options {
        buildDiscarder logRotator( 
                    daysToKeepStr: '16', 
                    numToKeepStr: '10'
            )
    }

    stages {
        
    
    stage('Setup Environment for APICTL') {
        sh '''#!/bin/bash
        envs=$(apictl get envs --format "{{.Name}}")
        if [ -z "$envs" ]; 
        then 
            echo "No environment configured. Setting dev environment.."
            apictl add env dev --apim https://${APIM_DEV_HOST}:9443  -k
        else
            echo "Environments :"$envs
            if [[ $envs != *"dev"* ]]; then
            echo "Dev environment is not configured. Setting dev environment.."
            apictl add env dev --apim https://${APIM_DEV_HOST}:9443 -k
            fi
        fi
        '''
    }

    stage('Deploy to Development Environment') {
            
        sh '''#!/bin/bash

         apictl login dev -u admin -p admin -k
        apictl import api -f C:/ProgramData/Jenkins/.jenkins/workspace/CICD_ARTIFACTE_UPLOAD/upload/SwaggerPetstore_1.0.0.zip --environment dev --params DeploymentArtifacts_SwaggerPetstore-1.0.0 --update -k
       

        '''
        
    }
     
}

}
