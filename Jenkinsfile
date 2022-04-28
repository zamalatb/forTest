pipeline {
    agent { label 'ec2' } 
    stages {
        stage('Import CSV from S3') {
            steps {
                sh '''
                export PGPASSWORD=postgres
                psql -h ${host_ip} -d postgres -U postgres -p 5432 -c "DROP TABLE IF EXISTS test"
                psql -h ${host_ip} -d postgres -U postgres -p 5432 -c "CREATE TABLE test (EMPLOYEE_ID varchar(120) NOT NULL, FIRST_NAME varchar(120) NOT NULL,LAST_NAME text,EMAIL text,PHONE_NUMBER text,HIRE_DATE text,JOB_ID text,SALARY text,COMMISSION_PCT text,MANAGER_ID text,DEPARTMENT_ID text)"
                aws s3 cp ${s3_path} - | psql -h ${host_ip} -d postgres -p 5432 -c "copy test(f1, f2) from stdin with(format csv)
                 psql -h ${host_ip} -d postgres -U postgres -p 5432 -c "SELECT * FROM test"
                 '''
            }
        }
    }
}
