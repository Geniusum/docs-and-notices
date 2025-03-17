## Préface

> **"N’oubliez pas que le code n’est que le langage dans lequel nous exprimons finalement les exigences. Nous pouvons créer des langages plus proches des exigences. Nous pouvons créer des outils qui nous aident à analyser et à assembler ces exigences en structures formelles. Mais nous n’enlèverons jamais une précision nécessaire. Par conséquent, il y aura toujours du code."**
> *Robert C. Martin, Coder proprement*

*Quoi ? Un langage de plus ?* Et bien oui, un langage de plus... Mais celui-ci n'est pas fait n'importe comment et n'est pas destiné à tout usage. MazeGroup essaie de son mieux de créer des logiciels complets et maintenables. Jusqu'à présent, nous utilisions nos bons vieux langages que sont Python, PHP et JavaScript. Parfois, nous nous aventurions dans le bas niveau avec du C++. Cependant, il est temps de changer nos habitudes et de nous spécialiser dans un autre univers : celui de l'ingénierie à la MazeGroup.

Ce document n'est pas censé traiter du modèle formel de MazeGroup, qui vise à uniformiser notre expertise pour améliorer notre façon de travailler. Mais le langage de programmation Pimo, bien que créé avant le lancement officiel de cette réforme, fait, entre autres, partie des solutions qui nous sont proposées.

Pimo, dont le compilateur a été écrit en Python, est construit sur des bases simples. En constante évolution, ces bases nécessitent elles aussi une flexibilité et une maintenance rigoureuse. Un langage de programmation est une responsabilité importante, même s'il n'est adopté que par un seul organisme. À ses débuts, son code source n'était que l'œuvre d'une seule personne. Cette personne a pris des décisions contraignantes, a dû réécrire son code pour employer d'autres outils, a passé de nombreuses heures à *debugger* du langage assembleur, a tenté de relire son ancien code, a modifié la syntaxe pour s'adapter aux limitations, a pris l'habitude de lancer sa session de codage sur Pimo au début de sa journée, a commencé à contribuer depuis son téléphone dans les transports, et a finalement constaté que tous ces efforts avaient abouti à... un code moyennement correct. Un langage dans lequel, après y avoir codé un peu trop longtemps, on finit par découvrir des problèmes. Un compilateur qui génère des représentations intermédiaires de plus de trente mille lignes de code à analyser. Des parties du code source datent encore de l'époque où le compilateur utilisait *flat assembler*. Cependant, ce compilateur reste un minimum flexible, et des tests unitaires pourront encore être appliqués pour le sauver. Il est encore relativement lisible et organisé. Il a été écrit par une personne soigneuse de son code, malgré elle, en plus d'être sage et réfléchie. Cette personne, c'est moi. Et je vous laisse à disposition un amas de code qui conserve un peu d'espoir, un potentiel qu'il serait absurde de laisser exploiter par une seule personne.

Ce langage se doit d'être expressif et complet (en commençant par être *Turing-complet*), et ce, dans l'intérêt de MazeGroup. Ce document n'a pas pour but de vous exposer les lignes directrices du projet, les valeurs du langage ou ses domaines d'excellence attendus. Tout cela vous sera expliqué par moi une fois que vous aurez appris à apprivoiser et à contribuer au compilateur, c'est-à-dire après avoir lu ce document dans son intégralité. Ce ne sera sûrement pas très compliqué (en tout cas, je l'espère pour vous). Il s'agit surtout de ne pas se dire : *Mais qu'est-ce qu'une seule personne aurait pu créer comme désastre en un peu plus d'un mois ? À quoi m'attendre ?* Pas de panique, ce n'est pas aussi brouillon que mes schémas initiaux ou mes langages de test, dont je vous épargne les détails pour ne pas vous déstabiliser.

*Quid de l'état actuel du langage ?* Il se porte plutôt bien, du moins à première vue ou lors des premiers tests simples. Par exemple, une fonction `main` retournant `5 + 4` est un programme assez fonctionnel pour un compilateur écrit en Python. Mais attention : ne laissez pas le programme intégrer des conditions, des boucles ou d'autres éléments essentiels, car les choses pourraient se compliquer. Bien sûr, puisque vous êtes chez MazeGroup, cela signifie que vous savez réagir à une telle situation : tenter de toutes vos forces de résoudre les problèmes un par un. Cependant, ce n'est pas si simple ! Pour ma part, résoudre un problème à un endroit déclenchait (ou re-déclenchait) un problème ailleurs, en plus de me faire passer plusieurs heures, voire plusieurs jours, dessus. Et n'oublions pas que le langage doit être multiplateforme, mais ce problème sera sûrement résolu avec un simple conteneur Docker.

Si vous parvenez à voir le potentiel de ce langage et la flexibilité qu'il pourrait offrir, vous pourrez lire cette documentation assez facilement, non pas parce que vous comprendrez tout immédiatement, mais parce que vous anticiperez déjà la suite en la lisant.

Mon but dans ce document — si vous ne l'avez pas encore compris — est de vous donner les clés pour vous permettre de contribuer au code de Pimo et par conséquent exploiter son potentiel dont je vous parlais juste avant.

> **"De quel désert est entouré le génie !"**
> *Friedrich Nietzsche*

## Installation locale

Actuellement, il n'y a pas d'image Docker. Vous serez donc contraint d'installer le *repository* GitHub localement sur votre machine pour effectuer vos tests.

Procédez ainsi à chaque fois que vous voudrez avoir une version du code source à jour :
- Clonez le *repository* depuis GitHub:
```bash
git clone https://github.com/Geniusum/pimo
```
- Rendez-vous dans le répertoire créé.
- Exécutez le script `genvenv` (existe en version shell et batch) :
```shell
./genvenv
```
- Exécutez le script `pimo` sans aucun argument :
```shell
./pimo
```
- Vous devriez avoir ce type de sortie :
```
usage: pimo [-h] [-t] [-o OUTPUT] [-p] [-b] [-sl] [-s] [-kll] [-ko] [-r] [-ul] [-ue] [-c] [-e] [-opt] [-w] sourcecode
pimo: error: the following arguments are required: sourcecode
```

Voilà donc l'environnement Python installé.

## Apprivoiser Pimo

### Premier script Pimo

Dans le répertoire `tests/` se trouvent tout les tests de script Pimo. Il se peut qu'il y ait des fichiers binaires, ils ne sont pas ignoré par le fichier `.gitignore`. Les fichiers sources Pimo terminent par l'extension `.pim`, le compilateur donnera une erreur de type `InvalidFileExtension` si ce n'est pas le cas lors de la compilation.

Créez (dans ce répertoire `tests/`) donc un fichier du nom de votre choix : par exemple `foo.pim` et remplissez le de ce code source :
```pimo
func byte main() {
	return 80;
}
```
Puis compilez et exécutez ce code source avec cette commande :
```shell
./pimo tests/foo.pim -r -e -c -sl
```
La sortie est censé être :
```
》[ERROR] Exception during the output execution : Command '"./tests/foo"' returned non-zero exit status 80.
```
Si c'est le cas, Pimo fonctionne sur votre machine !

Pour la faire simple et courte, on crée un programme qui possède une fonction principale, cette dernière va retourner l'octet 80 en tant que code de sortie, d'où l'erreur d'exécution du compilateur. Nous parlerons davantage du langage et du compilateur par la suite.

### Utiliser le compilateur

Vous l'aurez donc compris : `./pimo` est la commande du compilateur. De fait, savoir l'utiliser dans son entièreté est déjà un grand fondement. Nous parlerons plus tard du comment il marche, pour l'instant intéressons nous à comment l'utiliser.

Tout d'abord, la commande nécessite un argument obligatoire étant le chemin d'accès du code source. Le reste de la commande ne sera que des options.

Voici quelques options importantes à s'avoir utiliser :
- `-r` `--replace` pour remplacer les fichiers binaires, objets et autres fichiers générés ;
- `-o` `--output` pour choisir le fichier binaire de sortie ;
- `-sl` `--silent` pour n'avoir que les informations nécessaire lors de l'exécution du compilateur (ex. les erreurs) ;
- `-c` `--chmod` `--change-mod` pour changer le mode du fichier binaire (sous Linux), n'est pas souvent nécessaire ;
- `-e` `--execute` pour exécuter le programme immédiatement après la compilation ;
- `-opt` `--optimize` pour optimiser la représentation intermédiaire, parfois nécessaire pour éviter les erreurs de segmentation ;
- `-t` `--timer` pour afficher le temps d'exécution de la commande du compilateur ;
- `-kll` `--keep-llvm` pour garder la représentation intermédiaire ;
- `-ko` `--keep-obj` pour garder le fichier objet.
D'autres options seront peut-être mentionnés par la suite dans ce document. Pour afficher toutes les options possibles, faites `./pimo -h`.