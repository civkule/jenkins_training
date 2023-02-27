pipeline {
  agent {label "D4ML-Builder"}
  stages {
    stage('Get variables') {
      steps {
        sh '''#!/bin/bash
        
        echo "\n------------------- Environment variables ------------------- \n$(printenv)"

        '''
      }
    }
    stage('Get Network') {
      steps {
        sh '''#!/bin/bash
        
        apt update >/dev/null 2>&1
        apt install -y net-tools >/dev/null 2>&1
        
        echo "\n------------------- Network Information -------------------"
        /sbin/ifconfig | grep inet \n

        TOKEN=`curl -s -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600"`
        PUBLIC_IP=`curl -s -H "X-aws-ec2-metadata-token: $TOKEN" http://169.254.169.254/latest/meta-data/public-ipv4`
        PRIVATE_IP=`curl -s -H "X-aws-ec2-metadata-token: $TOKEN" http://169.254.169.254/latest/meta-data/local-ipv4`

        echo "Public IP address: $PUBLIC_IP"
        echo "Private/Lan IP address: $PRIVATE_IP"
 
        '''
      }
    }
    stage('Get Hostname') {
      steps {
        sh '''#!/bin/bash
        
        echo "\n------------------- Instance Hostname ------------------- \n $(hostname)"
      }
    }
    stage('Get CPU Usage') {
      steps {
        echo '\n------------------------ CPU usage--------------------' && lscpu | grep -m 1 'CPU(s)'
          echo "CPU usage: $(grep 'cpu ' /proc/stat | awk '{usage=($2+$4)*100/($2+$4+$5)} END {print usage "%"}')"

        
        '''
      }
    }
    stage('Get RAM Usage') {
      steps {
        sh '''#!/bin/bash

        echo "\n------------------- RAM Usage ------------------- \n$(cat /proc/meminfo | awk '/^Mem/ { printf("%s\t%0.3f GB\\n", $1, $2/1000000) }')"
        
        '''
      }
    }
    stage('Get Disk Space Usage') {
      steps {
        sh '''#!/bin/bash
        
        echo "\n------------------- Disk space usage ------------------- \n$(df -h)"
        
        '''
      }
    }
  }
}
