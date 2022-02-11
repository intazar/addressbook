pipeline {
    agent any

    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "my-maven"
    }

    stages {
        stage('Source Code Checkout') {
            steps {
                echo 'Addressbook Project GitHub Repository Code Checkout'
                // Get some code from a GitHub repository
                git 'https://github.com/swagat030/addressbook.git'
                
            }
        }
        stage('Maven Compile') {
            steps {
                echo 'Addressbook Project Maven Compile'
                // Run Maven on a Unix agent.
                sh "mvn compile"
            }
        }
        stage('Code Review(PMD)') {
            steps {
                echo 'Addressbook Project Code Review Analysis'
                sh "mvn -P metrics pmd:pmd"
            }
        }
        stage('Unit Test') {
            steps {
                echo 'Addressbook Project Unit Test'
                sh "mvn test"
                junit '**/target/surefire-reports/TEST-*.xml'
            }
        }
        stage('Cobertura Coverage Report') {
            steps {
                echo 'Addressbook Project Cobertura Coverage Report'
                sh 'mvn clean cobertura:cobertura -Dcobertura.report.format=xml'
                cobertura autoUpdateHealth: false, autoUpdateStability: false, coberturaReportFile: '**/target/site/cobertura/coverage.xml', conditionalCoverageTargets: '70, 0, 0', failUnhealthy: false, failUnstable: false, lineCoverageTargets: '80, 0, 0', maxNumberOfBuilds: 0, methodCoverageTargets: '80, 0, 0', onlyStable: false, sourceEncoding: 'ASCII', zoomCoverageChart: false
            }
        }
        stage('Maven Packaging') {
            steps {
                echo 'Addressbook Project Maven Packaging'
                sh "mvn package"
                archiveArtifacts 'target/*.war'
            }
        }
       stage('Deploy with Tomcat using Ansible') {
            steps {
                echo 'Addressbook Project Deployement with Tomcat Server using Ansible'
                ansiblePlaybook credentialsId: 'private-key' disableHostKeyChecking: true, installation: 'my-ansible', inventory: 'hosts.inv', playbook: 'deployment.yaml'
            }
        }
    }
}
