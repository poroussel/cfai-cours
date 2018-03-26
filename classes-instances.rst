.. |date| date:: %d/%m/%Y
.. sectnum::
.. footer:: ###Title### - ###Page### / ###Total###

=========================
Découverte du langage C++
=========================

Évolutions et nouveautés hors système objet
===========================================

Le langage C++ est à l'origine un sur-ensemble du C, c'est-à-dire un langage entièrement compatible
(tout programme écrit en C est compilable par un compileur C++) offrant une abstraction objet et
quelques autres nouveautés. Il est toutefois important de noter que cette vérité n'en est plus
vraiment une avec les dernières évolutions des deux langages. Ces évolutions étant gérées par deux
instances différentes et qui semblent ne pas communiquer beaucoup, chaque langage suit désormais sa
propre voie.

Le type bool
------------

Les variables booléennes ont leur propre type, `bool`, qui accepte deux valeurs : `false` et `true`.

.. code:: c++

   bool maVariable = true;
   /* ... */
   if (maVariable == false) {
	  autreVariable = 66;
   }

La conversion avec les numériques est implicite : false = 0 et true = 1


Valeurs par défaut pour les arguments de fonctions
--------------------------------------------------

Il est possible d'indiquer dans la définition d'une fonction des valeurs par défaut pour certains de ses arguments.
Ces arguments pourront alors être 'oubliés' lors de l'appel à cette fontion. Par exemple :

.. code:: c++

   void afficheNombre(int nombre, int base = 10)
   {
	  switch (base) {
	  case 10:
	     printf("%d", nombre);  // Affichage en base 10
	     break;
	  case 16:
	     printf("%x", nombre);  // Affichage en hexadécimal
	     break;
	  default:
	     printf("%d", nombre);  // Base inconnue => décimal
	  }
   }
   afficheNombre(37, 16);  // Affiche 0x25
   afficheNombre(44);      // Base décimale par défaut, affiche 44

Les arguments optionnels sont définis depuis le dernier vers le premier. Il est par exemple interdit de
définir la fonction ayant un argument obligatoire entouré de deux arguments facultatifs.

.. code:: c++

   /*
    * Le code suivant n'est pas valide pour une bonne raison :
    * il est impossible de déterminer lors de l'appel de la
    * fonction si a=11, b=22 et c=2 ou si a=1, b=11 et c=22
    */
   void interdit(int a=1, int b, int c=2);
   interdit(11, 22);


Surcharge des fonctions
-----------------------

Il est possible d'utiliser le même nom pour plusieurs fonctions si la signature de chacune d'elles est unique.
La signature d'une fonction est définie par son nom, le nombre et le type de chacun de ses arguments. Le type
de retour ne modifie par la signature. Exemple :

.. code:: c++

   int minimum(int, int);
   float minimum(float, int);
   float minimum(float, float);
   /* Interdit car même signature que la première fonction */
   float minimum(int, int);


Gestion de la mémoire dynamique
-------------------------------

Le C++ introduit deux nouveaux opérateurs (et non pas fonctions, ils sont donc surchargeables)
dédiés à la gestion de la mémoire : `new` et `delete`. `new` alloue la mémoire nécessaire pour stocker
un objet (ou un tableau d'objets pour la 2ème syntaxe) dont le type est passé en
paramètre (new retourne 0 en cas d'erreur) :

.. code:: c++

   int *pEntier = new int;
   float *tableauFloat = new float[20];

L'opérateur `delete` permet de libérer la mémoire utilisée par un objet ou un tableau d'objets :

.. code:: c++

   delete pEntier;
   delete [] tableauFloat;

L'utilisation des fonctions `malloc`, `free`, `realloc` etc est possible mais un mixte des deux méthodes
n'est pas possible pour un même objet.


Passage de paramètre par référence
----------------------------------

Le passage de paramètre par référence vient s'ajouter au passage par valeur et au passage par adresse. La
syntaxe est la suivante :

.. code:: c++

   void swap(int &a, int &b)
   {
	  int tmp = a;
	  a = b;
	  b = tmp;
   }

   void main(void)
   {
	  int i = 45;
	  int j = 57;

	  /* Inverse les valeurs de i et j */
	  swap(i, j);
   }

Les variables `a` et `b` de la fonction `swap` font référence aux objets passés lors de l'appel, ce sont
donc bien les valeurs de `i` et `j` qui sont échangées. L'effet est le même qu'avec un passage par
adresse (pointeur) mais la syntaxe est simplifiée.


Nouvelle syntaxe pour les commentaires
--------------------------------------

En `C++` un double slash `//` indique un commentaire qui se poursuit jusqu'à la fin de ligne. Par exemple :

.. code:: c++

   struct Point {
	  int x; // Abcisse dans le repère orthonormé, en pixels
	  int y;
   };


Spécifités de c++11 et au-delà
==============================

Le langage C++ est, comme la plupart des langages de programmation, en évolution. À la fin des années
90 et au début des années 2000 il a connu une période de stagnation mais depuis les années 2010 il
connaît un nouvel élan.

De nouvelles versions du langage et de la librairie standard qui l'accompagne sont désormais publiées
tous les 3 ans : c++11, c++14, c++17 etc. Chaque nouvelle version apporte des nouveautés tant au niveau
du langage que de la librairie. c++11 marque une évolution majeure par rapport à la norme précédente.

Typage automatique
------------------

Le mot clé `auto` permet de laisser au compilateur la responsabilité de déterminer le type
d'une variable. Cela ne peut évidemment fonctionner que si une valeur est spécifiée lors de
la déclaration de la variable.

Par exemple on peut remplacer le code suivant :

.. code:: c++

  int compteur = 0;
  string str = "etat";
  vector<int> vct = vector<int>(11, 22);

  for (vector<int>::iterator it = vct.begin(); it != vct.end(); it++)
     cout << *it << '\n';

par

.. code:: c++

  auto compteur = 0;
  auto str = "etat";
  auto vct = vector<int>(11, 22);

  for (auto it = vct.begin(); it != vct.end(); it++)
     cout << *it << '\n';


Range based for loop
--------------------

Une nouvelle variante de `for` permet de remplacer avantageusement l'utilisation des itérateurs
lors du parcours complet d'un container. Par exemple, la boucle du paragraphe précédent peut
s'écrire ainsi :

.. code:: c++

   for (auto &e: vct)
      cout << e << endl;

Cette syntaxe est également valide pour les tableaux hérités du langage C.


Pointeur nul
------------

Le nouveau mot clé `nullptr` représente un pointeur nul et peut être affecté à tous les types de
pointeurs.


Classes, instances, objets etc
==============================

Toute classe est à la fois la définition d'un nouveau type de données et de la "machine"
qui pourra créer des objets de ce type. On appelle *instanciation* la création d'objets
(ou *instances*) d'une classe.

Une classe est définie par son nom, qui sera le nom du type de données créé. Nous pouvons
par exemple définir en classe nommée `Date` ainsi :

.. code:: c++

  class Date {};

Une fois la classe définie elle peut être utilisée comme type de la même manière que les
types simples tels que `int`, `float` etc. Nous pouvons donc déclarer une variable locale
ainsi :

.. code:: c++

  int main(void)
  {
      Date var1;
      /* ... */
      return 0;
  }

Ici la classe `Date` sera instanciée lors de l'entrée dans le fonction `main` afin
de stocker la variable `var1` : un espace mémoire de taille suffisant sera alloué sur
la pile (*stack*) et initialisé par la "machine" `Date`. À la fin de la fonction, la
variable `var1` étant locale elle est détruite : l'espace mémoire qu'elle occupait est
libéré.

La création dynamique (allocation sur le tas ou *heap*) d'une instance de `Date` est
sans surprise :

.. code:: c++

  int main(void)
  {
      Date *var2 = new Date;
      /* ... */
      delete var2;
      return 0;
  }

Attributs et méthodes
=====================

Une instance (un objet) est défini par 2 choses : son état et son comportement (ce qu'il
est possible de faire avec). En l'état la classe `Date` a peu d'intérêt : une instance
de cette classe ne porte aucune information et elle ne propose aucune action. Nous allons
donc complèter la définition de la classe en lui ajoutant des membres, qui peuvent être
de 2 types :

* des attributs
* des méthodes

Chaque membre d'une classe est défini entre autre par sa visibilité (`public`, `protected`
ou `private`) qui précise qui pourra accèder à ce membre. Pour simplifier nous utiliserons
pour l'instant des membres `public`.

Les attributs
-------------

Les attributs sont des données portées par les objets de la classe et les méthodes sont
des fonctions qui définissent le comportement et les possibilités de ces objets. Chaque
date peut être définie par exemple par :

* une année
* un mois
* un jour
* un type de calendrier
* etc

Cela pourrait se transcrire ainsi :

.. code:: c++

   class Date {
   public:
       int annee;
       int mois;
       int jour;
   };

Chaque instance de la classe `Date` ainsi modifiée stockera ses propres valeurs pour les
3 attributs. Chaque instance occupera en mémoire un espace dont la taille sera au minimum
égale à la somme de l'espace nécessaire pour chaque attribut, ici environ 12 octets. Les
instances sont indépendantes les unes des autres.

Pour le moment une classe ressemble fortement à une structure, y compris pour la syntaxe
utilisée pour accèder aux attributs :

.. code:: c++

  int main(void)
  {
      Date noel, travail;

      noel.annee = 2015;
      noel.mois = 12;
      noel.jour = 25;

      travail.annee = 2016;
      travail.mois = 5;
      travail.jour = 1;

      if (noel.annee != travail.annee)
	  cout << "Différentes années" << endl;
      else
	  cout << "Même année" << endl;
      return 0;
  }


Les méthodes
------------

Les méthodes sont des fonctions s'appliquant sur une classe d'objets qui définissent le
comportement de ces objets : comment ces objets sont créés, les actions que l'on peut
effectuer sur ces objets etc.

Pour un objet de type `Date` on peut imaginer de nombreuses méthodes permettant de modifier
cet objet, d'effectuer des calculs à partir de cet objet. Il est par exemple intéressant de
déterminer si une `Date` appartient à une année bissextile, d'ajouter un nombre d'années, de
mois ou de jour à une date etc.

.. code:: c++

   class Date {
   public:
       int annee;
       int mois;
       int jour;

       bool bissextile() {
	  if (annee % 4 == 0 && (annee % 100 > 0 || annee % 400 == 0))
	     return true;
	  return false;
       }
   };

Ici `bissextile()` est une méthode qui s'applique sur un objet de classe `Date` et qui retourne
`true` si l'année stockée par cet objet est bissextile ou `false` sinon. La méthode est un membre
de la classe, on y accède donc avec la même syntaxe que celle utilisée pour accèder aux attributs.

.. code:: c++

  int main(void)
  {
      Date noel, travail;

      noel.annee = 2015;
      noel.mois = 12;
      noel.jour = 25;

      travail.annee = 2016;
      travail.mois = 5;
      travail.jour = 1;

      if (noel.bissextile() == true)
	  cout << "L'année " << noel.annee << " est bissextile" << endl;
      if (travail.bissextile() == true)
	  cout << "L'année " << travail.annee << " est bissextile" << endl;
      return 0;
  }
