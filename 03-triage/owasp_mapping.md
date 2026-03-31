# Mapping OWASP MASVS — DivaApplication.apk

**Standard de référence** : OWASP MASVS v2.0  
**Guide de test associé** : OWASP MASTG  
**Date** : 2026  
**Source des constats** : triage.csv (FIND-001 à FIND-012)

---

## FIND-001 : Stockage de données sensibles en clair
- **Catégorie OWASP** : MASVS-STORAGE
- **Référence spécifique** : MASVS-STORAGE-1
- **Justification** : Les données sensibles (credentials, tokens) stockées sans chiffrement dans SharedPreferences ou SQLite violent l'exigence de protection des données au repos. Toute donnée sensible persistée localement doit être chiffrée.

---

## FIND-002 : Communication réseau en clair (HTTP)
- **Catégorie OWASP** : MASVS-NETWORK
- **Référence spécifique** : MASVS-NETWORK-1
- **Justification** : L'utilisation de HTTP non chiffré et le flag `usesCleartextTraffic=true` exposent les données en transit à une interception MITM, en violation directe de l'exigence de chiffrement des communications réseau.

---

## FIND-003 : Permissions excessives déclarées
- **Catégorie OWASP** : MASVS-PLATFORM
- **Référence spécifique** : MASVS-PLATFORM-1
- **Justification** : La déclaration de permissions non nécessaires au fonctionnement de l'application (LOCATION, CAMERA, CONTACTS) va à l'encontre du principe du moindre privilège requis par le standard pour l'interaction avec la plateforme Android.

---

## FIND-004 : Injection SQL dans les requêtes locales
- **Catégorie OWASP** : MASVS-CODE
- **Référence spécifique** : MASVS-CODE-4
- **Justification** : La construction de requêtes SQL par concaténation de chaînes sans validation ni paramétrage constitue une vulnérabilité de qualité du code critique, exploitable pour manipuler la base de données locale de l'application.

---

## FIND-005 : Activities exportées sans protection
- **Catégorie OWASP** : MASVS-PLATFORM
- **Référence spécifique** : MASVS-PLATFORM-2
- **Justification** : Les composants Android exportés sans restriction de permission permettent à des applications tierces d'invoquer des fonctionnalités internes, ce qui viole l'exigence de contrôle d'accès aux composants de la plateforme.

---

## FIND-006 : API Key / secret exposé dans les assets
- **Catégorie OWASP** : MASVS-STORAGE
- **Référence spécifique** : MASVS-STORAGE-2
- **Justification** : L'intégration de secrets (clés API, tokens) en dur dans les assets ou le bytecode de l'APK les rend extractibles par simple décompilation, violant l'exigence d'absence de données sensibles dans le code source ou les ressources packagées.

---

## FIND-007 : Journalisation excessive avec données sensibles
- **Catégorie OWASP** : MASVS-CODE
- **Référence spécifique** : MASVS-CODE-2
- **Justification** : La journalisation de données sensibles via `android.util.Log` en production contrevient aux exigences de qualité du code qui interdisent toute exposition de données critiques dans les mécanismes de débogage accessibles en dehors de l'environnement de développement.

---

## FIND-008 : Bibliothèques tierces obsolètes
- **Catégorie OWASP** : MASVS-CODE
- **Référence spécifique** : MASVS-CODE-3
- **Justification** : L'utilisation de dépendances non maintenues et présentant des CVE connues constitue une dette technique et sécuritaire directement adressée par l'exigence de gestion rigoureuse des composants tiers dans le standard MASVS.

---

## FIND-009 : allowBackup activé dans le manifeste
- **Catégorie OWASP** : MASVS-STORAGE
- **Référence spécifique** : MASVS-STORAGE-4
- **Justification** : Le flag `android:allowBackup=true` autorise l'extraction complète des données applicatives via ADB sans nécessiter de root, exposant l'ensemble du stockage privé de l'application à toute personne ayant un accès USB à l'appareil.

---

## FIND-010 : Absence de certificate pinning
- **Catégorie OWASP** : MASVS-NETWORK
- **Référence spécifique** : MASVS-NETWORK-2
- **Justification** : Sans épinglage de certificat, un proxy TLS (Burp Suite, mitmproxy) peut intercepter les communications HTTPS de l'application, rendant inefficace la protection TLS contre un attaquant en position de MITM actif.

---

## FIND-011 : Hardcoded credentials dans le code source
- **Catégorie OWASP** : MASVS-STORAGE
- **Référence spécifique** : MASVS-STORAGE-2
- **Justification** : Des identifiants codés en dur dans le bytecode sont extractibles via des outils de décompilation standard (jadx, apktool). Toute donnée d'authentification doit être gérée dynamiquement et jamais statiquement intégrée dans le binaire.

---

## FIND-012 : Absence de protection anti-décompilation
- **Catégorie OWASP** : MASVS-RESILIENCE
- **Référence spécifique** : MASVS-RESILIENCE-3
- **Justification** : L'absence d'obfuscation (ProGuard/R8 non activé) facilite le reverse engineering complet de l'application, permettant à un attaquant de comprendre la logique métier, d'identifier des vulnérabilités supplémentaires et d'extraire des secrets embarqués.

---

## Tableau récapitulatif du mapping

| ID        | Élément                              | Catégorie MASVS    | Référence        |
|-----------|--------------------------------------|--------------------|------------------|
| FIND-001  | Stockage données sensibles en clair  | MASVS-STORAGE      | MASVS-STORAGE-1  |
| FIND-002  | Communication HTTP en clair          | MASVS-NETWORK      | MASVS-NETWORK-1  |
| FIND-003  | Permissions excessives               | MASVS-PLATFORM     | MASVS-PLATFORM-1 |
| FIND-004  | Injection SQL                        | MASVS-CODE         | MASVS-CODE-4     |
| FIND-005  | Activities exportées sans permission | MASVS-PLATFORM     | MASVS-PLATFORM-2 |
| FIND-006  | API Key exposée dans les assets      | MASVS-STORAGE      | MASVS-STORAGE-2  |
| FIND-007  | Journalisation excessive             | MASVS-CODE         | MASVS-CODE-2     |
| FIND-008  | Bibliothèques tierces obsolètes      | MASVS-CODE         | MASVS-CODE-3     |
| FIND-009  | allowBackup activé                   | MASVS-STORAGE      | MASVS-STORAGE-4  |
| FIND-010  | Absence de certificate pinning       | MASVS-NETWORK      | MASVS-NETWORK-2  |
| FIND-011  | Hardcoded credentials                | MASVS-STORAGE      | MASVS-STORAGE-2  |
| FIND-012  | Absence d'obfuscation                | MASVS-RESILIENCE   | MASVS-RESILIENCE-3|

---

*Référence* : https://github.com/OWASP/owasp-masvs  
*Guide de test* : https://mas.owasp.org/MASTG/
