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

<img width="1252" height="202" alt="image" src="https://github.com/user-attachments/assets/c0fb2548-d70f-4176-9ef7-cf916fa0d8e4" />


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

# Task 4 — Collecte BeVigil : endpoints, domaines, emails, technologies 

Maintenant, on va créer un fichier 01-bevigil/bevigil_notes.md structuré qui contient la documentation des éléments identifiés:

<img width="1901" height="981" alt="image" src="https://github.com/user-attachments/assets/ecbe76c9-8c00-4bc9-9b34-ae6583cf3715" />

Puis on va Enregistrer la commande dans le log:

<img width="1287" height="81" alt="image" src="https://github.com/user-attachments/assets/386bf0cc-d5be-48c7-9e74-390bdde22070" />

<img width="1252" height="267" alt="image" src="https://github.com/user-attachments/assets/05e529f0-0717-444c-bc41-76d18d189849" />

# Task 5 — Démarrage et prise en main Yaazhini 

Aprés l'installation de Yaazhini par ce site https://www.vegabird.com/yaazhini/, on va le lancer et démarrer l'analyse de l'APK DIVA:

<img width="1123" height="747" alt="image" src="https://github.com/user-attachments/assets/516eba02-b8fb-4f1c-90e4-cb2cab918ac4" />

<img width="1907" height="797" alt="image" src="https://github.com/user-attachments/assets/ce66389a-2b7f-41b2-baae-33f6d3c1ee53" />

Les vulnérabilités trouvées:

1. High

<img width="1609" height="958" alt="image" src="https://github.com/user-attachments/assets/b577b744-552e-4a04-8fe8-9cc12465452e" />

<img width="1587" height="532" alt="image" src="https://github.com/user-attachments/assets/bccad7cd-a518-4a72-aeda-a9d54b68756c" />

2. Medium

<img width="1601" height="950" alt="image" src="https://github.com/user-attachments/assets/df6791f7-2592-4a55-9010-5a75784d9ca9" />

<img width="1591" height="527" alt="image" src="https://github.com/user-attachments/assets/8e57e502-1698-4599-b37a-d4d1cf23c32f" />

<img width="1601" height="488" alt="image" src="https://github.com/user-attachments/assets/9535ca6c-11a5-4d18-b061-78dcb62865b2" />

<img width="1582" height="526" alt="image" src="https://github.com/user-attachments/assets/6945f951-dc59-4498-8fbd-c906c7056a5f" />

<img width="1598" height="965" alt="image" src="https://github.com/user-attachments/assets/89e052df-eabd-4f54-969b-f370340f7c1a" />

<img width="1586" height="507" alt="image" src="https://github.com/user-attachments/assets/0e7f3e54-defc-4650-a65d-5e8137e37c6a" />

3. Low:

<img width="1600" height="957" alt="image" src="https://github.com/user-attachments/assets/e8da160f-080f-4c52-9336-9630d8b85efe" />

<img width="1588" height="531" alt="image" src="https://github.com/user-attachments/assets/a1036c01-7e7c-44e8-93bc-0caf02b21953" />


Linked URL's:

<img width="1915" height="446" alt="image" src="https://github.com/user-attachments/assets/a3823eea-32f5-4b6f-98c9-b5cf6f24dc9f" />

Libraries:

<img width="1200" height="333" alt="image" src="https://github.com/user-attachments/assets/ed4ee876-5466-4ae9-8f6d-5aa1f733bec5" />

Permissions:

<img width="1598" height="297" alt="image" src="https://github.com/user-attachments/assets/f8a364c5-0c9c-4f60-b068-3671df3d6e74" />

Activities:

<img width="1601" height="951" alt="image" src="https://github.com/user-attachments/assets/306e4b83-6968-49e8-b359-27045addb05d" />



# Checklist de fin

- Scope clairement défini et respecté
  
- Informations de traçabilité complètes

- Hash de l'APK documenté

- Fichier notes  BeVigil sauvegardé

- Fichier notes  Yaazhini sauvegardé

- Notes d'analyse complètes

- Triage.csv rempli avec tous les constats

- Mapping OWASP réalisé pour au moins 5 constats

- Rapport final complet et structuré

- Aucun secret exposé dans les fichiers

- Aucune donnée personnelle exposée

- Aucune technique d'exploitation documentée





















































