---
layout: page
title: Compilation calligraplan sous Linux
---


# Préparation de l'environnement

L'installation est réalisée à partir d'une Debian 12 de base avec KDE & le packet `build-essential` et `kdevelop`.
De toute façon, les scripts sont récupérés tout ce qu'il faut.

On utilise `kdesrc-build` qui est l'outil principal qui permet de compiler les applications kde sous Linux.

Suivre l'installation comme indiqué sur le site ([kdesrc](https://github.com/KDE/kdesrc-build)):

```
mkdir -p ~/.local/share
cd ~/.local/share/
git clone https://invent.kde.org/sdk/kdesrc-build.git
mkdir ~/.local/bin
ln -sd ~/.local/share/kdesrc-build/kdesrc-build ~/.local/bin/
```

 `~/.local/bin` doit être inclus dans le `PATH`, vérifier avec :

```
echo $PATH
```

Modifier le fichier `~/.bashrc` si besoin (et si vous êtes sous bash) et insérer:

```
export PATH=~/.local/bin:$PATH
```

On vérifie si ça fonctionne et normalement sous Debian 12, ce n'est pas le cas.

Il faut installer le paquet `libjson-xs-perl`:

```
cd ~
kdesrc-build --version
sudo apt-get install libjson-xs-perl

```

### Initialisation

```
kdesrc-build --initial-setup
```

Initialiser le fichier de configuration globale, il est sous `~/.config/kdesrc-buildrc` (cf. [kdesrc-buildrc](kdesrc-buildrc.md))

Mettre pour la branche `kf5-qt5` à ce jour (09/2024), il semble que calligraplan ne soit pas compatible qt6.

```
kdesrc-build --metadata-only
kdesrc-build --pretend
```

> **Note**
> `--pretend` renvoie une erreur sur les libs Qt5, vous pouvez ignorer, lorsque il va compiler, il va les trouver quand même.

Utiliser un répertoire pour stocker les éléments de compilations des dépendances

```
mkdir -p ~/projects/kde
mkdir -p ~/projects/kde/kde_home
cd ~/projects/kde
```

### Recuperation de calligraplan

```
git clone https://invent.kde.org/office/calligraplan.git
```


### Compilation des différents élements

Si on compile directement calligraplan, il informe qu'il manque des packets kde, il faut donc les récupérer.

Voici les commandes de compilation, je me suis basé sur le fichier CMakeLists.txt de calligraplan.

En cas d'erreur, revérifier le ficher kdesrc-buildrc et les logs de compilation.
Etre patient, ça peut etre long :-)

On peut regrouper toutes les commandes en une aussi.


```
kdesrc-build karchive
kdesrc-build kconfigwidgets
kdesrc-build kconfig
kdesrc-build kcoreaddons kdbusaddons kguiaddons
kdesrc-build ki18n kiconthemes kitemviews kitemmodels kjobwidgets
```

Pour compiler  kio il faut au moins la libgpg-error en version 1.47.
Il faut la compiler manuellement car la version pour la Distrib Debian 12 est trop ancienne.

Dans un autre dossier:

```
git clone git://git.gnupg.org/libgpg-error.git
git checkout libgpg-error-1.47
./autogen.sh
./configure --enable-maintainer-mode
make
sudo make install
```

kio peut maintenant etre compilé

```
kdesrc-build kio
```

Suite des packages...

```
kdesrc-build knotifications kparts
kdesrc-build sonnet
kdesrc-build ktextwidgets kwidgetsaddons kwindowsystem kxmlgui
kdesrc-build plasma-activities
kdesrc-build kholidays
kdesrc-build kdiagram
kdesrc-build threadweaver
```

Normalement désormais, toutes les dependances sont compilées, on peut s'occuper de calligraplan

### Génération de l'éxécutable

Mettre à jour les variables d'environnement, pour cela s'inspirer d'un des modules compilé

Créer le fichier env.sh, l'adapter pour ses chemins (ici \[user\] à remplacer par votre login)
```
export PATH=/home/[user]/projects/kde/usr/bin:$PATH

# LD_LIBRARY_PATH only needed if you are building without rpath
# export LD_LIBRARY_PATH=/home/[user]/projects/kde/usr/lib/x86_64-linux-gnu:$LD_LIBRARY_PATH

export XDG_DATA_DIRS=/home/[user]/projects/kde/usr/share:${XDG_DATA_DIRS:-/usr/local/share/:/usr/share/}
export XDG_CONFIG_DIRS=/home/[user]/projects/kde/usr/etc/xdg:${XDG_CONFIG_DIRS:-/etc/xdg}

export QT_PLUGIN_PATH=/home/[user]/projects/kde/usr/lib/x86_64-linux-gnu/plugins:$QT_PLUGIN_PATH
export QML2_IMPORT_PATH=/home/[user]/projects/kde/usr/lib/x86_64-linux-gnu/qml:$QML2_IMPORT_PATH

export QT_QUICK_CONTROLS_STYLE_PATH=/home/[user]/projects/kde/usr/lib/x86_64-linux-gnu/qml/QtQuick/Controls.2/:$QT_QUICK_CONTROLS_STYLE_PATH

```

Il doit y exister un autre moyen car à l'étape cmake, un fichier identique va être crée...


```
. env.sh
cd calligraplan
mkdir build
cd build
cmake ../.
make
make install
```

L'installation est importante, elle permet d'installer les plugins de l'application. Sans cela, on a une erreur au démarrage.

Normalement vous avez un executable calligraplan de généré.
Dans le répertoire build:
```
. prefix.sh
export KDEHOME=/home/[user]/projects/kde/kde_home/
cd calligraplan/build
./bin/calligraplan
```

