# CARTES ADDITIONNELLES ET HATs

## N.B. CE REPO EST DÉPRÉCIÉ. Les nouvelles utilitaires HAT+ EEPROM se trouvent dans notre [repo utilitaires](https://github.com/Modding-Gaming/Raspberrypi/utils/tree/master/eeptools). ##https://github.com/Modding-Gaming/Raspberrypi/edit/main/hats/README.md

**REMARQUE** Toutes les références aux numéros GPIO dans ce document concernent les GPIO BCM283x (**NON** les numéros de broche sur le connecteur GPIO du Pi).

## Introduction

Les cartes Raspberry Pi avec des connecteurs GPIO de 40 broches (Modèle B+ et suivants) ont été conçues spécifiquement avec les cartes additionnelles en tête. Ces cartes supportent les 'HATs' (Hardware Attached on Top). Un HAT est une carte additionnelle qui respecte les spécifications HAT. Les HATs ne sont pas rétrocompatibles avec les modèles originaux Raspberry Pi 1 A et B.

En octobre 2018, Raspberry Pi a introduit la spécification Micro-HAT (uHAT). Les uHATs doivent suivre toutes les règles électriques standard des HAT, mais ils ont un facteur de forme mécanique plus petit comme spécifié [ici](uhat-board-mechanical.pdf).

Il existe évidemment de nombreuses cartes additionnelles conçues pour les modèles originaux A et B (qui se connectent au connecteur GPIO à 26 broches d'origine). Les 26 premières broches du connecteur GPIO de 40 broches sont identiques à celles des modèles originaux, donc la plupart des cartes existantes fonctionneront toujours.

Le plus grand changement avec les cartes additionnelles HAT par rapport aux anciennes cartes conçues pour les modèles A et B est que le connecteur de 40 broches comporte 2 broches spéciales (ID_SC et ID_SD) réservées exclusivement à la connexion d'une 'ID EEPROM'. L'ID EEPROM contient des données qui identifient la carte, indiquent au Pi comment les GPIO doivent être configurés et quel matériel est présent sur la carte. Cela permet à la carte additionnelle d'être automatiquement identifiée et configurée par le logiciel du Pi au démarrage, y compris le chargement de tous les pilotes nécessaires.

Bien que nous ne puissions pas obliger quiconque à respecter nos exigences minimales ou les spécifications HAT, le faire facilitera la vie des utilisateurs, la rendra plus sûre et augmentera la probabilité que nous recommandions un produit. De même, si l'une des exigences minimales est ignorée, nous serons peu enclins à recommander un produit.

Alors pourquoi faisons-nous tout cela ? Fondamentalement, nous voulons garantir la cohérence et la compatibilité avec les futures cartes additionnelles, et offrir une bien meilleure expérience utilisateur, en particulier pour les utilisateurs moins techniques.

Enfin, si vous avez des questions, veuillez vous rendre sur les [forums](https://forums.raspberrypi.com/viewforum.php?f=45) pour les poser.

## Exigences de base pour les nouveaux HATs / cartes additionnelles

Si vous concevez une nouvelle carte additionnelle qui utilise les broches du connecteur GPIO de 40 broches **autres que les 26 broches d'origine**, vous **devez** respecter les exigences de base suivantes :

1. Les broches ID_SC et ID_SD doivent être utilisées uniquement pour connecter une ID EEPROM compatible. **Ne pas utiliser les broches ID_SC et ID_SD pour autre chose que la connexion d'une ID EEPROM, si elles ne sont pas utilisées, ces broches doivent rester non connectées**.
2. Si vous alimentez en arrière via les broches GPIO de 5V, vous devez vous assurer que cela est sûr même si l'alimentation 5V du Pi est également connectée. Ajouter une diode de sécurité idéale comme indiqué dans la section pertinente du [guide de conception](designguide.md) est la méthode recommandée.
3. La carte doit se protéger contre les anciens micrologiciels qui pourraient accidentellement conduire les GPIO 6, 14 ou 16 au démarrage si l'une de ces broches est également pilotée par la carte elle-même.

Notez que pour les nouveaux designs qui utilisent uniquement les broches du connecteur GPIO à 26 broches d'origine, il est toujours recommandé de suivre l'exigence 2 si la carte supporte l'alimentation arrière du Pi.

## Exigences HAT

Une carte ne peut être appelée HAT que si :

1. Elle respecte les exigences de base pour les cartes additionnelles.
2. Elle possède une ID EEPROM valide (y compris les informations sur le vendeur, la carte GPIO et les informations valides sur l'arbre des périphériques).
3. Elle a un connecteur GPIO de taille complète de 40 broches.
4. Elle respecte la [spécification mécanique HAT](hat-board-mechanical.pdf).
5. Elle utilise un connecteur GPIO qui espace le HAT d'au moins 8 mm du Pi (c'est-à-dire utilise des entretoises de 8 mm ou plus - voir également la note sur le connecteur PoE ci-dessous).
6. Si elle alimente en arrière via le connecteur GPIO, le HAT doit être capable de fournir au moins 1,3A en continu au Pi (mais une capacité de 2A en continu est recommandée).

Bien sûr, les utilisateurs sont libres d'ajouter une ID EEPROM sur des cartes qui ne respectent pas autrement le reste des spécifications - en fait, nous encourageons fortement cela ; nous voulons simplement que les produits appelés HAT soient des entités connues et bien spécifiées pour faciliter la vie des clients, en particulier les moins techniques.

REMARQUE que le Pi3B+ a introduit un nouveau connecteur PoE à 4 broches près du trou de montage en haut à droite. Les HAT récemment conçus qui ne prévoient pas de connecteur pour ce connecteur doivent éviter de l'encombrer.

## Ressources de Conception

Avant de concevoir toute nouvelle carte additionnelle (conforme ou non aux spécifications HAT), veuillez lire attentivement le [guide de conception](designguide.md).

Pour savoir ce qu'il faut flasher dans l'ID EEPROM, consultez la [spécification du format des données EEPROM](eeprom-format.md).

Des outils et de la documentation sur la façon de flasher les EEPROM sont disponibles [ici](./eepromutils).

## FAQ

**Q : Je souhaite continuer à expédier une carte existante / expédier une nouvelle carte qui ne se connecte qu'aux broches GPIO de 26 broches d'origine.**

C'est OK. Vous ne pouvez pas l'appeler HAT.
Si la carte alimente le Pi en arrière, nous recommandons d'ajouter la diode de sécurité conformément à l'exigence 2 des exigences de base pour les cartes additionnelles.

**Q : Je veux expédier une carte qui se connecte au connecteur GPIO de 40 broches et couvre ID_SD et ID_SC mais ne comprend pas d'EEPROM.**

C'est OK tant que cela respecte les exigences de base. Vous ne pouvez pas l'appeler HAT.

**Q : Je veux expédier une carte avec une ID EEPROM mais qui ne respecte pas les autres spécifications HAT.**

C'est OK tant que cela respecte également les exigences de base. Vous ne pouvez pas l'appeler HAT mais vous **pouvez** dire qu'elle supporte l'autoconfiguration GPIO si l'EEPROM contient des informations valides sur le vendeur, la carte GPIO et le blob DT.

**Q : Je veux expédier un HAT mais le logiciel pour créer l'EEPROM et/ou le blob DT n'est pas encore prêt.**

Nous attendons que tous les HAT aient une EEPROM correctement programmée, mais des bogues peuvent survenir, donc assurez-vous que l'EEPROM est flashable par l'utilisateur. Vous devrez ajouter une fonctionnalité permettant à l'utilisateur de déverrouiller la protection en écriture de l'EEPROM pour la (re-)flasher lui-même comme suggéré dans le [guide de conception](designguide.md). Veuillez fournir des instructions sur votre site Web / emballage du produit sur la manière de re-flasher la carte lorsqu'une nouvelle image devient disponible.

**Q : J'utilise la spécification mécanique HAT mais je ne veux pas / ne peux pas ajouter la découpe / fente pour le câble flex d'affichage / caméra.**

C'est OK et la carte respecte toujours les spécifications HAT. Certains HATs ne pourront pas supporter la fente / découpe en fonction de l'emplacement des connecteurs et des composants (mais il est recommandé de les supporter si possible).

**Q : Je veux créer une carte qui se connecte aux broches 'RUN' ou 'PEN'.**

Pas de problème, mais vous ne pouvez pas l'appeler HAT.
Les HATs sont conçus pour être faciles à utiliser. L'utilisation de la broche RUN nécessite que l'utilisateur soude un connecteur sur le Pi, ce n'est donc pas quelque chose que nous souhaitons inclure dans la spécification HAT.
