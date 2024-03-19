pipeline{

      agent any
      parameters {
        choice choices: ['chrome', 'firefox'], description: 'Select the browser', name: 'BROWSER'
      }

      stages{

          stage('Start Grid'){
              steps{
                   bat "docker-compose -f grid.yaml up  --scale ${params.BROWSER}=1 -d"
                   script{
                       if(fileExists('output/flight-reservation/testng-failed.xml') ||fileExists('output/flight-reservation/testng-failed.xml'))
                          error('failed tests found')

                                        }
              }

          }
           stage('Run Tests'){
                steps{

                     bat "docker-compose -f test-suites.yaml up --pull=always"
                     script{
                         if(fileExists('output/flight-reservation/testng-failed.xml') ||fileExists('output/flight-reservation/testng-failed.xml'))
                         error('failed tests found')

                     }
              }

           }
      }
    post{
         always{
         bat "docker-compose -f grid.yaml down"
         bat "docker-compose -f test-suites.yaml down"
         bat "docker image rm shaarv70/selenium:latest"
         archiveArtifacts artifacts: 'output/flight-reservation/emailable-report.html', followSymlinks:false
         archiveArtifacts artifacts: 'output/vendor-portal/emailable-report.html', followSymlinks:false
         }
      }
}