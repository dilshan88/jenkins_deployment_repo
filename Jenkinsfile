pipeline    {

	agent   {
		node{
			label 'built-in'
		}
	}
	environment {
		Path = "/root/apictl:$PATH"
	}
	options {
	 buildDiscarder logRotator(
		daysToKeepStr: '16',
		numToKeepStr: '10'
     )
	}
	stages  
    {
	 stage('Setup Environment for APICTL'){
		steps{
			sh '''#!/bin/bash
                #rm C:/ProgramData/Jenkins/.jenkins/workspace/gitconfig
                #touch C:/ProgramData/Jenkins/.jenkins/workspace/gitconfig
                apictl set --vcs-config-path C:/ProgramData/Jenkins/.jenkins/workspace/gitconfig
				#apictl set --vcs-source-repo-path C:/ProgramData/Jenkins/.jenkins/workspace/CICD-PIPELINE-DEV

                envs=$(apictl get envs --format "{{.Name}}")
                if [ -z "$envs" ]; 
                then 
                    echo "No environment configured. Setting dev environment.."
                    apictl add env dev --apim https://localhost:9443 
                else
                    echo "Environments :"$envs
                    if [[ $envs != *"dev"* ]]; then
                    echo "Dev environment is not configured. Setting dev environment.."
                    apictl add env dev --apim https://localhost:9443 
                    fi
                fi
                '''
		}
	 }
	 stage('Deploy APIs to Dev Environment'){
		steps{
			sh """
			apictl login dev -u admin -p admin -k
			search_dir=C:/ProgramData/Jenkins/.jenkins/workspace/CICD-PIPELINE-DEV/upload
			for entry in "$search_dir"/*
				do
					fileNameWithExtension=${entry##*/}
					fileNameWithOutExtension=${fileNameWithExtension%.*}
					echo "fileNameWithOutExtension :"$fileNameWithOutExtension
					echo "fileNameWithExtension :"$fileNameWithExtension
					paramPath="DeploymentArtifacts_"$fileNameWithOutExtension
					echo "Param path :"$paramPath
					apictl import api -f C:/ProgramData/Jenkins/.jenkins/workspace/CICD-PIPELINE-DEV/upload/$fileNameWithExtension --environment dev --params $paramPath --update -k

					echo "$entry"
					echo ${entry##*/}
				done
			"""
		}
	 }
	
	}
}
