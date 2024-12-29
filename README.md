docker composee

version: '3'

services:
  # Database
  db:
    image: mysql:5.7
    volumes:
      - db_data:/var/lib/mysql
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: password
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpress
    networks:
      - wpsite
  # Wordpress
  wordpress:
    depends_on:
      - db
    image: wordpress:latest
    ports:
      - '8000:80'
    restart: always
    volumes: ['./:/var/www/html']
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: wordpress
    networks:
      - wpsite
networks:
  wpsite:
volumes:
  db_data:




Script:
pipeline {
   agent any
   tools {
     maven 'MAVEN_HOME'
   }
   stages {
     stage('git repo & clean') {
       steps {
         bat "rmdir /s /q SampleMavenJavaProject"
         bat "git clone https://github.com/budarajumadhurika/SampleMavenJavaProject.git"
         bat "mvn clean -f SampleMavenJavaProject"
       }
     }
     stage('install') {
       steps {
           bat "mvn install -f SampleMavenJavaProject"
       }
     }
     stage('test') {
       steps {
         bat "mvn test -f SampleMavenJavaProject"
       }
     }
     stage('package') {
       steps {
           bat "mvn package -f SampleMavenJavaProject"
       }
     }
   }
}
