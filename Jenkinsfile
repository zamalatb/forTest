pipeline {
    agent { label 'ec2' } 
    stages {
        stage('Import CSV from S3') {
            steps {
                sh '''
                export PGPASSWORD=postgres
                psql -h ${host_ip} -d postgres -U postgres -p 5432 -c "CREATE EXTENSION aws_s3 CASCADE"
                psql -h ${host_ip} -d postgres -U postgres -p 5432 -c "DROP TABLE test"
                psql -h ${host_ip} -d postgres -U postgres -p 5432 -c "CREATE TABLE test (EMPLOYEE_ID varchar(120) NOT NULL, FIRST_NAME varchar(120) NOT NULL,LAST_NAME text,EMAIL text,PHONE_NUMBER text,HIRE_DATE text,JOB_ID text,SALARY text,COMMISSION_PCT text,MANAGER_ID text,DEPARTMENT_ID text)"
                psql -h ${host_ip} -d postgres -U postgres -p 5432 -c "SELECT aws_s3.table_import_from_s3(
                'test', 'EMPLOYEE_ID,FIRST_NAME,LAST_NAME,EMAIL,PHONE_NUMBER,HIRE_DATE,JOB_ID,SALARY,COMMISSION_PCT,MANAGER_ID,DEPARTMENT_ID', '(format csv, header true)',
                    '${S3_path}',
                'employees-1.csv',
                'us-east-2'
                 )"
                 psql -h ${host_ip} -d postgres -U postgres -p 5432 -c "SELECT * FROM test"
                 '''
            }
        }
    }
}
