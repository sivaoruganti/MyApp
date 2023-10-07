node{
  stage('Source code Management Checkout'){
     git 'https://github.com/sivaoruganti/MyApp.git'
   }
   //Need to spicify the maven path to jenkins.
   stage('Maven-buildstage'){
      def mvnHome =  tool name: 'maven3', type: 'maven'   
      sh "${mvnHome}/bin/mvn package"   //How config maven code ans is ./mvn
	    sh 'mv target/myweb*.war target/newapp.war'
   }
   stage('SonarQube Analysis') {
	        def mvnHome =  tool name: 'maven3', type: 'maven'
	        withSonarQubeEnv('sonar') { 
	          sh "${mvnHome}/bin/mvn sonar:sonar" //How to send data analysis code for  sonar qube ./mvn sonar:sonar
	        }
	    }
   stage('Build a Docker image'){
    sh 'docker build -t sivaoruganti/MyApp:0.0.2 .'
   }
   stage('Docker Image push'){
    withCredentials([string(credentialsId: 'dockerPass', variable: 'dockerPassword')]) {
    sh "docker login -u soruganti23 -p ${dockerPassword}"
    }
    sh 'docker push sivaoruganti/MyApp:0.0.2'
   }
   stage('Remove the previous container'){
    try{
      sh 'docker rm -f tomcattest'
    }catch(error){
      //do nothing if there any exception
    }
   }
   stage('Docker deployment'){
   sh 'docker run -d -p 8090:8080 --name tomcattest sivaoruganti/MyApp:0.0.2' 
   }
}
   


