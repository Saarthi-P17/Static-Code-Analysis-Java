node {

    /* -------------------------------
       Global Variables
    --------------------------------*/

    def GIT_REPO           = "https://github.com/mukeshdevelp/ot-microservice-sarthi.git"
    def GIT_BRANCH         = "backend"
    def PROJECT_DIR        = "salary/salary-api"

    def MAVEN_TOOL         = "Maven3"
    def SONAR_SERVER       = "SonarQube"

    def SONAR_PROJECT_KEY  = "Salary-API"
    def SONAR_PROJECT_NAME = "Salary-API"

    def SLACK_CHANNEL      = "#ci-operation-notifications"

    def mvnHome

    try {

        stage('Checkout Code') {
            git url: GIT_REPO, branch: GIT_BRANCH
        }

        stage('Initialize Tools') {
            mvnHome = tool MAVEN_TOOL
        }

        stage('Static Code Analysis - SonarQube') {
            dir(PROJECT_DIR) {

                withSonarQubeEnv(SONAR_SERVER) {

                    sh """
                    ${mvnHome}/bin/mvn clean verify sonar:sonar \
                    -Dsonar.projectKey=${SONAR_PROJECT_KEY} \
                    -Dsonar.projectName=${SONAR_PROJECT_NAME}
                    """

                }

            }
        }

        echo "Static Code Analysis completed successfully."

        /* -------------------------------
           SUCCESS NOTIFICATION
        --------------------------------*/
        slackSend(
            channel: SLACK_CHANNEL,
            color: 'good',
            message: """
Static Code Analysis Successful

Job: ${env.JOB_NAME}
Build: #${env.BUILD_NUMBER}
URL: ${env.BUILD_URL}
"""
        )

    } catch (err) {

        /* -------------------------------
           FAILURE NOTIFICATION
        --------------------------------*/
        slackSend(
            channel: SLACK_CHANNEL,
            color: 'danger',
            message: """
Static Code Analysis Failed

Job: ${env.JOB_NAME}
Build: #${env.BUILD_NUMBER}
URL: ${env.BUILD_URL}
"""
        )

        error "Pipeline failed: ${err}"

    } finally {

        /* -------------------------------
           POST ACTION
        --------------------------------*/
        archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true

    }
}
