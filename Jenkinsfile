node{
   stage('SCM Checkout'){
     git 'https://github.com/RakeshGanapathy/my-app.git'
   }
   stage('Compile-Package'){

      def mvnHome =  tool name: 'maven3', type: 'maven'   
	  sh "${mvnHome}/bin/mvn clean package"
	  sh 'mv target/myweb*.war target/newapp.war'
   }
//    stage('SonarQube Analysis') {
// 	        def mvnHome =  tool name: 'maven3', type: 'maven'
// 	        withSonarQubeEnv('sonar') { 
// 	          sh "${mvnHome}/bin/mvn sonar:sonar"
// 	        }
// 	    }
   stage('Build Docker Imager'){
   sh 'docker build -t arkhes/mydemo:0.0.2 .'
   }
   stage('Docker Image Push'){
   withCredentials([string(credentialsId: 'dockerPass', variable: 'dockerPassword')]) {
   sh "docker login -u arkhes -p ${dockerPassword}"
    }
   //docker push username/reponame:tagname
   sh 'docker push arkhes/mydemo:0.0.2'
   }
//    stage('Nexus Image Push'){
//    sh "docker login -u admin -p admin123 65.0.181.193:8083"
//    sh "docker tag saidamo/myweb:0.0.2 65.0.181.193:8083/damo:1.0.0"
//    sh 'docker push 65.0.181.193:8083/damo:1.0.0'
//    }
//    stage('Remove Previous Container'){
// 	try{
// 		sh 'docker rm -f tomcattest'
// 	}catch(error){
// 		//  do nothing if there is an exception
// 	}
//   }
   stage('Submit Stack') {
            steps {
            sh "aws cloudformation create-stack --stack-name s3bucket --template-body file://s3Bucket.yml --region 'us-east-1'"
              }
             }
   stage('Docker deployment'){
   sh 'docker run -d -p 8090:8080 --name tomcattest arkhes/mydemo:0.0.2' 
   }
}
