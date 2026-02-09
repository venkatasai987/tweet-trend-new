pipeline {
    agent {
        node {
            label 'js-1'
        }
    }
environment {
    PATH = "/opt/apache-maven-3.9.12/bin:$PATH"
}
    stages {
        stage("build"){
            steps {
                 echo "----------- build started ----------"
                sh 'mvn clean deploy -Dmaven.test.skip=true'
                 echo "----------- build complted ----------"
            }
        }
        stage("test"){
            steps{
                echo "----------- unit test started ----------"
                sh 'mvn surefire-report:report'
                 echo "----------- unit test Complted ----------"
            }
        }
		stage("Docker Build") {
steps {
script {
echo '<--------------- Docker Build Started --------------->'
app = docker.build("${imageName}:${version}")
echo '<--------------- Docker Build Ends --------------->'
}
}
}

stage("Docker Publish") {
steps {
script {
echo '<--------------- Docker Publish Started --------------->'

  docker.withRegistry('https://hub.docker.com/repositories/venkatasai9876', 'dh') {
    app.push()
    app.push("latest")   // optional
  }

  echo '<--------------- Docker Publish Ended --------------->'
}

}
}
		
		}
		}
		
