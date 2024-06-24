pipeline { 
    agent any
    stages {
        stage ('Checkout') {
            steps {
                git branch:'master', url: 'https://github.com/ScaleSec/vulnado.git'
            } 
        }
        stage ('Build') { 
            steps {
                sh '/var/lib/jenkins/apache-maven-3.8.8/bin/mvn --batch-mode -V -U -e clean verify -Dsurefire.useFile=false -Dmaven.test.failure.ignore'
            } 
        }
        stage ('Analysis') { 
            steps {
                sh '/var/lib/jenkins/apache-maven-3.8.8/bin/mvn --batch-mode -V -U -e checkstyle:checkstyle pmd:pmd pmd:cpd findbugs:findbugs'
            } 
        }
    } 
    post {
        always {
            junit testResults: '**/target/surefire-reports/TEST-*.xml'
            recordIssues tools: [mavenConsole(), java(), javaDoc()]
            recordIssues tool: checkStyle()
            recordIssues tool: spotBugs(pattern: '**/target/findbugsXml.xml')
            recordIssues tool: cpd(pattern: '**/target/cpd.xml')
            recordIssues tool: pmdParser(pattern: '**/target/pmd.xml')
        } 
    }
}
