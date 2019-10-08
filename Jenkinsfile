node {
    def newApp
    def registry = 'https://registry-1.docker.io/v2/'
    def imagename = "yuvalschneider/todo"
    def registryCredential = 'dockerhub'
    def buildName
    def branch = env.BRANCH_NAME
    def tag = "$branch"+"_"+"$BUILD_NUMBER"
	
        stage('Git') {
		git 'https://github.com/yuvalschneider/node-todo-frontend.git'
	}
	stage('Build') {
		sh 'npm install'
	}
	stage('Test') {
		sh 'npm test'
	}
	stage('Building image') {
        docker.withRegistry( registry, registryCredential ) {
		    def buildName = imagename + ":$BUILD_NUMBER"
			newApp = docker.build(buildName)
			newApp.push()
                        newApp.push('latest')
        }
	}
	stage('Deploy') {
		sh "sudo helm upgrade todo todo/. --recreate-pods --set image.tag=$tag"
	}
}
