pipeline {
    agent any

    stages {

        stage('Deploy to EC2') {
            steps {
                sshagent(['ec2-ssh']) {

                    sh '''
                        ssh -o StrictHostKeyChecking=no ubuntu@13.60.36.216 "
                            cd /home/ubuntu/djcicd &&
                            git checkout main &&
                            git pull origin main &&
                            /home/ubuntu/djcicd/venv/bin/pip install -r requirements.txt &&
                            /home/ubuntu/djcicd/venv/bin/python manage.py migrate
                        "
                    '''
                }
            }
        }

        stage('Restart Django') {
            steps {
                sshagent(['ec2-ssh']) {

                    sh '''
                        ssh -o StrictHostKeyChecking=no ubuntu@13.60.36.216 "
                            cd /home/ubuntu/djcicd &&
                            pkill -f 'manage.py runserver 0.0.0.0:8000' || true &&
                            nohup /home/ubuntu/djcicd/venv/bin/python manage.py runserver 0.0.0.0:8000 > /home/ubuntu/djcicd/django.log 2>&1 &
                        "
                    '''
                }
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