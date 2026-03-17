
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
<p align="center"> <img src="images/m1.png" width="800"> </p>
                                                            
3. Créer un dossier de travail pour l’analyse :
<p align="center"> <img src="images/m6.png" width="800"> </p>   

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


# Task 2 — Lancement de MobSF
<p align="center"> <img src="images/m2.png" width="800"> </p>
<p align="center"> <img src="images/m3.png" width="800"> </p>
<p align="center"> <img src="images/m4.png" width="800"> </p
<p align="center"> <img src="images/m5.png" width="800"> </p>
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
<p align="center"> <img src="images/m7.png" width="800"> </p>   
<p align="center"> <img src="images/m8.png" width="800"> </p>   

# Task 3 — Import et analyse de l’APK

## Objectif
Soumettre l’APK à **MobSF** pour une analyse statique complète.

## Pourquoi ?
L’analyse statique examine le code et les ressources sans exécuter l’application, révélant des vulnérabilités potentielles.

## Étapes

1. Dans MobSF, cliquer sur **Upload & Analyze**, sélectionner l’APK, puis cliquer sur **Analyze**.
<p align="center"> <img src="images/m5.png" width="800"> </p>   
2. Noter l’heure de début de l’analyse :
<p align="center"> <img src="images/m9.png" width="800"> </p>   


### Fin de l’analyse

Attendre la fin de l’analyse.

Noter l’heure de fin et le score de sécurité :

<p align="center"> <img src="images/m10.png" width="800"> </p>   
<p align="center"> <img src="images/m11.png" width="800"> </p>   

# Task 4 — Analyse du manifeste et des permissions
 
## Objectif
Examiner le manifeste Android et les permissions demandées par l’application.

## Pourquoi ?
Le manifeste contient des informations critiques sur la configuration et la sécurité de l’application.

## Étapes

1. Dans MobSF, accéder à la section **App Information**.
<p align="center"> <img src="images/m40.png" width="800"> </p>   
<p align="center"> <img src="images/m41.png" width="800"> </p>  
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
<p align="center"> <img src="images/m13.png" width="800"> </p>   
### Permissions dangereuses

Créer un fichier pour documenter les permissions sensibles :
<p align="center"> <img src="images/m14.png" width="800"> </p>   
## Composants exportés et observations
<p align="center"> <img src="images/m17.png" width="800"> </p>   
<p align="center"> <img src="images/m15.png" width="800"> </p>   

# Task 5 — Analyse de la configuration réseau

## Objectif
Examiner la sécurité réseau de l’application.

## Pourquoi ?
La configuration réseau peut exposer des données sensibles si elle est mal configurée.

<p align="center"> <img src="images/m42.png" width="800"> </p>   


# Task 6 — Analyse du code et des ressources

## Objectif
Identifier les vulnérabilités dans le code et les ressources de l’application.


1. section **Code Analysis** dans MobSF et noter les alertes par catégorie.
<p align="center"> <img src="images/m22.png" width="800"> </p>   
<p align="center"> <img src="images/m23.png" width="800"> </p>   
3. Vérifier les fichiers sensibles dans la section **Files**.
<p align="center"> <img src="images/m25.png" width="800"> </p>   
4. Documenter les vulnérabilités détectées :
<p align="center"> <img src="images/m24.png" width="800"> </p>   


### Ressources sensibles

Créer un fichier pour documenter les ressources sensibles détectées :
<p align="center"> <img src="images/m26.png" width="800"> </p>   


# Task 7 — Corrélation avec OWASP MASVS

## Objectif
Associer les vulnérabilités identifiées aux exigences du standard MASVS.

## Étapes

1. Consulter la documentation **MASVS**et **MASTG** pour les tests.
<p align="center"> <img src="images/m27.png" width="800"> </p>   
<p align="center"> <img src="images/m28.png" width="800"> </p>   
2. Identifier les vulnérabilités critiques :

- `android:debuggable="true"` → MASVS-RESILIENCE-1  
- `android:allowBackup="true"` → MASVS-STORAGE-1  

3. Documenter la corrélation et Consulter MASTG pour des tests complémentaires :

<p align="center"> <img src="images/m29.png" width="800"> </p>   

# Task 8 — Exportation et analyse du rapport complet

## Objectif
Exporter le rapport MobSF et extraire les informations essentielles.

<p align="center"> <img src="images/m30.png" width="800"> </p>   
<p align="center"> <img src="images/m31.png" width="800"> </p>   
<p align="center"> <img src="images/m32.png" width="800"> </p>   
<p align="center"> <img src="images/m33.png" width="800"> </p>   

## Analyse :
<p align="center"> <img src="images/m34.png" width="800"> </p>   
<p align="center"> <img src="images/m35.png" width="800"> </p>  

### Documenter les top vulnerabilites :
<p align="center"> <img src="images/m36.png" width="800"> </p>  
# Task 9 — Rédaction du mini-rapport d’audit

## Objectif
Synthétiser les découvertes dans un rapport clair et professionnel.


 <p align="center"> <img src="images/m37.png" width="800"> </p>  
  <p align="center"> <img src="images/m38.png" width="800"> </p>  

# Structure final d'analyse :
 <p align="center"> <img src="images/m43.png" width="800"> </p>  





