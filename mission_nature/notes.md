# Pense bête pour la compilation de l'application Mission Nature


https://github.com/NaturalSolutions/Mission-Nature-mobile

https://github.com/NaturalSolutions/Mission-Nature-scripts

en complément notamment de https://github.com/NaturalSolutions/Mission-Nature-scripts/blob/master/mission_manual.rst



## 1. Générer les fichiers JSON d'après les CSV

- mettre les CSV `missions.csv` et `taxons.csv` dans `/home/mission/Mission-Nature-scripts` via WinSCP

(question : et pour les CSV `cities.csv` et `credits_photos.csv` ?)

réponses :
credits_photos.csv > avec site csv to json
et placer le json dans le dossier data

cities.csv > ne pas y toucher pour l'instant car il y a un index


- en ligne de commande (putty), se placer dans ce dossier :

`cd /home/mission/Mission-Nature-scripts`

- executer la commande :

`php ./mission_tojson.php`

NB. : messages erreurs : `PHP Notice:  Undefined offset: 1 in /home/mission/Mission-Nature-scripts/mission_tojson.php on line 15`
(ignorer : lié à une ligne vide ??)


## 2. Redimensionner les photos pour les dossier thumb et full

- mettre les photos dans les dossiers `photos` du répertoire `Mission-Nature-scripts/mission_taxon/` (via WinScp)

- lancer les commandes (putty) mogrify depuis les dossiers `photos` (pour taxons et missions) :

`cd /home/mission/Mission-Nature-scripts/mission_taxon/taxon/photos`
`mogrify -path ../thumbs -thumbnail 256x256^ -gravity center -extent 256x256 *.jpg`
`mogrify -path ../poster/ -resize 900x900 *.jpg`

* /!\ nb : incohérence avec les répertoires de l'appli : /thumbs et /poster et non /photos_thumb et /photos_full*


## 3. Compilation de l'appli avec CORDOVA

- télécharger en local (sur `C:\mission_nature` ) l'application sur Github :
https://github.com/NaturalSolutions/Mission-Nature-mobile

- mettre les photos en local depuis le serveur Mission Nature (poster + thumbs) dans les dossiers `www/images/mission_taxons` et dans `www/data/image_source` (via WinSCP)

- (question : ne devrait-on pas mettre aussi le fichier `missions.json`)

- Adapter `www\modules\main\config.js.tpl` en `config.js` avec :
```
  coreUrl: 'http://149.202.129.99/mission_nature',
  apiUrl: 'http://149.202.129.99/mission_nature/obfmobileapi',
```

- avec CMD, se placer dans le dossier où a été téléchargée l'app :

`cd C:\mission_nature\Mission-Nature-mobile-master`

- executer ces commandes :

`npm install` (execution : 13s)

`cordova platform add android`

(normalement très long. Mais si plateform déjà ajoutée, rapide ('Android plateform already added'))


`cordova build android`

> erreur (mars)
```
Failed to find 'ANDROID_HOME' environment variable. Try setting it manually.
https://stackoverflow.com/questions/36198165/failed-to-find-android-home-environment-variable
https://cordova.apache.org/docs/en/latest/guide/platforms/android/index.html#requirements-and-support
```
> peut-être résolu ? (à vérifier)


> si tout va bien :
```
BUILD SUCCESSFUL in 1m 28s
44 actionable tasks: 44 executed
Built the following apk(s):
        C:/mission_nature/Mission-Nature-mobile-master/platforms/android/build/outputs/apk/debug/android-debug.apk
```
