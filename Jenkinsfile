node {

    def GIT_REPO    = "https://github.com/mukeshdevelp/ot-microservice-sarthi.git"
    def GIT_BRANCH  = "backend"
    def PROJECT_DIR = "salary/salary-api"

    def SONAR_SERVER = "SonarQube"
    def SCANNER_TOOL = "sonar-scanner"

    def scannerHome

    try {

        stage('Checkout Code') {
            git url: GIT_REPO, branch: GIT_BRANCH
        }

        stage('Initialize Scanner') {
            scannerHome = tool SCANNER_TOOL
        }

        stage('SonarQube Analysis (Scanner Only)') {
            dir(PROJECT_DIR) {

                withSonarQubeEnv(SONAR_SERVER) {

                    sh """
                    ${scannerHome}/bin/sonar-scanner \
                    -Dsonar.projectKey=SCA-SAARTHI \
                    -Dsonar.projectName=Salary-API \
                    -Dsonar.sources=src \
                    -Dsonar.java.binaries=target/classes \
                    """

                }

            }
        }

        echo "Sonar Scanner analysis completed successfully."

    } catch (err) {

        error "Pipeline failed: ${err}"

    }
}
