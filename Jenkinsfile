node {
    stage('checkout') {
        git url: 'https://github.com/peter-mw/jenkins-test.git'
    }
    stage("build") {
        writeFile file: "test.txt", text: "test"
             docker.image("allebb/phptestrunner-74:latest").inside("-v /home/jenkins/foo.txt:/foo.txt") { c ->
                sh 'cat /foo.txt' // we can mount any file from host
                sh 'cat test.txt' // we can access files from workspace
                sh 'echo "modified-inside-container" > test.txt' // we can modify files in workspace
                sh 'printenv' // jenkins is passing all envs variables into container

        }
        sh 'cat test.txt' // will be "modified-inside-container" here
    }
}