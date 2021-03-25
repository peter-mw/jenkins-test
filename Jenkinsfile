pipeline {
  agent none
  stages {
    stage('Checkout') {
      steps {
        git(url: 'https://github.com/peter-mw/jenkins-test.git')
      }
    }

    stage('Test') {
      parallel {


        stage('PHP 7.4') {
  agent { docker {
                           image 'allebb/phptestrunner-74:latest'
                           args '-u root:sudo'
                         }
                     }
          steps {
            sh 'pwd'
            sh 'ls -la'
            echo 'Running PHP 7.4 tests...'
            sh 'php -v'
            echo 'Installing Composer'
            sh 'curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer'
            echo 'Installing project composer dependencies...'
            sh 'cd $WORKSPACE && composer install --no-progress'
            sh 'curl -sSfL -o $WORKSPACE/vendor/bin/phpunit https://phar.phpunit.de/phpunit-5.7.phar'
            sh 'php -v'
            sh 'composer --version'
            sh 'chmod -R 777 $WORKSPACE/storage'
            echo 'Running PHPUnit tests...'
            sh 'php $WORKSPACE/vendor/bin/phpunit --coverage-html $WORKSPACE/report/clover --coverage-clover $WORKSPACE/report/clover.xml --log-junit $WORKSPACE/report/junit.xml'
            sh 'chmod -R a+w $PWD && chmod -R a+w $WORKSPACE'
            junit 'report/*.xml'
          }
        }

      }
    }

    stage('Release') {
      steps {
        echo 'Ready.'
      }
    }

  }
}