node {
    stage('checkout') {
        git url: 'https://github.com/peter-mw/jenkins-test.git'
    }
    stage("build") {
        writeFile file: "test.txt", text: "test"
             docker.image("allebb/phptestrunner-74:latest").inside("-v /home/jenkins/:/home/jenkins/") { c ->
                sh 'pwd'
                sh 'ls -la'
                sh 'printenv' // jenkins is passing all envs variables into container

        }
        sh 'cat test.txt' // will be "modified-inside-container" here
    }
}