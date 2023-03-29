Cher mainteneurs de package, vous versionnez probablement vos package de la mauvaise manière.

J'ai mis à jour mes dépendance et paf ! Le projet est cassé. Cela vous est-il déjà arrivé ? Une fois ? Deux fois ? Personnelement je ne compte plus.

Au cas où vous ne le sauriez pas déjà, si vous utilisez python il est possible de lister toutes ses dépendances dans un fichier requirements.txt.

Voici a quoi cela ressemble dans sa version la plus simple :
pandas
gensim
scikit-learn
mariadb

C'est très pratique sauf que vous allez ouvrir votre projet dans 6 mois la plupart des dépendances vont casser.

Alors vous vous dites, pourquoi ne pas figer les versions ?
pandas=1.5.3
gensim=3.8.0
scikit-learn=1.2.1
mariadb=1.0

Parfais, sauf que maintenant vous ne profiterez plus des mises à jour de sécurité. Il faudrait donc trouver un moyen de dire : je veux uniqumeent les mises à jours compatibles avec les version actuellemnt installées
Et là miracle, vous découvrez l'existance de la syntaxe avec un "~=", qui s'appelle la "compatible release clause" https://peps.python.org/pep-0440/#compatible-release
pandas~=1.5.3
gensim~=3.8.0
scikit-learn~=1.2.1
mariadb>=1.0,<1.1

C'est merveilleux, sauf que les mainteneurs de package utilisent mal cette notation.
La compatible release clause va autoriser les mises à jours sur toutes les version mineures. Donc si vous avez mis mariadb~=1.0, mariadb=1.1 va être considéré comme une mise à jour compatible.

Or là où le bât blesse, c'est que, si on reste sur cet exemple, la branche 1.1 du package mariadb n'est pas compatible avec la branche 1.0, et c'est même écrit dans leur documentation : https://mariadb.com/docs/xpand/connect/programming-languages/python/install/
La raison est que le package python requiert la présence d'une librarie qui doit être installée séparemment. Mettre à jour en 1.1 nécessiste donc de mettre à jour cette librarie en dehors du gestionnaire de package python.

Donc il ne s'agit pas d'une erreur, mais à l'évidence d'une mauvaise compréhension du fonctionnement des packages "compatibles".
La bonne manière aurait été de versionner le package 2.0.0, et non pas 1.1.

Mais cela est peut être dû à un problème de définition des mots. La notation est major.minor.micro. Qu'est-ce que signifie une version "majeur" après tout ?
Et voici ma définition : une version majeur, est une version qui est de facto incompatible avec les prédécentes. Cela ne dépend pas du nombre de ligne de code, et cela englobe à l'évidence une modification des API, mais pas que.

Et la version mineure dans tout ça ? Et bien dans un monde idéal, cela doit relfeter l'évolution des APIs. Il n'y a aucune honte à maintenenir plusieurs versions majeurs et faire évoluer leurs API en parrallèle, bien que cela soit très couteux.
Certaines personnes se plaignent de se faire remplacer par une IA, et bien voici une application concrète et qui rendrait service à tout le monde.

