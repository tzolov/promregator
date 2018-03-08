#!groovy

timestamps {
	node("slave") {
		dir("build") {
			git url: 'https://github.com/promregator/promregator.git'
			
			stage("Build") {
				sh """#!/bin/bash -xe
				export CF_PASSWORD=dummypassword
				mvn -U -B clean package
				"""
			}
			
			stage("Post-processing quality data") {
				junit 'target/surefire-reports/*.xml'
				
				step([
					$class: 'FindBugsPublisher',
					pattern: '**/findbugsXml.xml',
					failedTotalAll: '100'
				])
				
				step([
					$class: 'PmdPublisher',
					failedTotalAll: '100'
				])
				
				step([
					$class: 'JacocoPublisher'
				])
			}
			
			stage("Archive") {
				archiveArtifacts 'target/promregator*.jar'
			}
			
		}
	}
}