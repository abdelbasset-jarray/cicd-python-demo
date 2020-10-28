# Openshift Multi Project CI CD Python-demo
Exemple : Openshift Multi Project CI CD Python

I- Configurez l'environnement OpenShift avec les projets suivants:

	oc new-project cicd --description ="Démo du pipeline CI / CD"
	oc new-project cicd-dev --description ="CI / CD - Dev"
	oc new-project cicd-prod --description ="CI / CD - Prod"
	oc new-project cicd-staging --description ="CI / CD - Staging"
				
II- Créer un compte de service jenkins dans le projet cicd
                
	oc create serviceaccount jenkins -n cicd
		
III- Donner l'accès en modification au compte de service jenkins aux autres projets:

        oc policy add-role-to-user edit system:serviceaccount:cicd:jenkins -n cicd-dev
        oc policy add-role-to-user edit system:serviceaccount:cicd:jenkins -n cicd-prod
        oc policy add-role-to-user edit system:serviceaccount:cicd:jenkins -n cicd-staging 

IV- Déployer le pipeline OpenShift à partir du référentiel Git
      
        oc new-app   https://github.com/abdoujarray/cicd-python-demo.git -n cicd

V- Démarrer une nouvelle construction de pipeline 
      
      1- Depuis interface graphique: Application Console> Project cicd> Builds> Pipelines -> cliquez sur le bouton Start Pipeline
      
      2- CLI:  oc start-build podcicd -n cicd

VI- Remarque: Si vous recevez une erreur similaire à la suivante lors de l'accès à la route jenkins:
          
	  {"error":"server_error","error_description":"The authorization server encountered an unexpected condition that prevented it from fulfilling the request.","state":"N2UwMTkwNzktYjBmNi00"}
	  

 Exécutez la commande suivante et réessayez:
 
          oc annotate sa jenkins serviceaccounts.openshift.io/oauth-redirectreference.jenkins='{"kind":"OAuthRedirectReference","apiVersion":"v1","reference":{"kind":"Route","name":"jenkins"}}' -n cicd 

VII- Pour nettoyer / désinstaller la demo, supprimez les projets:

       oc delete project cicd-prod
       oc delete project cicd-staging
       oc delete project cicd-dev
       oc delete project cicd 
  
  
    
      

