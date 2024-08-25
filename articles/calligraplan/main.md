 
# Compilation calligraplan


Cet article décrit les différentes étapes utilisées pour compiler calligraplan  (https://calligra.org/plan/), à la fois sous Linux et sous Windows.

calligraplan est un logiciel utilisant le framework kde, ce qui rends sa portabilité sous Windows plus facile. Par contre les systèmes de compilation avec les nombreuses dépendances me semble complexe. D'où la description ici.

## Pourquoi ?

Je ne trouve pas à mon sens d'outil open-source pour la gestion de planning. J'ai essayé la version courante qui me semble pas mal. En fait c'est une des rares en open-source qui permettent de repartir la charge entre ressources. La plupart des outils que j'ai vu ne le font pas, ou alors dans une variante payante.

## Compilation sous Linux - Utilisation de kdesrc-build

C'est l'outil principal qui permet de compiler les applications kde sous Linux.

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

Initialiser le fichier de configuration globale, il est sous `~/.config/kdesrc-buildrc`
(cf. kdesrc-buildrc)


## Compilation sous Windows

