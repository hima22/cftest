node {
        def DO_NOT_ROLLBACK = "--on-failure DO_NOTHING"
        if (Action == 'update') {
          DO_NOT_ROLLBACK = ""
        }
        stage('pull code from scm'){
            checkout scm
       }
        stage('service and cluster update'){
            sh 'aws s3 cp ./cloudformation/templates/VPC1.yml s3://newcftest/cloudformation/templates/VPC1.yml'
            sh 'aws s3 cp ./cloudformation/parameters/Dev/VPC1.json s3://newcftest/cloudformation/templates/VPC1.json'
            sh 'aws s3 cp ./Jenkinsfile s3://newcftest/'
        }

        stage('microservice'){
            sh 'aws --region us-east-1 cloudformation ${Action}-stack --stack-name ${Environment}-${Stack} --template-body file://./cloudformation/templates/VPC1.yml --parameters file://./cloudformation/parameters/${Environment}/VPC1.json ${DO_NOT_ROLLBACK}'
       }

        stage('Stack Status'){
            sh 'aws --region us-east-1 cloudformation wait stack-${Action}-complete --stack-name ${Environment}-${Stack}'
   }
}
