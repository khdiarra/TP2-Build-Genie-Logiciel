---
title:     TP2 de Génie Logiciel
subtitle:  les outils de « build »
authors:    Diarra Khadidiatou et Diallo Djenabou Cellou

---


# 1.2 Makefile

Lorsque l'on tape **make** pour la première fois, l'outil applique les règles présentes dans le fichier Makefile à partir de la règle 'all', et compile tous les fichiers (main.c, function.c) pour produire l'exécutable 'example'.

```bash
$ make
gcc -c main.c -o main.o
gcc -c function.c -o function.o
gcc main.o function.o -o example
```

Par contre,  lorsque l'on tape une deuxième fois la commande **make** sans avoir modifié un des fichier source, l'outil ne fait que l'opération de linkage pour produire l'exécutable 'example' (car les dépendances main.o et function.o de la règle 'all' n'ont pas été modifiées).

```bash
$ make
gcc main.o function.o -o example
```

Si l'on modifie le fichier function.c et que l'on retape la commande **make**, l'outil va seulement recompiler le fichier 'function.c' et effectuer l'opération de linkage pour produire l'exécutable 'example'. Le fichier main.c ne sera lui pas recompilé car il n'a pas été modifié.

```bash
$ make
gcc -c function.c -o function.o
gcc main.o function.o -o example
```

La commande **make clean** permet d'exécuter la commande shell **rm -rf \*.o example**, qui supprime tous les fichiers objets ainsi que l'exécutable 'example'.

```bash
$ make clean
rm -rf *.o example
```

## Makefile évolué

Lorsque l'on tape la commande **make**, l'outil va chercher le fichier de règles 'Makefile' ou 'makefile'. Avec l'option -f de make, on peut utiliser un fichier de règles avec un autre nom (ici, Makefile2).

En tapant la commande **make -f Makefile2**, on créer les deux fichiers objets function.o et main.o et les lient pour obtenir l'exécutable 'example'. Des messages en plus sont affichés, grâce à la commande shell **echo** présente dans les règles.

```bash
$ make -f Makefile2
compilation de main.c :
gcc -o main.o -c main.c
compilation de function.c :
gcc -o function.o -c function.c
linkage :
gcc -o example main.o function.o
```

En retapant la commande **make -f Makefile2** sans avoir modifié de fichiers, on obtient le message 'make: Rien à faire pour « all ».'. Celà est dût au fait que la règle 'all' a comme dépendance le fichier exécutable example, et le fichier example a comme dépendances function.o et main.o : puisque les fichiers objets n'ont pas été modifiés, le **make** n'a rien à recompiler.

```bash
$ make -f Makefile2
make: Rien à faire pour « all ».
```

Comme avec le premier Makefile, la commande **make -f Makefile2 clean** permet de supprimer tous les fichiers objets ainsi que l'exécutable.

```bash
$ make -f Makefile2 clean
rm -rf *.o example
```

# 1.3 Scons

Lorsque l'on tape **scons** pour la première fois, l'outil compile tous les fichiers (function.c et main.c) en fichiers objets, et les lient pour produire l'exécutable example.

```bash
$ scons
scons: Reading SConscript files ...
scons: done reading SConscript files.
scons: Building targets ...
gcc -o main.o -c main.c
gcc -o function.o -c function.c
gcc -o example main.o function.o function.h
scons: done building targets.
```

Si on retape **scons** en ayant modifié function.h, l'ensemble du projet est recompilé. Celà signifie que le fichier function.h est une dépendance pour chaque fichier source .c.

```bash
$ scons
scons: Reading SConscript files ...
scons: done reading SConscript files.
scons: Building targets ...
gcc -o main.o -c main.c
gcc -o function.o -c function.c
gcc -o example main.o function.o function.h
scons: done building targets.
```

**scons -c** permet de supprimer les fichiers objets *.o et le fichier exécutable 'example'.

```bash
$ scons -c
scons: Reading SConscript files ...
scons: done reading SConscript files.
scons: Cleaning targets ...
Removed main.o
Removed function.o
Removed example
scons: done cleaning targets.
```

# 1.4 Rake

Lorsque l'on tape **rake** pour la première fois, l'ensemble des fichiers sources sont compilés (function.c et main.c) en fichiers objets, puis liés pour produire l'exécutable example. Lorsque l'on retape **rake** sans modifier de fichiers, rien n'est recompilé.

```bash
$ rake
cc -c -o function.o function.c
cc -c -o main.o main.c
cc -o example function.o main.o
```

Si l'on modifie function.h et que l'on retape **rake** le fichier main.c est recompilé, car il dépends de main.c et function.h. Et puisqu'un fichier objet a été modifié, le fichier exécutable example est relinké à partir des différents fichiers objets.

```bash
$ rake
cc -c -o main.o main.c
cc -o example function.o main.o
```

**rake clean** permet de supprimer tous les fichiers objets, l'exécutable ainsi que les fichiers temporaires d'emacs (*~).

```bash
$ rake clean
rm -r function.o
rm -r main.o
rm -r main.c~
rm -r function.c~
rm -r example
```

# 1.5 Ant

Lorsque l'on lance pour la première fois la commande **ant**, il créer un dossier build/ où sont stockés les fichiers .class. Ensuite, les deux fichiers sources (Function.java et Main.java) sont compilés à l'aide de **javac**, pour produire les fichiers build/Function.class et build/Main.class.

```bash
$ ant
Buildfile: /home/SCIENCES/khdiarra/M_diarra/GenieLogiciel/projet-\
           java/build.xml

init:
    [mkdir] Created dir: /home/SCIENCES/diarra/M_diarra/GenieLo\
                         giciel/projet-java/build

compile:
    [javac] /home/SCIENCES/diarra/M_diarra/GenieLogiciel/projet\
            -java/build.xml:11: warning: 'includeantruntime' was \
            not set, defaulting to build.sysclasspath=last; set t\
            o false for repeatable builds
    [javac] Compiling 2 source files to /home/SCIENCES/diarra/M_\
            diarra/GenieLogiciel/projet-java/build

BUILD SUCCESSFUL
Total time: 1 second
```

Si l'on relance la commande **ant**, aucun des fichiers *.java sont recompilés et le dossier build/ n'est pas recréé.

```bash
$ ant
Buildfile: /home/SCIENCES/diarra/M_diarra/GenieLogiciel/projet-\
           java/build.xml

init:

compile:
    [javac] /home/SCIENCES/diarra/M_diarra/GenieLogiciel/projet\
            -java/build.xml:11: warning: 'includeantruntime' was \
            not set, defaulting to build.sysclasspath=last; set t\
            o false for repeatable builds

BUILD SUCCESSFUL
Total time: 0 seconds
```

La commande **ant dist** créer l'arborescence dist/lib/ si les dossiers n'existent pas, et créer le fichier dist/lib/MyProject-20161010.jar à partir des fichiers build/Function.class et build/Main.class. (on peut noter la date de la création dans le nom du fichier .jar).

```bash
Buildfile: /home/SCIENCES/diarra/M_diarra/GenieLogiciel/projet-\
           java/build.xml

init:

compile:
    [javac] /home/SCIENCES/diarra/M_diarra/GenieLogiciel/projet\
            java/build.xml:11: warning: 'includeantruntime' was n\
            ot set, defaulting to build.sysclasspath=last; set to\
            false for repeatable builds

dist:
    [mkdir] Created dir: /home/SCIENCES/diarra/M_diarra/GenieLo\
            giciel/projet-java/dist/lib
      [jar] Building jar: /home/SCIENCES/diarra/M_diarra/GenieL\
            ogiciel/projet-java/dist/lib/MyProject−20161014.jar

BUILD SUCCESSFUL
Total time: 0 seconds
```

La commande **ant clean** supprime les dossiers build/ et dist/.

```bash
$ ant clean
Buildfile: /home/SCIENCES/diarra/M_diarra/GenieLogiciel/projet-\
           java/build.xml

clean:
   [delete] Deleting directory /home/SCIENCES/diarra/M_diarra/G\
            enieLogiciel/projet-java/build
   [delete] Deleting directory /home/SCIENCES/diarra/M_diarra/G\
            enieLogiciel/projet-java/dist

BUILD SUCCESSFUL
Total time: 0 seconds
```

Si l'on retape la commande **ant dist**, les deux arborescences sont recréés, les fichiers sources sont recompilés en .class et le fichier .jar regroupant l'ensemble des .class est recréé.

```bash
$ ant dist
--------
Buildfile: /home/SCIENCES/diarra/M_diarra/GenieLogiciel/projet-\
           java/build.xml

init:
    [mkdir] Created dir: /home/SCIENCES/diarra/M_diarra/GenieLo\
            giciel/projet-java/build

compile:
    [javac] /home/SCIENCES/diarra/M_diarra/GenieLogiciel/projet\
            -java/build.xml:11: warning: 'includeantruntime' was \
            not set, defaulting to build.sysclasspath=last; set t\
            o false for repeatable builds
    [javac] Compiling 2 source files to /home/SCIENCES/diarra/M_\
            diarra/GenieLogiciel/projet-java/build

dist:
    [mkdir] Created dir: /home/SCIENCES/diarra/M_diarra/GenieLo\
            giciel/projet-java/dist/lib
      [jar] Building jar: /home/SCIENCES/diarra/M_diarra/GenieL\
            ogiciel/projet-java/dist/lib/MyProject−20161014.jar

BUILD SUCCESSFUL
Total time: 1 second
```s
