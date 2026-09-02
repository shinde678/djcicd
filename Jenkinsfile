stage('Deploy to EC2') {
    steps {
        sshagent(['ec2-ssh']) {
            sh '''
                ssh -o StrictHostKeyChecking=no ubuntu@13.60.36.216 "
                    cd /home/ubuntu/djcicd &&
                    git checkout master &&
                    git pull origin master &&
                    /home/ubuntu/djcicd/venv/bin/pip install -r requirements.txt &&
                    /home/ubuntu/djcicd/venv/bin/python manage.py migrate
                "
            '''
        }
    }
}