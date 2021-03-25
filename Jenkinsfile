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
                echo 'Running PHP 7.4 tests...'
                sh 'php -v'
                echo 'Installing Composer'
                sh 'curl -sSfL -o $WORKSPACE/composer.phar https://getcomposer.org/download/2.0.11/composer.phar'
                echo 'Installing project composer dependencies...'
                sh 'cd $WORKSPACE && php composer.phar install --no-progress'
                sh 'curl -sSfL -o $WORKSPACE/vendor/bin/phpunit https://phar.phpunit.de/phpunit-5.7.phar'
                sh 'php -v'
                sh 'php composer.phar --version'
                sh 'chmod -R 777 $WORKSPACE/storage'
                echo 'Running PHPUnit tests...'
                sh 'php $WORKSPACE/vendor/bin/phpunit --coverage-html $WORKSPACE/report/clover --coverage-clover $WORKSPACE/report/clover.xml --log-junit $WORKSPACE/report/junit.xml'
                sh 'chmod -R a+w $PWD && chmod -R a+w $WORKSPACE'
                junit 'report/*.xml'

        }
        sh 'cat test.txt' // will be "modified-inside-container" here
    }
}