pipeline {
	agent {
    node {
        label 'linux'
        
    }
}
	stages {
	stage('Unit Tests') {
	    steps {
		    sh 'ant -f test.xml -v'
		    junit 'reports/result.xml'
	    }
	}  
	stage('Build') {
            steps {
		 sh 'ant -f build.xml -v'
            }
	}
          stage('Deploy') {
            steps {
                 archiveArtifacts artifacts: 'dist/rectangle-${BUILD_NUMBER}.jar'
		 sh(" aws s3 cp dist/rectangle-${BUILD_NUMBER}.jar s3://archie1410-assignment10/rectangle-${BUILD_NUMBER}.jar")	
				
            }
		}
		
             stage('Report') {
		steps {
		withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', accessKeyVariable: 'AWS_ACCESS_KEY_ID', credentialsId: '5fe33c5e-00d7-48e1-bbc9-dd1466fb1d10', secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']])
			{
   sh(" aws cloudformation describe-stack-resources --region us-east-1 --stack-name jenkins") 
}

			
	     }
	}
}
