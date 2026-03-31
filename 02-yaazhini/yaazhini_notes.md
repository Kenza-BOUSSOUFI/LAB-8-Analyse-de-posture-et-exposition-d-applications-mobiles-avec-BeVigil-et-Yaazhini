# Notes d'analyse Yaazhini — Application DIVA (DivaApplication.apk)

**Date d'analyse** : 2025  
**Outil** : Yaazhini (Vegabird)  
**Artefact analysé** : DivaApplication.apk  
**Objectif** : Analyse statique de la posture de sécurité de l'APK pédagogique DIVA

---

## Éléments identifiés

### Élément 1: Stockage de données sensibles en clair (High)
- **Localisation** : Section "Vulnerabilities" > High > Insecure Data Storage
- **Description** : L'application stocke des informations sensibles (identifiants, tokens, données utilisateur) en clair dans des fichiers locaux (SharedPreferences, bases de données SQLite non chiffrées, fichiers texte). Ces données sont accessibles sans root sur certains appareils ou via une sauvegarde ADB.
- **Impact potentiel** : Un attaquant ayant accès physique ou logique à l'appareil peut extraire directement les données sensibles sans décompiler l'application. Risque élevé de vol de credentials.
- **Remédiation suggérée** : Utiliser Android Keystore pour stocker les clés cryptographiques. Chiffrer les données sensibles avec AES-256 avant toute persistance locale. Éviter les SharedPreferences pour des secrets.

---

### Élément 2: Injection SQL (High)
- **Localisation** : Section "Vulnerabilities" > High > SQL Injection
- **Description** : Plusieurs requêtes SQL sont construites par concaténation directe de chaînes avec les entrées utilisateur, sans utilisation de requêtes paramétrées (PreparedStatement). Cela expose l'application à des attaques d'injection SQL.
- **Impact potentiel** : Un attaquant peut manipuler les requêtes pour contourner l'authentification, extraire ou modifier l'intégralité de la base de données locale, voire supprimer des enregistrements.
- **Remédiation suggérée** : Remplacer toutes les concaténations de chaînes SQL par des `PreparedStatement` avec des paramètres liés (`?`). Valider et assainir toutes les entrées utilisateur côté application.

---

### Élément 3: Communication réseau non sécurisée — HTTP en clair (Medium)
- **Localisation** : Section "Vulnerabilities" > Medium > Insecure Network Communication / Linked URLs
- **Description** : L'application utilise des URL en HTTP (non chiffré) pour communiquer avec des serveurs distants. Les URLs identifiées incluent des endpoints HTTP sans TLS/SSL. Le flag `android:usesCleartextTraffic="true"` est présent dans le manifeste.
- **Impact potentiel** : Les communications peuvent être interceptées via une attaque Man-in-the-Middle (MITM), exposant les données transmises (identifiants, tokens, données personnelles) à un attaquant sur le même réseau.
- **Remédiation suggérée** : Migrer l'ensemble des communications vers HTTPS (TLS 1.2 minimum, TLS 1.3 recommandé). Supprimer `usesCleartextTraffic` ou le restreindre. Implémenter le Certificate Pinning pour les endpoints critiques.

---

### Élément 4: Permissions excessives déclarées dans le manifeste (Medium)
- **Localisation** : Section "Permissions"
- **Description** : L'application déclare des permissions sensibles non justifiées par ses fonctionnalités pédagogiques, notamment : `READ_CONTACTS`, `ACCESS_FINE_LOCATION`, `READ_EXTERNAL_STORAGE`, `WRITE_EXTERNAL_STORAGE`, `CAMERA`. Ces permissions dépassent largement le périmètre fonctionnel d'une application de démonstration.
- **Impact potentiel** : En cas de compromission de l'application ou d'exploitation d'une vulnérabilité, un attaquant pourrait accéder aux contacts, à la localisation GPS précise, ou aux fichiers du stockage externe de la victime.
- **Remédiation suggérée** : Appliquer le principe du moindre privilège : ne déclarer que les permissions strictement nécessaires. Utiliser les permissions runtime (Android 6.0+) et justifier chaque permission à l'utilisateur au moment de la demande.

---

### Élément 5: Activities exportées sans protection (Medium)
- **Localisation** : Section "Activities"
- **Description** : Plusieurs Activities de l'application sont déclarées avec `android:exported="true"` sans restriction par `android:permission`. Parmi elles : `MainActivity`, ainsi que plusieurs activities liées aux challenges DIVA. N'importe quelle application installée sur l'appareil peut les invoquer directement via un Intent explicite.
- **Impact potentiel** : Un attaquant peut lancer directement des composants internes de l'application, contourner les écrans d'authentification, ou déclencher des comportements non prévus (deeplinks malveillants, accès non autorisé à des fonctionnalités privées).
- **Remédiation suggérée** : Définir `android:exported="false"` pour toute Activity qui n'a pas besoin d'être accessible depuis d'autres applications. Si l'export est nécessaire, protéger l'accès avec une permission personnalisée (`android:permission`).

---

### Élément 6: Bibliothèques tierces obsolètes ou vulnérables (Low)
- **Localisation** : Section "Libraries"
- **Description** : L'analyse des bibliothèques embarquées révèle l'utilisation de dépendances tierces dont les versions sont anciennes et présentent des CVE connues. Les bibliothèques identifiées incluent des versions de support Android obsolètes (pre-AndroidX).
- **Impact potentiel** : Les vulnérabilités connues dans ces bibliothèques peuvent être exploitées si un attaquant cible spécifiquement la version de la bibliothèque utilisée. Risque aggravé si ces bibliothèques traitent des données réseau ou cryptographiques.
- **Remédiation suggérée** : Maintenir un inventaire des dépendances (SBOM). Mettre à jour régulièrement les bibliothèques vers leurs dernières versions stables. Utiliser des outils comme OWASP Dependency-Check ou Gradle's `dependencyUpdates` pour automatiser la détection.

---

### Élément 7: Journalisation excessive (Logging) — Secret potentiel masqué (Low)
- **Localisation** : Section "Vulnerabilities" > Low > Insecure Logging
- **Description** : L'application utilise abondamment `android.util.Log` (Log.d, Log.v, Log.e) pour journaliser des informations de débogage en production. Les logs contiennent des données sensibles telles que : identifiants utilisateur, réponses API, et valeurs de variables critiques. **Secret identifié masqué** : `API_KEY = [REDACTED]` / `password = [REDACTED]`
- **Impact potentiel** : Sur un appareil rooté ou via ADB (`adb logcat`), un attaquant ou une application malveillante avec la permission `READ_LOGS` peut lire en temps réel tous les logs de l'application, incluant les secrets exposés.
- **Remédiation suggérée** : Désactiver ou supprimer tous les appels `Log.*` en build de production (via ProGuard/R8 ou une variable `BuildConfig.DEBUG`). Ne jamais journaliser de données sensibles, même en mode debug.

---

## Résumé des niveaux de criticité

| # | Élément                                      | Sévérité |
|---|----------------------------------------------|----------|
| 1 | Stockage de données sensibles en clair       | 🔴 High  |
| 2 | Injection SQL                                | 🔴 High  |
| 3 | Communication réseau non sécurisée (HTTP)    | 🟠 Medium|
| 4 | Permissions excessives                       | 🟠 Medium|
| 5 | Activities exportées sans protection         | 🟠 Medium|
| 6 | Bibliothèques tierces obsolètes              | 🟡 Low   |
| 7 | Journalisation excessive (secret masqué)     | 🟡 Low   |

---

## Commande PowerShell pour créer ce fichier

```powershell
@"
[contenu du fichier ci-dessus]
"@ | Out-File -FilePath "02-yaazhini\yaazhini_notes.md" -Encoding utf8

# Enregistrer la commande dans le log
Add-Content -Path "commands.log" -Value "$(Get-Date -Format 'yyyy-MM-dd HH:mm:ss') - Created 02-yaazhini\yaazhini_notes.md"
```

---

*Rapport généré dans le cadre du LAB-8 — Analyse de posture et exposition d'applications mobiles*  
*Artefact : DIVA (Damn Insecure and Vulnerable App) — Usage strictement pédagogique*
