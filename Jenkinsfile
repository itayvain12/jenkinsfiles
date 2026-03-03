pipeline {
    agent any


    stages {
        stage('Debug Branch') {
            steps {
                script {
                    echo "Selected branch parameter: '${params.BRANCH}'"
                }
            }
        }

        
        stage('Checkout') {
            steps {
                script {                   
                    echo "Checking out branch: 'master'"
                    checkout([$class: 'GitSCM',
                        branches: [[name: "main"]],
                        doGenerateSubmoduleConfigurations: false,
                        extensions: [],
                        userRemoteConfigs: [[
                            credentialsId: 'itayv',
                            url: 'https://github.com/itayvain12/playbooks.git'
                             ]]
                    ])

                }
            }
        }

        
        stage('Run Ansible Playbook') {
            steps {
                script {
                    if (params.DRY_RUN) {
                        echo "Running Ansible playbook in dry-run mode (--check)..."
                        sh """ansible-playbook spectorious.yml -i inventory.ini --check -e "host=spectorious clickhouse_server=${params.CLICKHOUSE_SERVER} clickhouse_password=${params.CLICKHOUSE_PASSWORD} mongodb_server=${params.MONGODB_SERVER} mongodb_password=${params.MONGODB_PASSWORD} mpop_mongodb_server=${params.MPOP_MONGODB_SERVER} mpop_mongodb_password=${params.MPOP_MONGODB_PASSWORD} branch=${params.BRANCH}" """
                    } else {
                        echo "Running Ansible playbook normally..."
                        sh """ansible-playbook spectorious.yml -i inventory.ini -e "host=spectorious clickhouse_server=${params.CLICKHOUSE_SERVER} clickhouse_password=${params.CLICKHOUSE_PASSWORD} mongodb_server=${params.MONGODB_SERVER} mongodb_password=${params.MONGODB_PASSWORD} mpop_mongodb_server=${params.MPOP_MONGODB_SERVER} mpop_mongodb_password=${params.MPOP_MONGODB_PASSWORD} branch=${params.BRANCH}" """

                    }
                }
            }
        }
    }

    
    post {
        success {
            echo "✅ Pipeline finished successfully for branch: ${params.BRANCH}"
        }
        failure {
            echo "❌ Pipeline failed for branch: ${params.BRANCH}"
        }
    }


}



