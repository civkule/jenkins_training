
pipeline {
  agent {label 'D4ML-Builder'}
  stages {
    stage('get information') {
      steps {
        sh ''' #! /bin/bash/
        set +x
        echo "\n------------------- Environment variables ------------------- \n
        $(printenv)"
        echo "\n------------------- Network Information -------------------" \n
        # TOKEN=`curl -X PUT "http://169.254.169.254/latest/api/token" -s -H "X-aws-ec2-metadata-token-ttl-seconds: 180"`
        # echo "PublicIP: $(curl -H "X-aws-ec2-metadata-token: $TOKEN" http://169.254.169.254/latest/meta-data/public-ipv4)"
        # echo "PrivateIP: $(curl -H "X-aws-ec2-metadata-token: $TOKEN" http://169.254.169.254/latest/meta-data/local-ipv4)"
          
        echo "PrivateIP: $(curl -s http://169.254.169.254/latest/meta-data/local-ipv4)" 
        echo "PublicIP: $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4)"
        echo "\n------------------- Instance Hostname ------------------- \n
        $(hostname)"
        echo '\n------------------------ CPU usage--------------------' && lscpu | grep -m 1 'CPU(s)'
          echo "CPU usage: $(grep 'cpu ' /proc/stat | awk '{usage=($2+$4)*100/($2+$4+$5)} END {print usage "%"}')"
        echo "\n------------------- RAM Usage ------------------- \n
        $(cat /proc/meminfo | awk '/^Mem/ { printf("%s\t%0.3f GB\\n", $1, $2/1000000) }')"
        echo "\n-----------------------------------------\n"
        echo 'Obtaining Disc space Usage: '
        df -h /
