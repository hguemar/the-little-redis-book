# À propos de ce livre

## Licence

Le petit livre sur Redis est fourni sous la licence Attribution-NonCommercial 3.0 Unported. Vous ne devriez pas avoir
payé pour l'obtenir.

Vous êtes libres de copier, distribuer, modifier ou afficher ce livre. Cependant, je demande à ce que vous attribuiez
toujours ce livre à mon, Karl Seguin, et que vous ne l'utilisiez pas dans un but commercial.

Vous pouvez accéder au *texte complet* de **la licence à l'adresse**: 

<http://creativecommons.org/licenses/by-nc/3.0/legalcode>

## À propos de l'auteur

Karl Seguin est un développeur avec de l'expériences dans différents domaines et technologies. Il est un contributeur
actifs dans de nombreux projets Open-Source, un écrivain technique et un speaker de manière occasionnelle. Il a écrit de
nombreux articles, ainsi que certains outils en lien avec Redis. Redis propulse son service gratuit de classement et de
statistique pour les développeurs occasionnels de jeux : [mogade.com](http://mogade.com/).

Karl a écrit [The Little MongoDB Book](http://openmymind.net/2011/3/28/The-Little-MongoDB-Book/), le livre gratuit et
populaire à propos de MongoDB.

Son blog peut-être trouvé à l'adresse : <http://openmymind.net> et il tweete sur le compte :
[@karlseguin](http://twitter.com/karlseguin)

## Traduction

La traduction a été réalisée par trois personnes : [David Loureiro](http://twitter.com/DavidLoureiroFr),
[Haikel Guémar](http://twitter.com/hguemar) et [Augustin Ragon](http://twitter.com/augustin82).

### David Loureiro

David Loureiro, après des études en mathématiques appliquées et ensuite de management a créé la société SysFera avec
[Eddy Caron](http://twitter.com/CaronEddy) et [Frédéric Desprez](http://twitter.com/desprez64).
SysFera est une société d'édition de logiciels avec une expertise forte dans la mise en place de solutions Open-Source
pour la gestion et l'exploitation as a Service d'applications et d'infrastructures.
Président de SysFera, le champ d'expertise de David Loureiro couvre les domaines du Cloud Computing, de l'Open-Source,
du HPC, et du SaaS, entre autres.

### Haikel Guémar

TODO

### Augustin Ragon

TODO

## Remerciements

Un remerciement tout particulier à [Perry Neal](https://twitter.com/perryneal) pour m'avoir loué ses yeux, son esprit
et sa passion. Tu m'as fourni une aide inestimable. Merci à toi.

## Dernière version

La dernière version des sources du livre original se trouve à l'adresse suivante :
<http://github.com/karlseguin/the-little-redis-book>
Vous pourrez trouver la dernière version des sources de la traduction française à l'adresse suivante :
<http://github.com/dloureiro/the-little-redis-book>

# Introduction

Au cours de ces deux dernières années, les outils et technologies utilisés pour le requêtage et la persistence des
données ont évolué à un rythme effréné. Bien qu'il soit certain que les bases de données relationnelles ne disparaitront
pas, on peut également affirmer que l'écosystème de la gestion de données ne sera plus jamais le même.

Pour ma part, de toutes les solutions et outils qui ont émergés, Redis est de loin, le plus intéressant. Pourquoi ?
Premièrement, parce qu'il est extrêmement simple d'apprentissage. Être à l'aise avec Redis est une question d'heures.
Deuxièment, il réponds à un ensemble spécifiques de cas d'utilisation tout en restant générique. Où veut-on en venir ?
Redis n'essaie pas d'être une solution fourre-tout pour tout type de données. Au fur et à mesure que vous prenez en main
Redis, il devient de plus en plus évident de distinguer ce qu'il peut faire ou non. Et quand il peut, il se révèle d'une
grande aide pour le développeur.

Bien que vous pouvez construire un système complet à l'aide de Redis uniquement, je pense que la plupart des
utilisateurs trouveront qu'il complémente assez bien leur système de gestion de données plus génériques - que ce soit
une base de données relationnelle classique, une base orienté documents ou autre. C'est le genre de solution que vous
utiliserez pour mettre en oeuvre des fonctionnalités particulières. Dans un sens, il est similaire à un moteur
d'indexation. Vous ne construirez pas votre application toute entière autour de Lucence. Mais quand vous avez besoin
d'un moteur de recherche robuste, vous profiterez d'une bien meilleur expérience - que ce soit pour vous et vos
utilisateurs. Bien entendu, les similitudes entre Redis et les moteurs d'indexation s'arrêtent là.

L'objectif de ce bouquin est de mettre en place les bases nécessaires pour que vous puissez maitriser Redis. Nous nous
concentrons sur l'apprentissage des cinq structures de données de Redis et étudierons les différentes approches de
modélisation des données. Nous toucherons également quelques mots à propos de l'administration et des techniques de
débogage.


# Mise en route

Nous apprenons tous différemment: certains aiment mettre les mains dans le cambouis, d'uatres aiment regarder des
vidéos, et certains autres aiment lire. Rien ne vous aidera mieux à comprendre Redis qu'expérimenter par vous même.
Redis est facile à installer et est livré avec un simple shell qui vous donnera tout ce dont nous avons besoin. Prenons
un peu de temps pour l'installer et le faire fonctionner sur notre ordinateur.

## Sur Windows

Redis lui-même n'est pas officiellement supporté sur Windows, mais il y a cependant certaines options possibles. Vous ne
devriez pas les faire tourner en production, mais je n'ai jamais perçu de limitation pendant le développement.

Un port par Microsoft Open Technologies, Inc. peut être trouvé à l'adresse suivante :
<https://github.com/MSOpenTech/redis>. Au moment de l'écriture de ce livre, cette solution n'est pas prête pour être
utilisée sur des systèmes en production.

Une autre solution, disponible depuis un certain temps, peut être trouvée à l'adresse suivante :
<https://github.com/dmajkic/redis/downloads>. Vous pouvez télécharger la version la plus à jour (qui devrait être en
haut de la liste). Extrayés le fichier zip et, en fonction de votre architecture, ouvrez le répertoire `64bit` ou
`32bit`.

## Sur *nix ou MacOSX

Pour les utilisateurs d'*nix ou de Mac, compiler et générer Redis depuis les sources est la meilleure option. Les
instructions, ainsi que la dernière version, sont disponibles à l'adresse suivante : <http://redis.io/download>. Au
moment de l'écriture de ce livre, la dernière version est la 2.6.2; pour installer cette version, vous devez exécuter:

	wget http://redis.googlecode.com/files/redis-2.6.2.tar.gz
	tar xzf redis-2.6.2.tar.gz
	cd redis-2.6.2
	make

(De manière alternative, Redis est disponible à travers différents gestionnaires de paquets. Par exemple, les
utilisateurs de MacOSX disposant de Homebrew peut simplement taper `brew install redis`.)

Si vous compilez et générez Redis depuis les sources, les binaires produits seront placés dans le répertoire `src`.
Naviguez jusqu'au répertoire `src` en tapant `cd src`.

## Lancer et se connecter à Redis

Si tout a fonctionné, les binaires de Redis devraient être à votre disposition. Redis possède un grand nombre
d'exécutables. Nous allons nous concentrer sur le serveur Redis et sur la ligne de commande Redis (un client DOS-like).
Démarrons le serveur. Sous Windows, double-cliquez sur `redis-server`. Sous *nix/MacOSX lancez `./redis-server`.

Si vous lisez le message de démarrage, vous devriez vour un avertissement indiquant que le fichier `redis.conf` ne peut
pas être trouvé. Redis va utiliser les paramètres par défaut en lieu et place, ce qui est correct pour ce que nous
allons faire.

Ensuite, lancez la console Redis, soit en double-cliquant sur `redis-cli` (Windows), soit en lançant `./redis-cli`
(*nix/MacOSX). Ceci va vous connecter au serveur tournant en local sur le port par défaut (6379).

Vous pouvez tester que tout fonctionne en entrant `info` dans l'interface en ligne de commande. Vous devriez normalement
voir un ensemble de paires clé-valeur fournissant un ensemble important d'éléments d'information sur le statut du
serveur.

Si vous avez des problème avec la mise en place ci-dessus, je suggère que vous recherchiez de l'aide dans le groupe de
support officiel de Redis : [official Redis support group](https://groups.google.com/forum/#!forum/redis-db).

# Drivers Redis

Comme vous allez très bientôt l'apprendre, l'API de Redis est mieux décrite par un ensemble de fonctions. Ceci amène un
sentiment de simplicité et un aspect assez procédure quand on l'utilise. Ceci signifie notamment que, que vous utilisiez
la ligne de commande, ou un driver pour votre langage préféré, les choses seront tout à fait similaires. C'est pourquoi
vous ne devriez pas avoir de problèmes durant la lecture, si vous préférez utiliser un langage de programmation. Si vous
le souhaitez, rendez vous sur la page des clients Redis [client page](http://redis.io/clients),
et téléchargez le driver approprié.

# Chapitre 1 - Les bases

Qu'est-ce qui rends Redis spécial ? Quelle classe de problèmes résout-il ? Quels sont les points d'achoppement que les
développeurs doivent faire attention lorsqu'ils l'utilisent ? Avant de pouvoir répondre à toutes ces questions, nous
devons comprendre ce qu'est Redis.

Redis est souvent décrit comme un moteur de stockage clé/valeur en mémoire persistent. Je ne crois pas que ça soit une
description exacte. En effet, Redis stocke toutes les données en mémoire (nous en saurons plus dans quelques instants),
et il persiste bien les données sur disque, mais il est bien plus qu'un simple moteur de stockage clé/valeur. Il est
essentiel de dépasser cette idée reçue sous peine de réduire votre perspective de Redis et des problèmes qu'il est
capable de résoudre.

En réalité, Redis expose cinq structures de données différentes, une seule d'entre elle peut être définie comme une
structure clé/valeur traditionnelle. En comprenant ces cinq structures de données, leur fonctionnements, leurs
interfaces et ce que vous pouvez modéliser avec, constitue la clé pour comprendre Redis. Mais commençons d'abord par
nous pencher sur la signifaction de ce qu'est l'interface d'une structure de données.

Si nous devions appliquer ce concept de structure de données au monde relationnel, nous pourrions affirmer que les
bases de données exposent une unique structure de données : les tables. Les tables sont à la fois complexe et flexible.
Il n'y a pas grand-chose que l'on ne puisse modéliser, stocker ou manipuler avec les tables. Cependant, leur nature
générique n'est pas sans inconvénients. Plus particulièrement, tout n'est pas aussi simple ou rapide, que ce que cela
devrait être. Et si, plutôt que d'avoir une seule structure de données unique, nous utilisions des structures
spécialisées ? Certes, il y aura des choses que l'on ne pourra pas faire (ou pas aisément du moins), mais ne
gagnerions-nous pas en simplicité et rapidité ?

Utiliser des structures de données spécifiques pour des problèmes spécifiques ? Mais n'est-ce pas notre façon d'agir
lorsque nous codons ? Nous n'utilisons pas d'une table de hachage pour chaque donnée, ni n'utilisons une variable
scalaire. C'est pour moi ce qui caractérise l'approche de Redis. Si vous devez gérer des scalaires, listes, listes de
hachages ou ensembles, pourquoi ne pas les stocker en tant que scalaires, listes, listes de hachages ou ensembles ?
Pourquoi vérifier l'existence d'une variable devrait être plus complexe que d'appeler `exists(key)` ou plus lent que
0(1) (recherche à temps constants qui ne ralentira pas quelque soit la quantité de données stockée) ?


# Les briques de bases

## Bases de données

Redis propose le même concept de base de données qui vous est familier. Une base de données contient un ensemble de
données. Le cas d'utilisation typique d'une base de données est de regrouper l'ensemble des données propre à une
application et de les cloisonner vis à vis des autres applications.

Dans Redis, les bases de données sont simplement identifiés par un numéro, la base de données par défaut étant `0`. Si
vous souhaitez utiliser une base différente, vous pouvez le faire via la commande `select`. Dans l'interface en ligne de
commande, tapez `select 1`. Redis devrait vous renvoyer un message `OK` et votre invite de commande devrait changer en
quelque chose ressemblant à `redis 127.0.0.1:6379[1]>`.Si vous souhaitez revenir à la base par défaut, tapez tout
simplement dans l'invite `select 0`...

## Commandes, Clés et Valeurs

Bien que Redis soit plus qu'une base clé/valeur, chacune des cinq structures de données de Redis possède au moins une
clé et une valeur. Il est impératif de comprendre les clés et les valeurs avant de passer à autre chose.

Les clés sont le mécanisme permettant d'identifier une données. Nous manipulerons beaucoup les clés par la suite, mais
il est déjà bon de savoir qu'une clé ressemblera à `users:leto`. On peut également s'attendre à ce qu'une telle clé
pointer vers des informations à propos d'un utilisateur nommé `leto`. Les deux-points n'ont pas de signification
particulière pour Redis, mais l'utilisation d'un séparateur est une pratique commune pour l'organisation des clés.

Lesvaleurs représentent les données associées à une clé. Elles peuvent être de n'importe quel type. Parfois, vous y
stockerez des chaines des caractères, parfois des entiers, parfois des objets sérialisés (en JSON, XML ou tout autre
format). La plupart du temps, Redis traite les données comme étant un tableau d'octets et ne se préoccupe pas de leurs
types. Notons que les différents pilotes gérent la sérialisation de manière différente (certains vous en laisse la
gestion), donc nous nous limiterons uniquement aux chaines de caractères, entiers et JSON pour la suite.

Mettons les mains à la pate. Tapez la commande suivante :

	set users:leto "{name: leto, planet: dune, likes: [spice]}"

C'est la structure basique pour toute commande Redis. D'abord, nous avons la commande, ici `set`. Ensuite, nous avons
les paramètres associés. La commande `set`  prends deux paramètres : la clé que nous avons définie et la valeur que nous
lui affectons. La majorité des commandes prennent en paramètre une clé (et quand c'est le cas, c'est souvent le premier
paramètre). Pouvez-vous deviner comment récupérer cette valeur ? Il est probable que vous essayeriez(mais ne vous
inquiétez-pas si vous n'étiez pas sûr !) :

	get users:leto

Allez-y et testez d'autres combinaisons. Les clés et les valeurs sont les concepts fondamentaux, et les commandes `get`
et `set` sont les outils de base pour les manipuler. Créez plus d'utilisateurs, essayez d'autres types de clés et des
valeurs différentes.

## Requêtes

Au fur et à mesure, que nous avancerons, deux choses deviendront claires. Du point de vue de Redis, les clés sont tout
et les valeurs rien. Autrement dit, Redis ne vous permets pas de faire des requêtes à propos des valeurs d'un objet.
Dans l'exemple précédent, nous ne pouvons pas retrouver le(s) utilisateur(s) habitant la planète `dune`.

Pour beaucoup, c'est un point critique. Nous vivons dans un monde où les requêtes sont tellement puissantes et flexibles
que l'approche prônée par Redis semble primitive et abbérente. Ne vous laissez pas perturber par ceci. Rappelez-vous que
Redis n'est pas une solution universelle à tout vos problèmes. Il y aura des problèmes que vous ne pourrez pas résoudre
avec Redis (entre autre à cause des requêtes limitées). Mais considérez que vous trouverez d'autres façons de modéliser
vos données.

Nous verrons quelques exemples concrets au fur et à mesure que nous avancerons, mais il est essentiel de comprendre cet
aspect bien réel de Redis. Cela nous aide à comprendre pourquoi les valeurs stockées peuvent être de tout type - Redis
n'ayant jamais besoin de les lire ou de les traityer. Cela nous aide aussi à modéliser nos données dans cette
perspective.


## Mémoire et persistence

Nous avons donc mentionné auparavant que Redis est une base en mémoire persistente. Par rapport à la persistence, par
défaut, Redis réalise des clichés de la base sur disque en fonction du nombre de clés modifiées. Vous pouvez configurer
que toutes les X clés modifiées, la base soit sauvegardée tout les Y secondes. Par défaut, Redis sauvegarde la base
toutes les 60 secondes si 1000 ou plus clés ont été modifiés ou toutes les 15 minutes si 9 ou moins ont été modifiées.

En alternative (ou en complément), Redis peut fonctionner en mode ajout. À chaque modification de clé, un fichier en
ajout est mis à jour sur le disque. Dans certains cas, il est acceptable de perdre 60 secondes de données, en
contrepartie de meilleures performances dans le cas d'une panne matérielle ou logicielle. Dans certains cas, la perte
des données est préjudiciable. Redis vous donne ce choix. Dans le chapitre 6, nous verrons qu'il existe une troisième
option qui est de déléguer la persistence à un esclave.

Vis à vis de la mémoire, Redis conserve toutes vos données en mémoire. La conséquence évidente de ceci est que la RAM
est le premier facteur de coût matériel pour un serveur Redis.

J'ai l'impression que certains développeurs n'ont plus en tête le peu d'espace que les données peuvent occuper. Les
oeuvres complètes de William Shakespeare prennent à peu près 5.5 Mo. Pour le passage à l'échelle, les autres solutions
tendent à être limités par les I/O ou la CPU. Limitation (RAM ou I/O) qui exigeront de vous de passer à l'échellle sur
plusieurs machines en fonction des données que vous devrez stocker et traiter. À moins que vous stockiez des fichiers
multimédias très large, le stockage des données en mémoire n'est pas un problème. Pour les applications où cela peut
poser problèmes, vous devrez certainement faire un compromis pour être limité par les I/O au lieu de la mémoire.

Par ailleurs, Redis gère également la mémoire virtuelle. Néanmoins, cette fonctionnalité a été considéré comme un échec
(par les propres développeurs de Redis) et a été dépréciée.

(Notons que le fichier de 5.5Mo contenant les oeuvres complètes de Shakespeare peuvent être compressé à environ 2Mo.
Redis ne compresse pas automatiquement les données, mais comme il gére des tableaux d'octets, rien ne vous empêche de le
faire vous-même.)


## Récapitulons

Nous avons abordés un certain nombre de sujets. La dernière chose que j'aimerais avant d'entrer dans les détails est de
récapituler tout ces sujets. Plus précisement, les limitations des requêtes, les structures de données et la manière
dont Redis stocke les données en mémoire.

Quand vous mettez ces trois points ensemble, vous obtenez quelque chose de fantastique : la rapidité. Certains penseront
"Bien évidemment, Redis est rapide car tout est en mémoire." Mais c'est qu'une partie de l'explication. La véritable
raison pour laquelle Redis surpasse les autres solutions sont ses structures de données spécialisées.

Comment plus rapide ? Cela dépends d'un grand nombre de facteurs - quels sont les commandes utilisées, le type des
données, etc. Mais les performances de Redis sont de l'ordre de plusieurs dizaines de milliers ou centaines de milliers
d'opérations **par seconde**. Vous pouvez lancer `redis-benchmark` (qui est dans le même répertoire que `redis-server`
et `redis-cli`) pour vous en rendre compte.

J'ai eu l'occasion de porter un code utilisant un modèle traditionnel à Redis. Un test de chargement que j'avais écrit
prenait près de 5 minutes à s'exécuter en utilisant le modèle relationnel. Il a fallu 150ms avec Redis. Vous n'aurez pas
toujours un gain aussi massif, mais cela devrait vous donnez une idée de ce dont on parle.

Il est essentiel de comprendre cet aspect de Redis parce qu'il impacte votre façon d'interagir avec. Les développeurs
avec une expérience SQL ont souvent tendance à minimiser les interrogations à la base de données. C'est une bonne
pratique avec n'importe quelle solution, Redis inclus. Cependant, étant donné que nous gérons des structures de données
plus simples, nous devons parfois interroger le serveur Redis à plusieurs reprises pour arriver à nos fins. Ces manières
d'accèder aux données peuvent sembler peu naturelles au départ, mais elles ont un coût négligeable par rapport aux gains
en performances brutes obtenus.

## Dans ce chapitre

Bien que nous avons à peine eu l'occasion de jouer avec Redis, nous avons déjà couvert un champs très large de sujets.
Ne vous inquiétez pas si certains points restent confus - comme les requêtes. Nous irons droit au but lors du chapitre
suivant et la plupart de vos questions y trouveront j'espère une réponse.

Les point principaux abordés dans ce chapitre sont :

* Les clés sont des chaines de caractères identifiant des données (valeurs)

* Les valeurs sont des tableaux d'octets qui ne sont pas traités par Redis

* Redis expose (et utilise en interne) uniquement cinq structures de données

* Ensemble, tout les points ci-dessus font de Redis une base rapide et simple d'utilisation mais pas adaptée à tout les
scénarios

# Chaptre 2 - Les Structures de Données 

Il est temps jeter un coup d'oeil aux cinq structures de données de Redis. Nous allons expliquer ce qu'est chaque
structure de données, quelles sont les méthodes disponibles et pour quel type de fonctionnalité ou de donnée vous allez
les utiliser.

Les seules construction du langage que nous avons vu jusque là sont les commandes, les clés et les valeurs. Jusqu'ici
rien de concret à propos des structures de données. Quand nous utilisons la commande `set`, comment Redis peut-il savoir
quelle structure de données utiliser? Il s'avère que chaque commande est spécifique à une structure de données. Par
exemple, quand vous utilisez `set`, vous êtes en train de stocker une valeur dans une structure de données de type
string. Quand vous utilisez `hset`, vous êtes en train de stocker un hash. Compte tenu de la taille restreinte du
vocabulaire de Redis, cela devrait être gérable.

**[Le site web de Redis](http://redis.io/commands) a une très bonne documentation de référence. Il n'est pas ici
question de répéter le travail qu'ils ont déjà réalisé. Nous allons seulement couvrir les commandes les plus importantes
 nécessaires pour la compréhension des différentes structures de données.**

Il n'y a rien de plus important que de prendre du plaisir en essayant de tester les concepts. Vous pouvez toujours
supprimer toutes les données de votre base de données en entrant `flushdb`, alors ne soyez pas timide et essayer de
tenter des choses un peu folles!

## Strings

Les Strings sont les structures de données disponibles dans Redis les plus basiques., Quand nous pensez à une paire
clée-valeur, vous pensez à des strings. Ne soyez pas perplexe par rapport au nom, comme toujours, votre valeur peut
être n'importe quoi. Je préfère les appeler des 'scalaires', mais ce n'est peut-être que moi.

Nous avons déjà vu un cas classique d'usage des strings, pour le stockage d'instances d'objets via des clées. C'est
quelque chose que vous risquez d'utiliser très souvent :

	set users:leto "{name: leto, planet: dune, likes: [spice]}"

De manière aditionnelle, Redis vous permet de réaliser certaines opérations très classiques. Par exemple `strlen <key>`
peut être utilisée pour récupérer la longueur d'une valeur associée à une clée; `getrange <key> <start> <end>` retourne
la sous-chaîne correspondante de la valeur associée à la clée fournie; `append <key> <value>` ajoute la valeur à une
valeur existante (ou la créée si elle n'existait pas déjà). Allez-y et essayez ces opérations. Voici ce que j'obtiens :

	> strlen users:leto
	(integer) 42

	> getrange users:leto 27 40
	"likes: [spice]"

	> append users:leto " OVER 9000!!"
	(integer) 54

Maintenant, vous devriez vous dire : c'est super, mais ça n'a pas de sens. Vous ne pouvez pas récupérer une partie
d'une donnée JSON ou ajouter une valeur simplement. Vous avez raison, la leçon à tirer ici est que certaines commandes,
et plus particulièrement avec les structures de données de type strings, ne font sens pour que des types de données
spécifiques.

Plus haut nous avons appris que Redis ne tient aucun compte des valeurs. La plupart du temps c'est vrai. Cependant,
quelques commandes relatives aux strings sont spécifiques à certains types ou structures de données. Comme vague
exemple, je vois notamment l'usage des fonctions déjà citées comme `append` et `getrange` qui sont utiles dans quelques
cas de sérialisation efficace en terme d'espace mémoire. Comme exemples plus concret, je vous donne les commandes
`incr`, `incrby`, `decr` et `decrby`. Ces commandes incrémentent ou décrémentent la valeur d'une string:

	> incr stats:page:about
	(integer) 1
	> incr stats:page:about
	(integer) 2

	> incrby ratings:video:12333 5
	(integer) 5
	> incrby ratings:video:12333 3
	(integer) 8

Comme vous pouvez l'imaginer, les strings Redis sont parfaites pour les statistiques. Essayez d'incrémenter `users:leto`
 (une valeur non-entière) et voyez ce qui arrive (vous devriez avoir une erreur).

Un exemple plus avancé est celui des commandes `setbit` et `getbit`. Il y a un [superbe article](http://blog.getspool
.com/2011/11/29/fast-easy-realtime-metrics-using-redis-bitmaps/) sur la façon dont Spool utilise ces deux commandes
pour répondre de manière efficace à la question "combien de visiteurs uniques avons nous eu aujourd'hui?". Pour 128
millions d'utilisateurs, un ordinateur portable génère la réponse en moins de 50 ms et n'utilise seulement que 16MB
de mémoire.

Il n'est pas important que vous compreniez la manière dont fonctionnent les bitmaps,
ou la façon dont Spool les utilise, mais il est plutôt important que vous compreniez que les strings de Redis sont
plus puissantes que ce qu'elles semblaient initialement. Toujours est-il que les cas d'usage les plus fréquents sont
ceux qui ont été donnés plus haut : stockage d'objets (complexes ou non) et compteurs. Enfin,
comme il est très rapide de récupérer des valeurs grâce à leurs clés, les strings sont souvent utilisées pour cacher
des données.

## Hashes

Hashes are a good example of why calling Redis a key-value store isn't quite accurate. You see, in a lot of ways, hashes
 are like strings. The important difference is that they provide an extra level of indirection: a field. Therefore, the
 hash equivalents of `set` and `get` are:

	hset users:goku powerlevel 9000
	hget users:goku powerlevel

We can also set multiple fields at once, get multiple fields at once, get all fields and values, list all the fields or
delete a specific field:

	hmset users:goku race saiyan age 737
	hmget users:goku race powerlevel
	hgetall users:goku
	hkeys users:goku
	hdel users:goku age

As you can see, hashes give us a bit more control over plain strings. Rather than storing a user as a single serialized
value, we could use a hash to get a more accurate representation. The benefit would be the ability to pull and update/delete
specific pieces of data, without having to get or write the entire value.

Looking at hashes from the perspective of a well-defined object, such as a user, is key to understanding how they work.
And it's true that, for performance reasons, more granular control might be useful. However, in the next chapter we'll
look at how hashes can be used to organize your data and make querying more practical. In my opinion, this is where
hashes really shine.

## Lists

Lists let you store and manipulate an array of values for a given key. You can add values to the list, get the first or last value and manipulate values at a given index. Lists maintain their order and have efficient index-based operations. We could have a `newusers` list which tracks the newest registered users to our site:

	lpush newusers goku
	ltrim newusers 0 50

First we push a new user at the front of the list, then we trim it so that it only contains the last 50 users. This is a common pattern. `ltrim` is an O(N) operation, where N is the number of values we are removing. In this case, where we always trim after a single insert, it'll actually have a constant performance of O(1) (because N will always be equal to 1).

This is also the first time that we are seeing a value in one key referencing a value in another. If we wanted to get the details of the last 10 users, we'd do the following combination:

	keys = redis.lrange('newusers', 0, 10)
	redis.mget(*keys.map {|u| "users:#{u}"})

The above is a bit of Ruby which shows the type of multiple roundtrips we talked about before.

Of course, lists aren't only good for storing references to other keys. The values can be anything. You could use lists to store logs or track the path a user is taking through a site. If you were building a game, you might use it to track a queued user actions.

## Sets

Set are used to store unique values and provide a number of set-based operations, like unions. Sets aren't ordered but they provide efficient value-based operations. A friend's list is the classic example of using a set:

	sadd friends:leto ghanima paul chani jessica
	sadd friends:duncan paul jessica alia

Regardless of how many friends a user has, we can efficiently tell (O(1)) whether userX is a friend of userY or not:

	sismember friends:leto jessica
	sismember friends:leto vladimir

Furthermore we can see what two or more people share the same friends:

	sinter friends:leto friends:duncan

and even store the result at a new key:

	sinterstore friends:leto_duncan friends:leto friends:duncan

Sets are great for tagging or tracking any other properties of a value for which duplicates don't make any sense (or where we want to apply set operations such as intersections and unions).

## Sorted Sets

The last and most powerful data structure are sorted sets. If hashes are like strings but with fields, then sorted sets are like sets but with a score. The score provides sorting and ranking capabilities. If we wanted a ranked list of friends, we might do:

	zadd friends:duncan 70 ghanima 95 paul 95 chani 75 jessica 1 vladimir

Want to find out how many friends `duncan` has with a score of 90 or over?

	zcount friends:duncan 90 100

How about figuring out `chani`'s rank?

	zrevrank friends:duncan chani

We use `zrevrank` instead of `zrank` since Redis' default sort is from low to high (but in this case we are ranking from high to low). The most obvious use-case for sorted sets is a leaderboard system. In reality though, anything you want sorted by an some integer, and be able to efficiently manipulate based on that score, might be a good fit for a sorted set.

## In This Chapter

That's a high level overview of Redis' five data structures. One of the neat things about Redis is that you can often do more than you first realize. There are probably ways to use string and sorted sets that no one has thought of yet. As long as you understand the normal use-case though, you'll find Redis ideal for all types of problems. Also, just because Redis exposes five data structures and various methods, don't think you need to use all of them. It isn't uncommon to build a feature while only using a handful of commands.

# Chapter 3 - Leveraging Data Structures

In the previous chapter we talked about the five data structures and gave some examples of what problems they might solve. Now it's time to look at a few more advanced, yet common, topics and design patterns.

## Big O Notation

Throughout this book we've made references to the Big O notation in the form of O(n) or O(1). Big O notation is used to explain how something behaves given a certain number of elements. In Redis, it's used to tell us how fast a command is based on the number of items we are dealing with.

Redis documentation tells us the Big O notation for each of its commands. It also tells us what the factors are that influence the performance. Let's look at some examples.

The fastest anything can be is O(1) which is a constant. Whether we are dealing with 5 items or 5 million, you'll get the same performance. The `sismember` command, which tells us if a value belongs to a set, is O(1). `sismember` is a powerful command, and its performance characteristics are a big reason for that. A number of Redis commands are O(1).

Logarithmic, or O(log(N)), is the next fastest possibility because it needs to scan through smaller and smaller partitions. Using this type of divide and conquer approach, a very large number of items quickly gets broken down in a few iterations. `zadd` is a O(log(N)) command, where N is the number of elements already in the set.

Next we have linear commands, or O(N). Looking for a non-indexed column in a table is an O(N) operation. So is using the `ltrim` command. However, in the case of `ltrim`, N isn't the number of elements in the list, but rather the elements being removed. Using `ltrim` to remove 1 item from a list of millions will be faster than using `ltrim` to remove 10 items from a list of thousands. (Though they'll probably both be so fast that you wouldn't be able to time it.)

`zremrangebyscore` which removes elements from a sorted set with a score between a minimum and a maximum value has a complexity of O(log(N)+M). This makes it a mix. By reading the documentation we see that N is the number of total elements in the set and M is the number of elements to be removed. In other words, the number of elements that'll get removed is probably going to be more significant, in terms of performance, than the total number of elements in the set.

The `sort` command, which we'll discuss in greater detail in the next chapter has a complexity of O(N+M*log(M)). From its performance characteristic, you can probably tell that this is one of Redis' most complex commands.

There are a number of other complexities, the two remaining common ones are O(N^2) and O(C^N). The larger N is, the worse these perform relative to a smaller N. None of Redis' commands have this type of complexity.

It's worth pointing out that the Big O notation deals with the worst case. When we say that something takes O(N), we might actually find it right away or it might be the last possible element.


## Pseudo Multi Key Queries

A common situation you'll run into is wanting to query the same value by different keys. For example, you might want to get a user by email (for when they first log in) and also by id (after they've logged in). One horrible solution is to duplicate your user object into two string values:

	set users:leto@dune.gov "{id: 9001, email: 'leto@dune.gov', ...}"
	set users:9001 "{id: 9001, email: 'leto@dune.gov', ...}"

This is bad because it's a nightmare to manage and it takes twice the amount of memory.

It would be nice if Redis let you link one key to another, but it doesn't (and it probably never will). A major driver in Redis' development is to keep the code and API clean and simple. The internal implementation of linking keys (there's a lot we can do with keys that we haven't talked about yet) isn't worth it when you consider that Redis already provides a solution: hashes.

Using a hash, we can remove the need for duplication:

	set users:9001 "{id: 9001, email: leto@dune.gov, ...}"
	hset users:lookup:email leto@dune.gov 9001

What we are doing is using the field as a pseudo secondary index and referencing the single user object. To get a user by id, we issue a normal `get`:

	get users:9001

To get a user by email, we issue an `hget` followed by a `get` (in Ruby):

	id = redis.hget('users:lookup:email', 'leto@dune.gov')
	user = redis.get("users:#{id}")

This is something that you'll likely end up doing often. To me, this is where hashes really shine, but it isn't an obvious use-case until you see it.

## References and Indexes

We've seen a couple examples of having one value reference another. We saw it when we looked at our list example, and we saw it in the section above when using hashes to make querying a little easier. What this comes down to is essentially having to manually manage your indexes and references between values. Being honest, I think we can say that's a bit of a downer, especially when you consider having to manage/update/delete these references manually. There is no magic solution to solving this problem in Redis.

We already saw how sets are often used to implement this type of manual index:

	sadd friends:leto ghanima paul chani jessica

Each member of this set is a reference to a Redis string value containing details on the actual user. What if `chani` changes her name, or deletes her account? Maybe it would make sense to also track the inverse relationships:

	sadd friends_of:chani leto paul

Maintenance cost aside, if you are anything like me, you might cringe at the processing and memory cost of having these extra indexed values. In the next section we'll talk about ways to reduce the performance cost of having to do extra round trips (we briefly talked about it in the first chapter).

If you actually think about it though, relational databases have the same overhead. Indexes take memory, must be scanned or ideally seeked and then the corresponding records must be looked up. The overhead is neatly abstracted away (and they  do a lot of optimizations in terms of the processing to make it very efficient).

Again, having to manually deal with references in Redis is unfortunate. But any initial concerns you have about the performance or memory implications of this should be tested. I think you'll find it a non-issue.

## Round Trips and Pipelining

We already mentioned that making frequent trips to the server is a common pattern in Redis. Since it is something you'll do often, it's worth taking a closer look at what features we can leverage to get the most out of it.

First, many commands either accept one or more set of parameters or have a sister-command which takes multiple parameters. We saw `mget` earlier, which takes multiple keys and returns the values:

	keys = redis.lrange('newusers', 0, 10)
	redis.mget(*keys.map {|u| "users:#{u}"})

Or the `sadd` command which adds 1 or more members to a set:

	sadd friends:vladimir piter
	sadd friends:paul jessica leto "leto II" chani

Redis also supports pipelining. Normally when a client sends a request to Redis it waits for the reply before sending the next request. With pipelining you can send a number of requests without waiting for their responses. This reduces the networking overhead and can result in significant performance gains.

It's worth noting that Redis will use memory to queue up the commands, so it's a good idea to batch them. How large a batch you use will depend on what commands you are using, and more specifically, how large the parameters are. But, if you are issuing commands against ~50 character keys, you can probably batch them in thousands or tens of thousands.

Exactly how you execute commands within a pipeline will vary from driver to driver. In Ruby you pass a block to the `pipelined` method:

	redis.pipelined do
	  9001.times do
		redis.incr('powerlevel')
	  end
	end

As you can probably guess, pipelining can really speed up a batch import!

## Transactions

Every Redis command is atomic, including the ones that do multiple things. Additionally, Redis has support for transactions when using multiple commands.

You might not know it, but Redis is actually single-threaded, which is how every command is guaranteed to be atomic. While one command is executing, no other command will run. (We'll briefly talk about scaling in a later chapter.) This is particularly useful when you consider that some commands do multiple things. For example:

`incr` is essentially a `get` followed by a `set`

`getset` sets a new value and returns the original

`setnx` first checks if the key exists, and only sets the value if it does not

Although these commands are useful, you'll inevitably need to run multiple commands as an atomic group. You do so by first issuing the `multi` command, followed by all the commands you want to execute as part of the transaction, and finally executing `exec` to actually execute the commands or `discard` to throw away, and not execute the commands. What guarantee does Redis make about transactions?

* The commands will be executed in order

* The commands will be executed as a single atomic operation (without another client's command being executed halfway through)

* That either all or none of the commands in the transaction will be executed

You can, and should, test this in the command line interface. Also note that there's no reason why you can't combine pipelining and transactions.

	multi
	hincrby groups:1percent balance -9000000000
	hincrby groups:99percent balance 9000000000
	exec

Finally, Redis lets you specify a key (or keys) to watch and conditionally apply a transaction if the key(s) changed. This is used when you need to get values and execute code based on those values, all in a transaction. With the code above, we wouldn't be able to implement our own `incr` command since they are all executed together once `exec` is called. From code, we can't do:

	redis.multi()
	current = redis.get('powerlevel')
	redis.set('powerlevel', current + 1)
	redis.exec()

That isn't how Redis transactions work. But, if we add a `watch` to `powerlevel`, we can do:

	redis.watch('powerlevel')
	current = redis.get('powerlevel')
	redis.multi()
	redis.set('powerlevel', current + 1)
	redis.exec()

If another client changes the value of `powerlevel` after we've called `watch` on it, our transaction will fail. If no client changes the value, the set will work. We can execute this code in a loop until it works.

## Keys Anti-Pattern

In the next chapter we'll talk about commands that aren't specifically related to data structures. Some of these are administrative or debugging tools. But there's one I'd like to talk about in particular: the `keys` command. This command takes a pattern and finds all the matching keys. This command seems like it's well suited for a number of tasks, but it should never be used in production code. Why? Because it does a linear scan through all the keys looking for matches. Or, put simply, it's slow.

How do people try and use it? Say you are building a hosted bug tracking service. Each account will have an `id` and you might decide to store each bug into a string value with a key that looks like `bug:account_id:bug_id`. If you ever need to find all of an account's bugs (to display them, or maybe delete them if they delete their account), you might be tempted (as I was!) to use the `keys` command:

	keys bug:1233:*

The better solution is to use a hash. Much like we can use hashes to provide a way to expose secondary indexes, so too can we use them to organize our data:

	hset bugs:1233 1 "{id:1, account: 1233, subject: '...'}"
	hset bugs:1233 2 "{id:2, account: 1233, subject: '...'}"

To get all the bug ids for an account we simply call `hkeys bugs:1233`. To delete a specific bug we can do `hdel bugs:1233 2` and to delete an account we can delete the key via `del bugs:1233`.


## In This Chapter

This chapter, combined with the previous one, has hopefully given you some insight on how to use Redis to power real features. There are a number of other patterns you can use to build all types of things, but the real key is to understand the fundamental data structures and to get a sense for how they can be used to achieve things beyond your initial perspective.

# Chapter 4 - Beyond The Data Structures

While the five data structures form the foundation of Redis, there are other commands which aren't data structure specific. We've already seen a handful of these: `info`, `select`, `flushdb`, `multi`, `exec`, `discard`, `watch` and `keys`. This chapter will look at some of the other important ones.

## Expiration

Redis allows you to mark a key for expiration. You can give it an absolute time in the form of a Unix timestamp (seconds since January 1, 1970) or a time to live in seconds. This is a key-based command, so it doesn't matter what type of data structure the key represents.

	expire pages:about 30
	expireat pages:about 1356933600

The first command will delete the key (and associated value) after 30 seconds. The second will do the same at 12:00 a.m. December 31st, 2012.

This makes Redis an ideal caching engine. You can find out how long an item has to live until via the `ttl` command and you can remove the expiration on a key via the `persist` command:

	ttl pages:about
	persist pages:about

Finally, there's a special string command, `setex` which lets you set a string and specify a time to live in a single atomic command (this is more for convenience than anything else):

	setex pages:about 30 '<h1>about us</h1>....'

## Publication and Subscriptions

Redis lists have an `blpop` and `brpop` command which returns and removes the first (or last) element from the list or blocks until one is available. These can be used to power a simple queue.

Beyond this, Redis has first-class support for publishing messages and subscribing to channels. You can try this out yourself by opening a second `redis-cli` window. In the first window subscribe to a channel (we'll call it `warnings`):

	subscribe warnings

The reply is the information of your subscription. Now, in the other window, publish a message to the `warnings` channel:

	publish warnings "it's over 9000!"

If you go back to your first window you should have received the message to the `warnings` channel.

You can subscribe to multiple channels (`subscribe channel1 channel2 ...`), subscribe to a pattern of channels (`psubscribe warnings:*`) and use the `unsubscribe` and `punsubscribe` commands to stop listening to one or more channels, or a channel pattern.

Finally, notice that the `publish` command returned the value 1. This indicates the number of clients that received the message.


## Monitor and Slow Log

The `monitor` command lets you see what Redis is up to. It's a great debugging tool that gives you insight into how your application is interacting with Redis. In one of your two redis-cli windows (if one is still subscribed, you can either use the `unsubscribe` command or close the window down and re-open a new one) enter the `monitor` command. In the other, execute any other type of command (like `get` or `set`). You should see those commands, along with their parameters, in the first window.

You should be wary of running monitor in production, it really is a debugging and development tool. Aside from that, there isn't much more to say about it. It's just a really useful tool.

Along with `monitor`, Redis has a `slowlog` which acts as a great profiling tool. It logs any command which takes longer than a specified number of **micro**seconds. In the next section we'll briefly cover how to configure Redis, for now you can configure Redis to log all commands by entering:

	config set slowlog-log-slower-than 0

Next, issue a few commands. Finally you can retrieve all of the logs, or the most recent logs via:

	slowlog get
	slowlog get 10

You can also get the number of items in the slow log by entering `slowlog len`

For each command you entered you should see four parameters:

* An auto-incrementing id

* A Unix timestamp for when the command happened

* The time, in microseconds, it took to run the command

* The command and its parameters

The slow log is maintained in memory, so running it in production, even with a low threshold, shouldn't be a problem. By default it will track the last 1024 logs.

## Sort

One of Redis' most powerful commands is `sort`. It lets you sort the values within a list, set or sorted set (sorted sets are ordered by score, not the members within the set). In its simplest form, it allows us to do:

	rpush users:leto:guesses 5 9 10 2 4 10 19 2
	sort users:leto:guesses

Which will return the values sorted from lowest to highest. Here's a more advanced example:

	sadd friends:ghanima leto paul chani jessica alia duncan
	sort friends:ghanima limit 0 3 desc alpha

The above command shows us how to page the sorted records (via `limit`), how to return the results in descending order (via `desc`) and how to sort lexicographically instead of numerically (via `alpha`).

The real power of `sort` is its ability to sort based on a referenced object. Earlier we showed how lists, sets and sorted sets are often used to reference other Redis objects. The `sort` command can dereference those relations and sort by the underlying value. For example, say we have a bug tracker which lets users watch issues. We might use a set to track the issues being watched:

	sadd watch:leto 12339 1382 338 9338

It might make perfect sense to sort these by id (which the default sort will do), but we'd also like to have these sorted by severity. To do so, we tell Redis what pattern to sort by. First, let's add some more data so we can actually see a meaningful result:

	set severity:12339 3
	set severity:1382 2
	set severity:338 5
	set severity:9338 4

To sort the bugs by severity, from highest to lowest, you'd do:

	sort watch:leto by severity:* desc

Redis will substitute the `*` in our pattern (identified via `by`) with the values in our list/set/sorted set. This will create the key name that Redis will query for the actual values to sort by.

Although you can have millions of keys within Redis, I think the above can get a little messy. Thankfully `sort` can also work on hashes and their fields. Instead of having a bunch of top-level keys you can leverage hashes:

	hset bug:12339 severity 3
	hset bug:12339 priority 1
	hset bug:12339 details "{id: 12339, ....}"

	hset bug:1382 severity 2
	hset bug:1382 priority 2
	hset bug:1382 details "{id: 1382, ....}"

	hset bug:338 severity 5
	hset bug:338 priority 3
	hset bug:338 details "{id: 338, ....}"

	hset bug:9338 severity 4
	hset bug:9338 priority 2
	hset bug:9338 details "{id: 9338, ....}"

Not only is everything better organized, and we can sort by `severity` or `priority`, but we can also tell `sort` what field to retrieve:

	sort watch:leto by bug:*->priority get bug:*->details

The same value substitution occurs, but Redis also recognizes the `->` sequence and uses it to look into the specified field of our hash. We've also included the `get` parameter, which also does the substitution and field lookup, to retrieve bug details.

Over large sets, `sort` can be slow. The good news is that the output of a `sort` can be stored:

	sort watch:leto by bug:*->priority get bug:*->details store watch_by_priority:leto

Combining the `store` capabilities of `sort` with the expiration commands we've already seen makes for a nice combo.

## In This Chapter

This chapter focused on non-data structure-specific commands. Like everything else, their use is situational. It isn't uncommon to build an app or feature that won't make use of expiration, publication/subscription and/or sorting. But it's good to know that they are there. Also, we only touched on some of the commands. There are more, and once you've digested the material in this book it's worth going through the [full list](http://redis.io/commands).

# Chapter 5 - Lua Scripting

Redis 2.6 includes a built-in Lua interpreter which developers can leverage to write more advanced queries to be executed within Redis. It wouldn't be wrong of you to think of this capability much like you might view stored procedures available in most relational databases.

The most difficult aspect of mastering this feature is learning Lua. Thankfully, Lua is similar to most general purpose languages, is well documented, has an active community and is useful to know beyond Redis scripting. This chapter won't cover Lua in any detail; but the few examples we look at should hopefully serve as a simple introduction.

## Why?

Before looking at how to use Lua scripting, you might be wondering why you'd want to use it. Many developers dislike traditional stored procedures, is this any different? The short answer is no. Improperly used, Redis' Lua scripting can result in harder to test code, business logic tightly coupled with data access or even duplicated logic.

Properly used however, it's a feature that can simplify code and improve performance. Both of these benefits are largely achieved by grouping multiple commands, along with some simple logic, into a custom-build cohesive function. Code is made simpler because each invocation of a Lua script is run without interruption and thus provides a clean way to create your own atomic commands (essentially eliminating the need to use the cumbersome `watch` command). It can improve performance by removing the need to return intermediary results - the final output can be calculated within the script.

The examples in the following sections will better illustrate these points.

## Eval

The `eval` command takes a Lua script (as a string), the keys we'll be operating against, and an optional set of arbitrary arguments. Let's look at a simple example (executed from Ruby, since running multi-line Redis commands from its command-line tool isn't fun):

    script = <<-eos
      local friend_names = redis.call('smembers', KEYS[1])
      local friends = {}
      for i = 1, #friend_names do
        local friend_key = 'user:' .. friend_names[i]
        local gender = redis.call('hget', friend_key, 'gender')
        if gender == ARGV[1] then
          table.insert(friends, redis.call('hget', friend_key, 'details'))
        end
      end
      return friends
    eos
    Redis.new.eval(script, ['friends:leto'], ['m'])

The above code gets the details for all of Leto's male friends. Notice that to call Redis commands within our script we use the `redis.call("command", ARG1, ARG2, ...)` method.

If you are new to Lua, you should go over each line carefully. It might be useful to know that `{}` creates an empty `table` (which can act as either an array or a dictionary), `#TABLE` gets the number of elements in the TABLE, and `..` is used to concatenate strings.

`eval` actually take 4 parameters. The second parameter should actually be the number of keys; however the Ruby driver automatically creates this for us. Why is this needed? Consider how the above looks like when executed from the CLI:
  
    eval "....." "friends:leto" "m"
    vs
    eval "....." 1 "friends:leto" "m"

In the first (incorrect) case, how does Redis know which of the parameters are keys and which are simply arbitrary arguments? In the second case, there is no ambiguity.

This brings up a second question: why must keys be explicitly listed? Every command in Redis knows, at execution time, which keys are going to needed. This will allow future tools, like Redis Cluster, to  distribute requests amongst multiple Redis servers. You might have spotted that our above example actually reads from keys dynamically (without having them passed to `eval`). An `hget` is issued on all of Leto's male friends. That's because the need to list keys ahead of time is more of a suggestion than a hard rule. The above code will run fine in a single-instance setup, or even with replication, but won't in the yet-released Redis Cluster.

## Script Management

Even though scripts executed via `eval` are cached by Redis, sending the body every time you want to execute something isn't ideal. Instead, you can register the script with Redis and execute it's key. To do this you use the `script load` command, which returns the SHA1 digest of the script:

    redis = Redis.new
    script_key = redis.script(:load, "THE_SCRIPT")

Once we've loaded the script, we can use `evalsha` to execute it:

    redis.evalsha(script_key, ['friends:leto'], ['m'])

`script kill`, `script flush` and `script exists` are the other commands that you can use to manage Lua scripts. They are used to kill a running script, removing all scripts from the internal cache and seeing if a script already exists within the cache.

## Libraries

Redis' Lua implementation ships with a handful of useful libraries. While `table.lib`, `string.lib` and `math.lib` are quite useful, for me, `cjson.lib` is worth singling out. First, if you find yourself having to pass multiple arguments to a script, it might be cleaner to pass it as JSON:

    redis.evalsha ".....", [KEY1], [JSON.fast_generate({gender: 'm', ghola: true})]

Which you could then deserialize within the Lua script as:

    local arguments = cjson.decode(ARGV[1])

Of course, the JSON library can also be used to parse values stored in Redis itself. Our above example could potentially be rewritten as such:

      local friend_names = redis.call('smembers', KEYS[1])
      local friends = {}
      for i = 1, #friend_names do
        local friend_raw = redis.call('get', 'user:' .. friend_names[i])
        local friend_parsed = cjson.decode(friend_raw)
        if friend_parsed.gender == ARGV[1] then
          table.insert(friends, friend_raw)
        end
      end
      return friends

Instead of getting the gender from specific hash field, we could get it from the stored friend data itself. (This is a much slower solution, and I personally prefer the original, but it does show what's possible).

## Atomic

Since Redis is single-threaded, you don't have to worry about your Lua script being interrupted by another Redis command. One of the most obvious benefits of this is that keys with a TTL won't expire half-way through execution. If a key is present at the start of the script, it'll be present at any point thereafter - unless you delete it.

## Administration

The next chapter will talk about Redis administration and configuration in more detail. For now, simply know that the `lua-time-limit` defines how long a Lua script is allowed to execute before being terminated. The default is generous 5 seconds. Consider lowering it.

## In This Chapter

This chapter introduced Redis' Lua scripting capabilities. Like anything, this feature can be abused. However, used prudently in order to implement your own custom and focused commands, it won't only simplify your code, but will likely improve performance. Lua scripting is like almost every other Redis feature/command: you make limited, if any, use of it at first only to find yourself using it more and more every day. 

# Chapter 6 - Administration

Our last chapter is dedicated to some of the administrative aspects of running Redis. In no way is this a comprehensive guide on Redis administration. At best we'll answer some of the more basic questions new users to Redis are most likely to have.

## Configuration

When you first launched the Redis server, it warned you that the `redis.conf` file could not be found. This file can be used to configure various aspects of Redis. A well-documented `redis.conf` file is available for each release of Redis. The sample file contains the default configuration options, so it's useful to both understand what the settings do and what their defaults are. You can find it at <https://github.com/antirez/redis/raw/2.4.6/redis.conf>.

**This is the config file for Redis 2.4.6. You should replace "2.4.6" in the above URL with your version. You can find your version by running the `info` command and looking at the first value.**

Since the file is well-documented, we won't be going over the settings.

In addition to configuring Redis via the `redis.conf` file, the `config set` command can be used to set individual values. In fact, we already used it when setting the `slowlog-log-slower-than` setting to 0.

There's also the `config get` command which displays the value of a setting. This command supports pattern matching. So if we want to display everything related to logging, we can do:

	config get *log*

## Authentication

Redis can be configured to require a password. This is done via the `requirepass` setting (set through either the `redis.conf` file or the `config set` command). When `requirepass` is set to a value (which is the password to use), clients will need to issue an `auth password` command.

Once a client is authenticated, they can issue any command against any database. This includes the `flushall` command which erases every key from every database. Through the configuration, you can rename commands to achieve some security through obfuscation:

	rename-command CONFIG 5ec4db169f9d4dddacbfb0c26ea7e5ef
	rename-command FLUSHALL 1041285018a942a4922cbf76623b741e

Or you can disable a command by setting the new name to an empty string.

## Size Limitations

As you start using Redis, you might wonder "how many keys can I have?" You might also wonder how many fields can a hash have (especially when you use it to organize your data), or how many elements can lists and sets have? Per instance, the practical limits for all of these is in the hundreds of millions.


## Replication

Redis supports replication, which means that as you write to one Redis instance (the master), one or more other instances (the slaves) are kept up-to-date by the master. To configure a slave you use either the `slaveof` configuration setting or the `slaveof` command (instances running without this configuration are or can be masters).

Replication helps protect your data by copying to different servers. Replication can also be used to improve performance since reads can be sent to slaves. They might respond with slightly out of date data, but for most apps that's a worthwhile tradeoff.

Unfortunately, Redis replication doesn't yet provide automated failover. If the master dies, a slave needs to be manually promoted. Traditional high-availability tools that use heartbeat monitoring and scripts to automate the switch are currently a necessary headache if you want to achieve some sort of high availability with Redis.

## Backups

Backing up Redis is simply a matter of copying Redis' snapshot to whatever location you want (S3, FTP, ...). By default Redis saves its snapshot to a file named `dump.rdb`. At any point in time, you can simply `scp`, `ftp` or `cp` (or anything else) this file.

It isn't uncommon to disable both snapshotting and the append-only file (aof) on the master and let a slave take care of this. This helps reduce the load on the master and lets you set more aggressive saving parameters on the slave without hurting overall system responsiveness.

## Scaling and Redis Cluster

Replication is the first tool a growing site can leverage. Some commands are more expensive than others (`sort` for example) and offloading their execution to a slave can keep the overall system responsive to incoming queries.

Beyond this, truly scaling Redis comes down to distributing your keys across multiple Redis instances (which could be running on the same box, remember, Redis is single-threaded). For the time being, this is something you'll need to take care of (although a number of Redis drivers do provide consistent-hashing algorithms). Thinking about your data in terms of horizontal distribution isn't something we can cover in this book. It's also something you probably won't have to worry about for a while, but it's something you'll need to be aware of regardless of what solution you use.

The good news is that work is under way on Redis Cluster. Not only will this offer horizontal scaling, including rebalancing, but it'll also provide automated failover for high availability.

High availability and scaling is something that can be achieved today, as long as you are willing to put the time and effort into it. Moving forward, Redis Cluster should make things much easier.

## In This Chapter

Given the number of projects and sites using Redis already, there can be no doubt that Redis is production-ready, and has been for a while. However, some of the tooling, especially around security and availability is still young. Redis Cluster, which we'll hopefully see soon, should help address some of the current management challenges.

# Conclusion

In a lot of ways, Redis represents a simplification in the way we deal with data. It peels away much of the complexity and abstraction available in other systems. In many cases this makes Redis the wrong choice. In others it can feel like Redis was custom-built for your data.

Ultimately it comes back to something I said at the very start: Redis is easy to learn. There are many new technologies and it can be hard to figure out what's worth investing time into learning. When you consider the real benefits Redis has to offer with its simplicity, I sincerely believe that it's one of the best investments, in terms of learning, that you and your team can make.
