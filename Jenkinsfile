// node('haimaxy-jnlp') {
node {
    stage('Prepare') {
        echo "1.Prepare Stage1234"
        checkout scm
        script {
            // build_tag = sh(returnStdout: true, script: 'git rev-parse --short HEAD').trim()
            build_tag = bat(returnStdout: true, script: 'git rev-parse --short HEAD').trim()
            if (env.BRANCH_NAME != 'master') {
                build_tag = "${env.BRANCH_NAME}-${build_tag}"
            }
            // build_tag = "kkk"
            echo "build_tag"
            echo build_tag
        }
    }
    stage('Test') {
      echo "2.Test Stage"
    }
    stage('Build') {
        echo "3.Build Docker Image Stage"
        // sh "docker build -t cnych/jenkins-demo:${build_tag} ."
        bat "docker build -t cnych/jenkins-demo:${build_tag} ."
    }
    stage('Push') {
        echo "4.Push Docker Image Stage"
        // withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
        //     sh "docker login -u ${dockerHubUser} -p ${dockerHubPassword}"
        //     sh "docker push cnych/jenkins-demo:${build_tag}"
        // }
        withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
            bat "docker login -u ${dockerHubUser} -p ${dockerHubPassword}"
            bat "docker push cnych/jenkins-demo:${build_tag}"
        }
    }
    stage('Deploy') {
        echo "5. Deploy Stage"
        if (env.BRANCH_NAME == 'master') {
            input "Do you want to deploy?"
        }
        // sh "sed -i 's/<BUILD_TAG>/${build_tag}/' k8s.yaml"
        // sh "sed -i 's/<BRANCH_NAME>/${env.BRANCH_NAME}/' k8s.yaml"
        // sh "kubectl apply -f k8s.yaml --record"
    }
}
