# LAB-8-Analyse-de-posture-et-exposition-d-applications-mobiles-avec-BeVigil-et-Yaazhini

Task 0 — Règles, périmètre et éthique:

1- Création d'un fichier 00-scope/scope.md

<img width="962" height="497" alt="image" src="https://github.com/user-attachments/assets/09793282-3876-4f0a-815e-4199ef15b1fa" />

<img width="958" height="502" alt="image" src="https://github.com/user-attachments/assets/530938d7-76b4-4e95-9915-c42e141b6099" />

<img width="957" height="468" alt="image" src="https://github.com/user-attachments/assets/6f3a2e80-6e87-4424-974f-024d2c553730" />

Task 1 — Préparation du workspace et traçabilité:

1- Création de l'arborescence de dossiers :

<img width="1331" height="962" alt="image" src="https://github.com/user-attachments/assets/03350630-ceef-4164-86b4-de729818f95f" />


2- Création du fichier  analyse_info.txt et le fichier commands.log pour sauvegarder toutes les commandes exécutées:

<img width="965" height="285" alt="image" src="https://github.com/user-attachments/assets/2add383d-9082-4c4a-923f-36cd2d71b0e1" />

Contenu du fichier  analyse_info.txt:

<img width="995" height="381" alt="image" src="https://github.com/user-attachments/assets/fb524fab-906a-4455-9a1a-e42466817596" />

3- # Ajouter cette commande au log: 

"mkdir 00-scope, 01-bevigil, 02-yaazhini, 03-triage, 04-report" 

<img width="961" height="58" alt="image" src="https://github.com/user-attachments/assets/c9e1d517-9316-4b71-885f-84db7c5ff454" />

Le contenu du fichier commands.log

<img width="992" height="221" alt="image" src="https://github.com/user-attachments/assets/1f0435b3-f9fa-4402-89f8-dc0681fcebf2" />


L'arboressance :

<img width="825" height="273" alt="image" src="https://github.com/user-attachments/assets/84019807-da07-4b5d-858b-96afc34c5bbf" />

Task 2 — Préparer l'artefact autorisé

Option A : APK pédagogique fourni ( Diva )

1- Copier l'APK dans le dossier 00-scope

<img width="965" height="52" alt="image" src="https://github.com/user-attachments/assets/4c00ed8d-2f93-4884-b116-6b59f81cf4f1" />

2- Calculer et afficher le hash SHA-256 de l'APK

<img width="962" height="135" alt="image" src="https://github.com/user-attachments/assets/54a082af-c961-4cc9-a7b5-ab12aaccb515" />

3- Mettre à jour analyse_info.txt avec le hash

<img width="965" height="71" alt="image" src="https://github.com/user-attachments/assets/f1227649-f41a-49c8-be5e-13edaa26c5e7" />


<img width="1012" height="307" alt="image" src="https://github.com/user-attachments/assets/f3252d2c-0805-462b-bab9-3fe47b90aeda" />

4- Enregistrer la commande dans le log:

"Copy-Item -Path 'E:\EMSI\4IIR\S2\Securites_des_apps_mobiles\diva-apk-file\DivaApplication.apk' -Destination '00-scope\'" 

"Get-FileHash -Path '00-scope\DivaApplication.apk' -Algorithm SHA256" 

<img width="963" height="117" alt="image" src="https://github.com/user-attachments/assets/ae5dd1ff-589e-410c-971d-58a030babe21" />


<img width="1257" height="206" alt="image" src="https://github.com/user-attachments/assets/9f4fc676-db19-4e7f-b177-501f64b61540" />

Task 3 — Démarrage et prise en main BeVigil




















