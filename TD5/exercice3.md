## Exercice 3

	joaobrilhante@GlaDOS:~$ netcat www.i3s.unice.fr 80

	GET /~boyenval/ HTTP/1.1
	Host: www.i3s.unice.fr

	HTTP/1.1 200 OK
	Date: Mon, 26 Nov 2018 11:24:16 GMT
	Server: Apache
	Transfer-Encoding: chunked
	Content-Type: text/html; charset=utf-8

	ef5
	<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
	<html>
		<head>
		...
		</head>
		<body>
		...
		</body>
	</html>

Nous constatons que les feuilles de styles, les images, les scripts de la page ne
sont pas téléchargés par la commande netcat. Il faudrait envoyer une requête pour
chaque ressource manquante. D'où la valeur ajoutée d'un navigateur qui réalise
cette tâche pour nous et affiche la page web.

En analysant l'en-tête de la réponse, nous constatons que la requête a été acceptée.
De plus, nous remarquons que le serveur web utilise Apache et nous renvoie un
texte en html en utilisant un encodage de transfert en blocs permettant de
transmettre des données sans connaître à l'avance la taille totale des données.

	joaobrilhante@GlaDOS:~$ netcat www.i3s.unice.fr 80
	GET /~test/ HTTP/1.1
	Host: www.i3s.unice.fr

	HTTP/1.1 404 Not Found
	Date: Mon, 26 Nov 2018 12:17:40 GMT
	Server: Apache
	X-Content-Type-Options: nosniff
	Expires: Sun, 19 Nov 1978 05:00:00 GMT
	Cache-Control: no-cache, must-revalidate
	X-Content-Type-Options: nosniff
	Content-Language: fr
	X-Frame-Options: SAMEORIGIN
	X-Generator: Drupal 7 (http://drupal.org)
	Transfer-Encoding: chunked
	Content-Type: text/html; charset=utf-8

	3dea
	<!DOCTYPE html>
	<html>
		<head>
		...
		</head>
		<body class="html not-front not-logged-in no-sidebars page-test i18n-fr">
			...
			<div id="content">
				<div id="breadcrumbs"></div>
					<section id="post-content" role="main">
						<h1 class="page-title">Page non trouvée</h1>
						<div class="region region-content">
							<div id="block-system-main" class="block block-system">
								<div class="content">
									La page demandée "/~test/" n'a pas pu être trouvée.
								</div>
							</div>
						</div>
					</section>
				</div>
			</div>
			...
		</body>
	</html>

En envoyant une requête au serveur pour une page inexistante, nous recevons une
page d'erreur. En effet, en analysant l'en-tête de la réponse du serveur, nous
constatons qu'il y a une erreur permanente. Le serveur n'a pas réussi à trouver
la ressource demandée.