pipeline {

    agent any

    stages {

        stage('Deploy to EC2') {
            steps {

                sh '''
                    ssh -i /var/lib/jenkins/.ssh/ec2-key.pem \
                    -o StrictHostKeyChecking=no \
                    ubuntu@13.60.36.216 << 'EOF'

                    set -e

                    cd /home/ubuntu/djcicd

                    echo "Pulling latest master code..."

                    git fetch origin
                    git reset --hard origin/master

                    echo "Activating virtual environment..."

                    source venv/bin/activate

                    echo "Installing dependencies..."

                    pip install -r requirements.txt

                    echo "Running migrations..."

                    python3 manage.py migrate

                    echo "Collecting static files..."

                    python3 manage.py collectstatic --noinput

                    echo "Running server..."

                    python3 manage.py runserver 0.0.0.0:8000

                    echo "Deployment completed successfully!"

                    EOF
                '''
            }
        }
    }

    post {

        success {
            echo 'EC2 Deployment Successful!'
        }

        failure {
            echo 'EC2 Deployment Failed!'
        }
    }
}