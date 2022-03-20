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
        stage ('push artifact') {
            steps {
                sh 'mkdir -p archive'
                sh 'echo test > archive/test.txt'
                script{ zip zipFile: 'test.zip', archive: false, dir: '$workspace/*' }
                archiveArtifacts artifacts: '**/*', fingerprint: true
            }
        }
        stage('pull artifact') {
            steps {
                copyArtifacts filter: 'test.zip', fingerprintArtifacts: true, projectName: '${JOB_NAME}', selector: specific('${BUILD_NUMBER}')
                unzip zipFile: 'test.zip', dir: '/var/www/html'
                sh 'cat archive_new/test.txt'
            }
        }
    }
}