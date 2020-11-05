pipeline{
    agent any
    stages{
       stage("compile"){
           agent{
               docker{
                   image 'python:alpine'
               }
           }
           steps{
               withEnv(["HOME=${env.WORKSPACE}"]) {
                    sh 'pip install -r requirements.txt'
                    sh 'python -m py_compile src/*.py'
                    stash(name: 'compilation_result', includes: 'src/*.py*')
                }
           }
       }
    }
}"3.7"

services:
    database:
        image: mysql:5.7
        volumes:
          - /my/own/datadir:/var/lib/mysql
        environment:
            MYSQL_ROOT_PASSWORD: R1234r
            MYSQL_DATABASE: phonebook
            MYSQL_USER: clarusway
            MYSQL_PASSWORD: Clarusway
        networks:
            - clarusnet
    app:
        image: 046402772087.dkr.ecr.us-east-1.amazonaws.com/matt/handson-jenkins:latest
        restart: always
        environment:
            MYSQL_DATABASE_HOST: database
            MYSQL_DATABASE_USER: clarusway
            MYSQL_DATABASE_PASSWORD: Clarusway
            MYSQL_DATABASE_DB: phonebook
            MYSQL_DATABASE_PORT: 3306
        depends_on:
            - database
        ports:
            - "80:80"
        networks:
            - clarusnet
networks:
    clarusnet:
        driver: bridge


