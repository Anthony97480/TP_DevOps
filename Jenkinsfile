pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Code source récupéré depuis GitHub'
            }
        }

        stage('Build calc-service') {
            steps {
                dir('calc-service') {
                    sh 'docker build -t 192.168.10.211:5000/calculator-calc:latest .'
                }
            }
        }

        stage('Build sum-service') {
            steps {
                dir('sum-service') {
                    sh 'docker build -t 192.168.10.211:5000/calculator-sum:latest .'
                }
            }
        }

        stage('Build sub-service') {
            steps {
                dir('sub-service') {
                    sh 'docker build -t 192.168.10.211:5000/calculator-sub:latest .'
                }
            }
        }

        stage('Build mul-service') {
            steps {
                dir('mul-service') {
                    sh 'docker build -t 192.168.10.211:5000/calculator-mul:latest .'
                }
            }
        }

        stage('Build div-service') {
            steps {
                dir('div-service') {
                    sh 'docker build -t 192.168.10.211:5000/calculator-div:latest .'
                }
            }
        }
	
	stage('Push images') {
	    steps {
		sh 'docker push 192.168.10.211:5000/calculator-calc:latest'
		sh 'docker push 192.168.10.211:5000/calculator-sum:latest'
		sh 'docker push 192.168.10.211:5000/calculator-sub:latest'
		sh 'docker push 192.168.10.211:5000/calculator-mul:latest'
		sh 'docker push 192.168.10.211:5000/calculator-div:latest'
	    }
	}
	
	stage('Diagnostic Kubernetes') {
    steps {
        sh '''
            echo "Utilisateur actuel :"
            whoami

            echo "HOME :"
            echo $HOME

            echo "KUBECONFIG :"
            echo $KUBECONFIG

            echo "Contexte Kubernetes :"
            kubectl config current-context

            echo "Adresse du cluster :"
            kubectl config view --minify \
                -o jsonpath='{.clusters[0].cluster.server}'

            echo ""

            echo "Variables proxy :"
            env | grep -i proxy || true
        '''
    }
}

	stage('Deploy to Kubernetes') {
	    steps {
		sh '''
		    kubectl apply -f kubernetes/calc-deployment.yaml
		    kubectl apply -f kubernetes/sum-deployment.yaml
		    kubectl apply -f kubernetes/sub-deployment.yaml
		    kubectl apply -f kubernetes/mul-deployment.yaml
		    kubectl apply -f kubernetes/div-deployment.yaml
		    kubectl apply -f kubernetes/calc-service.yaml
		    kubectl apply -f kubernetes/sum-service.yaml
		    kubectl apply -f kubernetes/sub-service.yaml
		    kubectl apply -f kubernetes/mul-service.yaml
		    kubectl apply -f kubernetes/div-service.yaml
		'''
	    }
	}
    }

    post {
        success {
            echo 'CI : toutes les images ont été construites avec succès !'
        }
        failure {
            echo 'CI : échec du pipeline.'
        }
    }
}
