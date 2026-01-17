pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Deploy naar TEST') {
            steps {
                sh '''
                echo "Deploy naar TEST..."
                scp index-test.html root@162.243.48.28:/var/www/html/index.html
                '''
            }
        }
        stage('Kwaliteitscheck') {
            steps {
                sh '''
                echo "Kwaliteitscheck uitvoeren..."
                if [ -f fail.flag ]; then
                    echo "❌ Fout: fail.flag gevonden. Deploy naar productie wordt gestopt."
                    exit 1
                else
                    echo "✔️ Geen fouten gevonden. Deploy naar productie mag doorgaan."
                fi
                '''
            }
        }
        stage('Deploy naar PRODUCTIE') {
            when {
                expression { currentBuild.currentResult == 'SUCCESS' }
            }
            steps {
                sh '''
                echo "Deploy naar PRODUCTIE..."
                scp index-prod.html root@162.243.4.137:/var/www/html/index.html
                '''
            }
        }
    }
}
