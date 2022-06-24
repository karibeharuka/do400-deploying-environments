pipeline {
    agent {
        node {
            label 'maven'
        }
    }

    stages {
        stage('Tests') {
            steps {
                sh './mvnw clean test'
            }
        }
        stage('Package') {
            steps {
                sh '''
                    ./mvnw package -DskipTests \
                    -Dquarkus.package.type=uber-jar
                '''
                archiveArtifacts 'target/*.jar'
            }
        }
        stage('Build Image') {
            environment { QUAY = credentials('QUAY_USER') }
            steps {
                sh '''
                    ./mvnw quarkus:add-extension \
                    -Dextensions="kubernetes,container-image-jib"
                    '''
                    sh '''
                    ./mvnw package -DskipTests \
                    -Dquarkus.jib.base-jvm-image=quay.io/redhattraining/do400-java-alpine-openjdk11-jre:latest \
                    -Dquarkus.container-image.build=true \
                    -Dquarkus.container-image.registry=quay.io \
                    -Dquarkus.container-image.group=kariharu0000 \
                    -Dquarkus.container-image.name=do400-deploying-environments \
                    -Dquarkus.container-image.username=kariharu0000 \
                    -Dquarkus.container-image.password="Kariharu0001" \
               
                    -Dquarkus.container-image.push=true
                '''
            }
        }
    }
}