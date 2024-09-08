---
layout: page
title: kdesrc-buildrc
---

Ce fichier est autogénéré

L'important la branche `kf5-qt5` et les emplacements `~/project/...`
J'ai préfixé les path avec ~/projects
Et le champ: module-definitions-dir

~~~

# This file controls options to apply when configuring/building modules, and
# controls which modules are built in the first place.
# List of all options: https://docs.kde.org/trunk5/en/kdesrc-build/kdesrc-build/conf-options-table.html

global
    branch-group kf5-qt5

    # Finds and includes *KDE*-based dependencies into the build.  This makes
    # it easier to ensure that you have all the modules needed, but the
    # dependencies are not very fine-grained so this can result in quite a few
    # modules being installed that you didn't need.
    include-dependencies true

    # Install directory for KDE software
    install-dir ~/projects/kde/usr

    # Directory for downloaded source code
    source-dir ~/projects/kde/src

    # Directory to build KDE into before installing
    # relative to source-dir by default
    build-dir ~/projects/kde/build

    # qt-install-dir ~/projects/kde/qt # Where to install Qt6 if kdesrc-build supplies it

    cmake-options -DCMAKE_BUILD_TYPE=RelWithDebInfo

    # kdesrc-build sets 2 options which is used in options like make-options or set-env
    # to help manage the number of compile jobs that happen during a build:
    #
    # 1. num-cores, which is just the number of detected CPU cores, and can be passed
    #    to tools like make (needed for parallel build) or ninja (completely optional).
    #
    # 2. num-cores-low-mem, which is set to largest value that appears safe for
    #    particularly heavyweight modules based on total memory, intended for
    #    modules like qtwebengine
    num-cores 4
    num-cores-low-mem 2

    # kdesrc-build can install a sample .xsession file for "Custom"
    # (or "XSession") logins,
    install-session-driver false

    # or add a environment variable-setting script to
    # ~/.config/kde-env-master.sh
    install-environment-driver true

    # Stop the build process on the first failure. If set to false, when kdesrc-build
    # encounters a build failure, it will attempt to continue building other modules,
    # using libraries from the system in cases where they would otherwise be provided
    # by a module that has failed to build.
    #
    # Unless your system has very up-to-date packages, this is probably not what you want.
    stop-on-failure true

    # Use a flat folder layout under ~/projects/kde/src and ~/projects/kde/build
    # rather than nested directories
    directory-layout flat

    # Use Ninja as cmake generator instead of gmake
    cmake-generator Kate - Ninja

    # Build with LSP support for everything that supports it
    compile-commands-linking false
    compile-commands-export true

    # Generate .vscode config files in project directories
    # Enable this if you want to use Visual Studio Code for development
    generate-vscode-project-config false
end global

# With base options set, the remainder of the file is used to define modules to build, in the
# desired order, and set any module-specific options.

#  This line includes module definitions provided in repo-metadata. Do not comment it.
include ${module-definitions-dir}/kf5-qt5.ksb

# To change options for modules that have already been defined, use an
# 'options' block. See kf6-common-options.ksb for an example

# kate: syntax kdesrc-buildrc;
~~~
