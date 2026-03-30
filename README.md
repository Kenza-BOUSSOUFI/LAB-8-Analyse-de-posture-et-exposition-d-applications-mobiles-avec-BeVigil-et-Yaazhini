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

1. On va accéder à BeVigil (interface web ) via l'URL : https://bevigil.com/

2. Aprés on va clicker sur "Scan App" du NavBar, et on va choisir l'option 2 ( scan .apk file), puis l'importation de l'APK:

<img width="1901" height="875" alt="image" src="https://github.com/user-attachments/assets/c734b2ab-d9e1-41d2-b07d-6bfa0ec5c81f"/>

<img width="1790" height="875" alt="image" src="https://github.com/user-attachments/assets/a16b76ea-045d-4916-b8de-7ee05138ac39" />

3. Ensuite, on va cliquer sur "view report" et on va explorer les différentes sections de l'interface:

<img width="1797" height="757" alt="image" src="https://github.com/user-attachments/assets/39b68483-a9e5-449e-8d38-b12f6a5516bd" />

Summary:

<img width="1801" height="742" alt="image" src="https://github.com/user-attachments/assets/b9acc4fc-b29e-4fd2-bb3e-7744c4810c23" />

<img width="1896" height="811" alt="image" src="https://github.com/user-attachments/assets/7234170e-b9eb-49b6-9bb6-fab66fa84e61" />

<img width="1911" height="672" alt="image" src="https://github.com/user-attachments/assets/2aa1aae3-996e-43e4-9564-905ed16a68ff" />


Les vulnérabilités trouvées:

<img width="1895" height="707" alt="image" src="https://github.com/user-attachments/assets/e62e0a85-9c24-4138-818d-992f5f6f3295" />

Strings:

<img width="1892" height="436" alt="image" src="https://github.com/user-attachments/assets/9bee5979-3420-4840-9b25-107bf2aed741" />

Assets:

<img width="1888" height="577" alt="image" src="https://github.com/user-attachments/assets/00de06e6-f23f-4d2e-ac22-fefc23575ce9" />

APKiD:

<img width="1893" height="401" alt="image" src="https://github.com/user-attachments/assets/5d2ac099-899d-4a6d-9aec-34bea5654c17" />



Permissions:

<img width="1892" height="420" alt="image" src="https://github.com/user-attachments/assets/27076599-9fad-455c-a503-6232d80d2dea" />

<img width="1892" height="366" alt="image" src="https://github.com/user-attachments/assets/167147ec-dcdf-45d3-a1aa-62864531a92b" />

<img width="1893" height="556" alt="image" src="https://github.com/user-attachments/assets/173e49e1-b373-4fb9-abbd-79dcf74dd948" />

AppData:

<img width="1897" height="813" alt="image" src="https://github.com/user-attachments/assets/d090b365-32c3-41f6-ae25-d436949c435d" />

4. On va exporter les résultats en cliquant sur le boutton "Download report"

<img width="1032" height="192" alt="image" src="https://github.com/user-attachments/assets/8bb2d0f0-9c6d-4e25-a8ed-0f4e5bd53c94" />

ils ne donne pas report 

<img width="1772" height="977" alt="image" src="https://github.com/user-attachments/assets/a6b3b3fc-a618-4281-95c9-5c418d62da4d" />
































