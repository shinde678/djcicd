pipeline {
    agent any

    stages {

        stage('Install Dependencies') {
            steps {
                sh '''
                    cd /home/ubuntu/cicdpipeline

                    /home/ubuntu/cicdpipeline/venv/bin/python -m pip install -r requirements.txt
                '''
            }
        }

        stage('Migrate') {
            steps {
                sh '''
                    cd /home/ubuntu/cicdpipeline

                    /home/ubuntu/cicdpipeline/venv/bin/python manage.py migrate
                '''
            }
        }

        stage('Collect Static') {
            steps {
                sh '''
                    cd /home/ubuntu/cicdpipeline

                    /home/ubuntu/cicdpipeline/venv/bin/python manage.py collectstatic --noinput
                '''
            }
        }

        stage('Restart Gunicorn') {
            steps {
                sh '''
                    sudo systemctl restart gunicorn
                    sudo systemctl status gunicorn --no-pager
                '''
            }
        }
    }

    post {
        success {
            echo 'Django Deployment Successful!'
        }

        failure {
            echo 'Django Deployment Failed!'
        }
    }
}