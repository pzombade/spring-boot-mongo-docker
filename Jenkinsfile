node{

	stage("Hello World"){
		//input("Are you sure to continue?")
        echo "Hello World From Git Jenkinsfile building branch ${branch_name}"
	}

    stage("Git Clone"){
      git branch: '${branch_name}', credentialsId: 'GIT_PZOMBADE_CREDS_NEW', url: 'https://github.com/pzombade/spring-boot-mongo-docker.git'
  //     git 'https://github.com/MithunTechnologiesDevOps/spring-boot-mongo-docker.git'
    }
    
    stage("Maven Clean Build"){
        def mavenHome = tool name: "Maven-3.6.1", type: "maven"
        echo "#####12345 ${mavenHome}"
        def mvnCMD = "${mavenHome}/bin/mvn "
        sh "${mvnCMD} clean package"
    }
    
    stage("Docker Image Build"){
        sh "docker build -t pzombade/spring-boot-mongo2 ."
    }
    
    stage("Docker Push"){
        withCredentials([usernamePassword(credentialsId: 'docker_cred', passwordVariable: 'dp', usernameVariable: 'du')]) {
            sh "docker login -u '${du}' -p '${dp}'"
        }
        sh "docker push pzombade/spring-boot-mongo2"
    }
    
    stage("Deploy into K8s from Jenkins node"){
        sh "kubectl delete -f springBootMongo.yml"
        sh "wait"
        sh "kubectl apply -f springBootMongo.yml"
    }
    
    /*
    stage("Deploy Application In K8s"){
        kubernetesDeploy(
            configs: 'springBootMongo.yml',
            kubeconfigId: 'KUBERNETES_CLUSTER_CONFIG')
    }
    */
    
    
    
}
