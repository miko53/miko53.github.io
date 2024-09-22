---
layout: page
title: Compilation calligraplan sous Windows
---

# Environnement

Je pars d'une version de Windows 11.

On va utiliser [`craft`](https://community.kde.org/Craft) qui est un outil qui permet de compiler les
logiciels KDE sous Windows.

# Installer craft

J'ai suivi la procédure indiquée sur le site [Beginning](https://community.kde.org/Get_Involved/development/Windows)

## 1.Python

  `craft` est basé sur Python, il faut donc installer [Python](https://www.python.org/downloads/).

  Je suis parti de la dernière version stable: 3.12.6 (installée en c:\Python)

## 2. Configurer PowerShell

  PowerShell, je suis sous Windows 11, donc pas la peine de mettre à jour.

## 3. Configurer le compilateur

  Calligraplan fonctionne sous Qt5, il faut donc visual studio 2019.

> **Note**
> Pour Qt6, il faudra installer Visual Studio 2022.

  Sur le site de [visual studio](https://learn.microsoft.com/en-us/visualstudio/releases/2019/history#installing-an-earlier-release), télécharger la version BuildTools (compilateur).

  Je suis partie de la la version 16.11.40.

  Durant l'installantion, ajout les composants necessaire en cliquant sur:
  - `Développement Desktop en c++`
  - `C++ ATL`
  - `Windows 11 SDK`

  Activer ensuite le mode développeur en se rendant sous Windows sous:
  - Paramètres/Système/Espace développeurs


## 4. Installer craft

Ouvrir un power shell (pas ISE)

Activer la possiblité d'éxecuter des scripts pas forcément signés:

`Set-ExecutionPolicy -Scope CurrentUser RemoteSigned`


Mettre à jour les certificats Python:
```
python -m pip install --upgrade certifi
```

executer
```
iex ((new-object net.webclient).DownloadString('https://raw.githubusercontent.com/KDE/craft/master/setup/install_craft.ps1'))
```

Il s'installe par défaut dans `c:\CraftRoot`. Indiquer Qt5 par défaut et compilateur vs2019.


# Compiler et lancer calligraplan

Une fois `craft` installée, il faut ajouter les blueprint.

Ajouter:
```
craft --add-blueprint-repository https://github.com/owncloud/craft-blueprints-owncloud.git
```

Une fois ajouté, on peut chercher le package:
```
craft --search calligraplan
```

On le trouve, on prends la version master:
```
craft --set version=master calligraplan
craft -i calligraplan
```

## Compilation

```
craft --compile --install --qmerge calligraplan
```

exemple de compilation pour [KdeConnect](https://community.kde.org/KDEConnect/Build_Craft)

Normalement si tout va bien, il ne reste plus qu'à la compiler en executant `calligraplan` depuis la fenêtre `PowerShell`

## Créer un package

Ovrir le fichier de configuration de craft. (`C:\CraftRoot\etc\CraftSettings.ini`)

Il s'agit de la variable `PackageType`, il y a plusieurs options possibles (par exemple `NullsoftInstallerPackager`)

Lancer la création du package. Pour cette exemple il faut install nsis.

```
craft nsis
```

et la création du package proprement dit:

```
craft --package calligraplan
```
Le resultat (comme indiqué dans la console) est sous `C:\CraftRoot\tmp\`

