# cicd-python-demo
Exemple : Openshift Multi Project CI CD Python

I- Configurez l'environnement OpenShift avec les projets suivants:

				oc new-project cicd --description = "Démo du pipeline CI / CD"
				oc new-project cicd-dev --description = "CI / CD - Dev"
				oc new-project cicd-prod --description = "CI / CD - Prod"
				oc new-project cicd-staging --description = "CI / CD - Staging"
				
II- Créer un compte de service jenkins dans le projet cicd
                
				oc create serviceaccount jenkins -n cicd
		
	
