pipeline {
 agent any
 stages {
        stage("Build") {
            environment {
                DB_HOST = "localhost"
                DB_DATABASE = "laravel"
                DB_USERNAME = "laravel"
                DB_PASSWORD = "Admin123!@#"
            }
            steps {
                sh 'php --version'
                sh 'composer install'
                sh 'composer --version'
                sh 'cp .env.example .env'
                sh 'echo DB_HOST=${DB_HOST} >> .env'
                sh 'echo DB_USERNAME=${DB_USERNAME} >> .env'
                sh 'echo DB_DATABASE=${DB_DATABASE} >> .env'
                sh 'echo DB_PASSWORD=${DB_PASSWORD} >> .env'
                sh 'php artisan key:generate'
                sh 'cp .env .env.testing'
                sh 'php artisan migrate'
            }
        }
        stage("Unit test") {
            steps {
                sh 'php artisan test'
            }
        }
        stage("PHPUnit") {
        sh 'vendor/phpunit/phpunit/phpunit --bootstrap build/bootstrap.php --configuration phpunit-coverage.xml'
        
         }
        stage('Checkstyle Report') {
        sh 'vendor/bin/phpcs --report=checkstyle --report-file=build/logs/checkstyle.xml --standard=phpcs.xml --extensions=php,inc --ignore=autoload.php --ignore=vendor/ app || exit 0'
        checkstyle pattern: 'build/logs/checkstyle.xml'
        }
    }
}