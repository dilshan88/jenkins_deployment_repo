node {

    properties([
        pipelineTriggers([
        [$class: 'GenericTrigger',
            genericVariables: [
            [ key: 'name', value: '$.data.name' ],
            [ key: 'location', value: '$.data.path' ]
            ],
            token: '123',
            printContributedVariables: true,
            printPostContent: true,

        ]
        ])
    ]
    )
    
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

        # derive param content name 
        fileName=$(echo $name | sed 's/\\(.*\\).zip/\\1 /')
        deploymentName=$(echo $fileName | sed 's/\\(.*\\)_/\\1-/')
        paramPath="DeploymentArtifacts_"$deploymentName
        echo "Param path :"$paramPath

        # login to the dev environment
        apictl login dev -u admin -p admin -k
        # import the artifact
        message=$(apictl import api -f $name --params $paramPath -e dev --update -k)
        if [ "$message" = "Successfully imported API." ]; then
            echo "Successfully imported API."
        else
            echo $message
        fi
        rm $name

        '''
        
    }
     
}