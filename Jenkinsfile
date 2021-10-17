node{
    def app
    def product = "incubyte-assignment"
    stage('clean workspace'){
        echo 'Clean Workspace'
        cleanWs()
    }
    stage('Clone repository') {
        echo "Cloning git repository to workspace"
        checkout([$class: 'GitSCM', branches: [[name: '*/main']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'cdac_github_token', url: "https://github.com/Lucifer0143/${product}.git"]]])
    }

    stage('Build image') {
        echo 'Build the docker flask image'
        app = docker.build("mpatel143/${product}")
    }

    stage('Test image') {
        echo 'Test the docker flask image'
        app.inside {
            sh 'python test.py'
        }
    }
    stage('Push image') {
        echo 'Push image to the docker hub'
        docker.withRegistry('https://registry.hub.docker.com', 'docker_cred') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }
}