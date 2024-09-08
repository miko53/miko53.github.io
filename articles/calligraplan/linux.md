---
layout: page
title: Compilation calligraplan
---


# Compilation sous Linux

On utilise kdesrc-build qui est l'outil principal qui permet de compiler les applications kde sous Linux.

Suivre l'installation comme indiqué sur leur site ([kdesrc](https://github.com/KDE/kdesrc-build)):

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
kdesrc-build --metadata-only
kdesrc-build --pretend
```

Utiliser un répertoire pour stocker les éléments de compilations des dépendances

```
mkdir -p ~/projects/kde
cd ~/projects/kde
```

Initialiser le fichier de configuration globale, il est sous `~/.config/kdesrc-buildrc` (cf. [kdesrc-buildrc](kdesrc-buildrc.md))

### Compilation des différents élements

Maintenant que le projet est prêt, on peut passer à la compilation de tous ces élements.


```
kdesrc-build karchive
kdesrc-build kconfigwidgets
kdesrc-build kconfig
kdesrc-build kcoreaddons kdBusaddons kguiaddons
kdesrc-build ki18n kiconthemes kitemviews kitemmodels kjobwidgets

apt install libgpg-error-dev

kdesrc-build kio
kdesrc-build knotifications kparts
kdesrc-build sonnet
kdesrc-build ktextwidgets kwidgetsaddons kwindowsystem kxmlgui
kdesrc-build kf5activities
kdesrc-build activities
kdesrc-build kactivities
kdesrc-build KF5Holidays
kdesrc-build Kf5hHolidays
kdesrc-build kf5hHolidays
kdesrc-build khHolidays
kdesrc-build kholidays
kdesrc-build kactivities
kdesrc-build activities
kdesrc-build plasma-activities
kdesrc-build kgantt
kdesrc-build kdiagram

tar xzf libgpg-error-1.47.tar.gz
cd libgpg-error-1.47/
./configure
make
sudo make install

export KDEHOME=/home/mickael/projects/kde_home/
cd calligraplan/build
. prefix.sh


```

