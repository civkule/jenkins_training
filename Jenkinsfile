def qsh(i) { // execute sh script without outputting the command itself
	sh """#!/bin/sh
		${i}
	"""
}

def msg(i) {
	sh """#!/bin/sh
		printf -- "\n-------- ${i} --------\n\n"
	"""
}

pipeline {
	agent { label "D4ML-Builder" }
	stages {
		stage("Environment") {
			steps {
				msg("Getting environmental variables")
				qsh('printenv | cat -v | sort # fix escaped variables')
			}
		}
		stage("Network") {
			steps {
				msg("Getting network information")
				qsh('''
					echo "Private IP: $(hostname -I)"
					echo "Public IP: $(curl -s ifconfig.me)"
				''')
			}
		}
		stage("Host name") {
			steps {
				msg("Getting hostname")
				qsh('echo "Hostname: $(hostname)"')
			}
		}
		stage("CPU Usage") {
			steps {
				msg("Getting CPU use percentage")
				qsh(''' # to not break quotes
					vmstat | awk 'END{print 100 - $15"%"}'
				''')
			}
		}
		stage("RAM Usage") {
			steps {
				msg("Getting RAM usage")
				qsh('''
					# workaround to free lacking 1 column on first line
					{ printf :; free -h; } | awk 'NR<=2 { print $3"/"$2 }'
				''')
			}
		}
		stage("Disk Usage") {
			steps {
				msg("Getting drive usage")
				qsh("df --total -h | awk '/^.dev/; NR==1'")
			}
		}
	}
}
