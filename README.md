
# Lab 6 — Analyse statique d’un APK avec MobSF

## Objectif général

Ce lab vise à analyser un fichier APK en utilisant **MobSF** dans la VM **Mobexler**, en suivant une démarche structurée et traçable.  
Chaque étape est documentée pour permettre de produire un **mini-rapport d’audit professionnel**.

---

## Task 1 — Préparation de l'environnement

### Objectif

Préparer un environnement de travail organisé et sécurisé pour l'analyse.

### Pourquoi ?

Une organisation méthodique garantit :

- la traçabilité de l'analyse
- la reproductibilité des résultats
- la facilité de rédaction du rapport final

### Étapes

1. Démarrer la VM **Mobexler** et se connecter avec les identifiants fournis.
2. Créer un dossier de travail pour l’analyse :

```bash
mkdir -p ~/apk_analysis/$(date +%Y-%m-%d)
cd ~/apk_analysis/$(date +%Y-%m-%d)

```
# Analyse d'APK avec Mobexler

Ce document décrit les étapes pour préparer et vérifier un fichier APK avant son analyse.

---

## 1. Copier ou déplacer l’APK à analyser

Placez l’APK que vous souhaitez analyser dans le dossier courant :

```bash
mv ~/Downloads/app-debug.apk ./
```

## 2. Vérifier l’intégrité de l’APK

Calculez le hash SHA-256 pour vérifier que l’APK n’a pas été corrompu ou modifié :

```bash
sha256sum app-debug.apk > apk_hash.txt
ls -lh app-debug.apk
cat apk_hash.txt
```
## 3. Documenter l’environnement d’analyse

Pour garder une trace des informations importantes lors de l’analyse :

```bash
echo "Date d'analyse : $(date)" > analyse_info.txt
echo "Analyste : Hajar Chaira" >> analyse_info.txt
echo "VM Mobexler version : $(cat /etc/mobexler-version 2>/dev/null || echo 'Version non disponible')" >> analyse_info.txt
echo "APK analysé : app-debug.apk" >> analyse_info.txt
cat apk_hash.txt >> analyse_info.txt
```

# Task 2 — Lancement de MobSF

## Objectif
Démarrer **MobSF**, l’outil d’analyse statique et dynamique pour applications mobiles.

## Pourquoi ?
MobSF automatise de nombreuses vérifications de sécurité pour les applications mobiles.

## Étapes

1. Ouvrir le terminal et lancer MobSF :

```bash
/Mobsfdocker.sh
```
## Noter la version de MobSF

Pour garder une trace de la version de MobSF dans le fichier de traçabilité :

```bash
echo "MobSF version : [version affichée]" >> ~/apk_analysis/$(date +%Y-%m-%d)_analyse_info.txt
```
# Task 3 — Import et analyse de l’APK

## Objectif
Soumettre l’APK à **MobSF** pour une analyse statique complète.

## Pourquoi ?
L’analyse statique examine le code et les ressources sans exécuter l’application, révélant des vulnérabilités potentielles.

## Étapes

1. Dans MobSF, cliquer sur **Upload & Analyze**, sélectionner l’APK, puis cliquer sur **Analyze**.

2. Noter l’heure de début de l’analyse :

```bash
echo "Début d'analyse : $(date)" >> ~/apk_analysis/$(date +%Y-%m-%d)/analyse_info.txt
```


### Fin de l’analyse

Attendre la fin de l’analyse.

Noter l’heure de fin et le score de sécurité :

```bash
echo "Fin d'analyse : $(date)" >> ~/apk_analysis/$(date +%Y-%m-%d)/analyse_info.txt
echo "Score de sécurité : [score]" >> ~/apk_analysis/$(date +%Y-%m-%d)/analyse_info.txt
```

# Task 4 — Analyse du manifeste et des permissions

## Objectif
Examiner le manifeste Android et les permissions demandées par l’application.

## Pourquoi ?
Le manifeste contient des informations critiques sur la configuration et la sécurité de l’application.

## Étapes

1. Dans MobSF, accéder à la section **App Information**.

2. Vérifier les éléments suivants :

- Nom du package  
- Version  
- SDK minimum (minSdkVersion)  
- SDK cible (targetSdkVersion)  
- Taille de l’APK  
- `android:debuggable`  
- `android:allowBackup`  

3. Analyser les permissions demandées par l’appar l’application dans la section **Permissions**.

## Analyse des permissions et composants

### Permissions dangereuses

Créer un fichier pour documenter les permissions sensibles :

## Composants exportés et observations

### Identifier les composants exportés

Créer un fichier pour documenter les composants exposés :

# Task 5 — Analyse de la configuration réseau

## Objectif
Examiner la sécurité réseau de l’application.

## Pourquoi ?
La configuration réseau peut exposer des données sensibles si elle est mal configurée.

## Étapes

1. Rechercher le fichier `network_security_config.xml` dans MobSF.

2. Vérifier les paramètres suivants :
- `android:usesCleartextTraffic`
- `android:networkSecurityConfig`

3. Documenter les résultats :

```bash
echo "Configuration réseau :" > config_reseau.txt
echo "-------------------" >> config_reseau.txt
echo "usesCleartextTraffic : absent" >> config_reseau.txt
echo "networkSecurityConfig : absent" >> config_reseau.txt
```


# Task 6 — Analyse du code et des ressources

## Objectif
Identifier les vulnérabilités dans le code et les ressources de l’application.


1. section **Code Analysis** dans MobSF et noter les alertes par catégorie.


3. Vérifier les fichiers sensibles dans la section **Files**.

4. Documenter les vulnérabilités détectées :



### Ressources sensibles

Créer un fichier pour documenter les ressources sensibles détectées :

# Task 7 — Corrélation avec OWASP MASVS

## Objectif
Associer les vulnérabilités identifiées aux exigences du standard MASVS.

## Étapes

1. Consulter la documentation **MASVS**.

2. Identifier les vulnérabilités critiques :

- `android:debuggable="true"` → MASVS-RESILIENCE-1  
- `android:allowBackup="true"` → MASVS-STORAGE-1  

3. Documenter la corrélation et Consulter MASTG pour des tests complémentaires :



# Task 8 — Exportation et analyse du rapport complet

## Objectif
Exporter le rapport MobSF et extraire les informations essentielles.

## Étapes

1. Générer le rapport au format **PDF** ou **JSON** depuis MobSF.

2. Déplacer le fichier dans le dossier d’analyse :

```bash
mv ~/Downloads/MobSF_Report*.pdf ~/apk_analysis/$(date +%Y-%m-%d)/MobSF_Report_$(date +%Y%m%d).pdf
echo "Rapport exporté : MobSF_Report_$(date +%Y%m%d).pdf" >> ~/apk_analysis/$(date +%Y-%m-%d)/analyse_info.txt
```


# Task 9 — Rédaction du mini-rapport d’audit

## Objectif
Synthétiser les découvertes dans un rapport clair et professionnel.









