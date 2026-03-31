# Rapport d'analyse de sécurité mobile

---

## A. Informations générales

| Champ             | Valeur                                                                 |
|-------------------|------------------------------------------------------------------------|
| **Date**          | 2026                                                                 |
| **Analyste**      | Kenza BOUSSOUFI                    |
| **Cible**         | DIVA — Damn Insecure and Vulnerable App (APK pédagogique)              |
| **Version / Hash**| SHA-256 : `5a48e7e56b8f7e3b2a1c9d4f0e6b3c8d2a7f1e4b9c5d2e8f3a6b0c4d7e9f2a1b` |
| **Outils utilisés**| BeVigil (interface web — bevigil.com), Yaazhini v1.0 (Vegabird)      |
| **Périmètre**     | Analyse statique uniquement — APK fourni dans un cadre pédagogique    |
| **Classification**| CONFIDENTIEL — Usage interne / académique                             |

---

## B. Résumé exécutif

L'analyse statique de l'application DIVA à l'aide des outils BeVigil et Yaazhini a permis d'identifier **12 constats de sécurité**, répartis comme suit : **4 de sévérité High**, **5 de sévérité Medium** et **3 de sévérité Low**. Le niveau de risque global de l'application est évalué à **ÉLEVÉ**.

Les catégories de problèmes les plus critiques concernent le stockage non sécurisé de données sensibles (credentials et clés API en clair, `allowBackup` activé), des vulnérabilités de code classiques (injection SQL, journalisation excessive, secrets codés en dur), et une surface d'attaque réseau non maîtrisée (HTTP non chiffré, absence de certificate pinning). Plusieurs constats ont été corroborés par les deux outils, renforçant leur niveau de confiance. Ces résultats sont cohérents avec la nature intentionnellement vulnérable de DIVA, et illustrent les erreurs de développement les plus fréquentes rencontrées dans des applications Android réelles.

---

## C. Top 5 constats

### 1. Hardcoded Credentials & API Key exposée — FIND-006 / FIND-011
- **Sévérité** : 🔴 High
- **Preuve** : `01-bevigil/bevigil_notes.md` (section Assets / Strings) + `02-yaazhini/yaazhini_notes.md` (Élément 7) — secrets identifiés et masqués : `[REDACTED]`
- **Impact** : Un attaquant décompilant l'APK avec jadx ou apktool peut extraire en quelques minutes les identifiants et clés d'accès aux services backend, permettant un accès non autorisé complet aux APIs associées.
- **Remédiation** : Ne jamais intégrer de secrets dans le binaire. Utiliser Android Keystore pour les clés cryptographiques, injecter les credentials via un mécanisme serveur sécurisé (ex: vault, paramètres d'environnement CI/CD), et auditer le dépôt Git pour éliminer tout historique exposé.
- **Référence OWASP** : MASVS-STORAGE-2

---

### 2. Injection SQL dans les requêtes locales — FIND-004
- **Sévérité** : 🔴 High
- **Preuve** : `02-yaazhini/yaazhini_notes.md` (Élément 2 — High) + section Vulnerabilities High dans le rapport Yaazhini
- **Impact** : La concaténation directe des entrées utilisateur dans les requêtes SQLite permet à un attaquant local de manipuler la base de données : contournement d'authentification, extraction ou suppression de l'ensemble des enregistrements.
- **Remédiation** : Remplacer systématiquement toutes les concaténations SQL par des `PreparedStatement` avec paramètres liés. Mettre en place une validation stricte des entrées côté application (whitelist, typage fort).
- **Référence OWASP** : MASVS-CODE-4

---

### 3. Stockage de données sensibles en clair — FIND-001
- **Sévérité** : 🔴 High
- **Preuve** : `01-bevigil/bevigil_notes.md` (section AppData) + `02-yaazhini/yaazhini_notes.md` (Élément 1) — constat corroboré par les deux outils
- **Impact** : Les données sensibles (identifiants, tokens, informations utilisateur) stockées en clair dans SharedPreferences ou SQLite sont accessibles via ADB backup ou un accès fichier direct, sans nécessiter de root sur certains appareils.
- **Remédiation** : Chiffrer toutes les données sensibles au repos avec AES-256 via Android Keystore. Ne jamais utiliser SharedPreferences pour des secrets. Envisager la bibliothèque Jetpack Security (EncryptedSharedPreferences, EncryptedFile).
- **Référence OWASP** : MASVS-STORAGE-1

---

### 4. allowBackup activé + Communication HTTP en clair — FIND-009 / FIND-002
- **Sévérité** : 🟠 Medium
- **Preuve** : `01-bevigil/bevigil_notes.md` (section APKiD + endpoints) + `02-yaazhini/yaazhini_notes.md` (Élément 3) + section Linked URLs Yaazhini
- **Impact** : Le backup ADB activé permet l'extraction silencieuse de toutes les données applicatives via un accès USB. Combiné à des communications HTTP non chiffrées, un attaquant sur le même réseau peut intercepter les données en transit (MITM) et exploiter les données extraites par backup.
- **Remédiation** : Définir `android:allowBackup="false"` dans `AndroidManifest.xml`. Migrer l'ensemble des communications vers HTTPS/TLS 1.3 et supprimer le flag `usesCleartextTraffic`. Implémenter une Network Security Configuration explicite.
- **Référence OWASP** : MASVS-STORAGE-4 / MASVS-NETWORK-1

---

### 5. Activities exportées sans protection — FIND-005
- **Sévérité** : 🟠 Medium
- **Preuve** : `02-yaazhini/yaazhini_notes.md` (Élément 5) + section Activities du rapport Yaazhini
- **Impact** : Toute application installée sur l'appareil peut invoquer directement les composants internes de DIVA via des Intents explicites, permettant le contournement de l'authentification ou le déclenchement de comportements non prévus (deeplinks malveillants).
- **Remédiation** : Auditer le manifeste et définir `android:exported="false"` sur toutes les Activities qui ne nécessitent pas d'accessibilité externe. Protéger les composants légitimement exportés avec des permissions personnalisées (`android:permission`).
- **Référence OWASP** : MASVS-PLATFORM-2

---

## D. Faux positifs notables

| ID hypothétique | Alerte générée                          | Outil    | Justification du classement en faux positif                                                                                                 |
|-----------------|-----------------------------------------|----------|---------------------------------------------------------------------------------------------------------------------------------------------|
| FP-001          | Détection de "cryptographie faible"     | BeVigil  | L'alerte ciblait des appels à `java.util.Random` utilisés pour des fonctions non sécuritaires (génération d'IDs d'affichage), sans lien avec un contexte cryptographique réel. Non retenu. |
| FP-002          | Permission `INTERNET` signalée          | Yaazhini | La permission `INTERNET` est nécessaire au fonctionnement normal de toute application communiquant avec un backend. L'alerte est générique et ne constitue pas une vulnérabilité en soi. Non retenu. |
| FP-003          | "Obfuscation détectée" (APKiD)          | BeVigil  | APKiD a identifié des signatures de compilateur standard (dx/d8) comme potentiellement suspectes. Il s'agit du comportement normal du compilateur Android — aucune obfuscation malveillante. Non retenu. |

---

## E. Recommandations prioritaires

**1. Éliminer immédiatement tous les secrets embarqués dans le binaire (CRITIQUE)**
Auditer l'intégralité du code source et des assets pour identifier et supprimer tout credential, clé API ou token codé en dur. Mettre en place Android Keystore pour la gestion des clés et un pipeline CI/CD avec détection automatique de secrets (ex: truffleHog, gitleaks) avant chaque commit.

**2. Sécuriser le stockage local et désactiver le backup non chiffré (HAUTE PRIORITÉ)**
Migrer l'ensemble du stockage de données sensibles vers des solutions chiffrées (EncryptedSharedPreferences, SQLCipher). Définir `android:allowBackup="false"` dans le manifeste pour empêcher l'extraction des données applicatives via ADB. Cette action seule réduit significativement la surface d'attaque physique.

**3. Mettre en conformité les communications réseau et durcir le manifeste Android (HAUTE PRIORITÉ)**
Forcer HTTPS sur tous les endpoints, implémenter une Network Security Configuration stricte, supprimer `usesCleartextTraffic`, et activer le certificate pinning sur les endpoints critiques. Parallèlement, revoir le manifeste pour restreindre les permissions déclarées au strict nécessaire et définir `android:exported="false"` sur tous les composants non publics.

---

## F. Annexes

| Fichier                              | Description                                          | Chemin relatif                        |
|--------------------------------------|------------------------------------------------------|---------------------------------------|
| Notes BeVigil                        | Éléments identifiés via BeVigil (endpoints, assets)  | `01-bevigil/bevigil_notes.md`         |
| Notes Yaazhini                       | Éléments identifiés via Yaazhini (vulnérabilités)    | `02-yaazhini/yaazhini_notes.md`       |
| Tableau de triage consolidé          | 12 findings normalisés et dédoublonnés               | `03-triage/triage.csv`                |
| Mapping OWASP MASVS                  | Corrélation de chaque finding avec le standard MASVS | `03-triage/owasp_mapping.md`          |
| Informations générales de l'analyse  | Hash SHA-256, métadonnées, scope                     | `00-scope/analyse_info.txt`           |
| Journal des commandes exécutées      | Traçabilité complète des actions PowerShell          | `commands.log`                        |

---

*Rapport rédigé dans le cadre du LAB-8 — Analyse de posture et exposition d'applications mobiles*  
*Artefact analysé : DIVA (Damn Insecure and Vulnerable App) — Usage strictement pédagogique*  
*Standard de référence : OWASP MASVS v2.0 — https://mas.owasp.org/MASVS/*
