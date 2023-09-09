properties([
    parameters([
        // Separator
        separator(name: "CREATE_ALERTS", sectionHeader: "Create Alerts",
          separatorStyle: "border-width: 0",
          sectionHeaderStyle: """
            background-color: #7ea6d3;
            text-align: left;
            padding: 4px;
            color: #343434;
            font-size: 22px;
            font-weight: normal;
            text-transform: uppercase;
            font-family: 'Orienta', sans-serif;
            letter-spacing: 1px;
            font-style: italic;
          """
        ),
        [
          $class: 'DynamicReferenceParameter', 
          choiceType: 'ET_FORMATTED_HTML', 
          description: 'Enter the Alerts Name to create in GCP',
          name: 'ALERTS_NAME', 
          omitValueField: true,
          referencedParameters: 'ENVIRONMENT',
          script: [
              $class: 'GroovyScript', 
              fallbackScript: [
                  classpath: [],
                  sandbox: true,
                  script: 
                      'return [\'Error message\']'
              ], 
              script: [
                  classpath: [], 
                  sandbox: true,
                  script: 
                      """ 
                          html=""
                          if (ENVIRONMENT.contains('')){
                              html="<input name='value' value='' class='setting-input' type='text'>"
                          }
                          else {
                            
                              html="Enter value in ALERTS_NAME to create alerts in GCP"
                          }
                          return html
                      """
              ]
          ]
        ]
    ])
])

pipeline {
    agent any
    environment {
        GCLOUD_DIR = "$JENKINS_HOME/google-cloud-sdk/bin"
    }
    stages {
        stage('Installing Dependencies') {
            steps {
                sh '''#!/bin/bash
                    echo "Checking for pre-installed dependencies..."
                    echo ""
                    if [ ! -d "$GCLOUD_DIR" ]; then
                        echo "Installing GCloud CLI..."
                        echo ""
                        cd $JENKINS_HOME
                        curl -O https://dl.google.com/dl/cloudsdk/channels/rapid/downloads/google-cloud-cli-412.0.0-linux-x86_64.tar.gz
                        tar -xf google-cloud-cli-412.0.0-linux-*.tar.gz
                        ./google-cloud-sdk/install.sh -q
                        source $JENKINS_HOME/google-cloud-sdk/completion.bash.inc
                        source $JENKINS_HOME/google-cloud-sdk/path.bash.inc
                    else--
                        echo "GCloud CLI is already Installed!"
                        echo ""
                    fi
                '''
            }
        }
        // Logging into GCloud
        stage('Logging into Google Cloud and Get Access Token') {
            steps {
                script {
                    withCredentials([file(credentialsId: '<CREDENTIALS_ID>', variable: 'GOOGLE_SERVICE_ACCOUNT_KEY')]) {
                        sh '${GCLOUD_DIR}/gcloud auth activate-service-account --key-file ${GOOGLE_SERVICE_ACCOUNT_KEY}'
                        env.TOKEN = sh([script: "${GCLOUD_DIR}/gcloud auth print-access-token", returnStdout: true]).trim()
                    }
                }
            }
        }

        stage('Create Alerts') {
            steps {
                script {
                    def selectedAlerts = params.ALERTS_NAME.split(',')
                    for (alerts in selectedAlerts) {
                    sh "${GCLOUD_DIR}/gcloud --quiet alpha monitoring policies create --project=<GCP_PROJECT_NAME> --policy-from-file='${WORKSPACE}/${alerts}.json'"
                    }
                }
            }
        
        }
    }
}