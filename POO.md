# Les classes (POO) :
  #### Principe/Structure/Création 
Un objet est la représentation informatique d'une entité matérielle ou immatérielle qui a des proporités/données et/ou des actions.

Les propriétés=attributs, sont des caractères/données propres à l'objet.

Les méthodes dont les actions applicables à l'objet. La class est le modèle qui définit la structure de l'objet (ses propriétés+méthodes).

Le type de l'objet = le nom de la class qui l'a créé.     

On peut faire des objets comme avec JS, pour ne pas avoir à réécrire plein de fois les choses avec un tableau associatif à chaque fois quand les données se répètent.
    
On crée une class par fichier.php, le fichier et la class ayant le même nom.   

```
    classe NomClassEnPascalCase {
    public/private $nomPropriété1EnCamelCase;
    public/private $nomPropriété2EnCamelCase;

    public_function __cosntruct ($prop1, $prop2=null)
    {
    $this -> nomPropriété1EnCamelCase = $prop1;
    $this -> nomPropriété2EnCamelCase = $prop2;
    }

    public/private function nomFonction()
        {
        instructions ;
        }
    }
```
#### $this
est une variable accessible dans le contexte de l'objet, qui fait référence à l'objet lui-même. On peut préciser le type de variable attendues dans les parenthèses de __construc( string/ int $prop1).

Pour créer un objet selon cette trame (instancer une classe), on appele le cosntruct grâce à new:

```
    ($nomObjectifFacultatif =) new NomClass("valeur1", "valeur2")
```
L'opérateur objet (object operator) -> remplace le . des objets JS donc pour appeler une méthode ou utiliser une propriété par exemple on écrit :

```
$nomObjet -> nomFonction();
$nomObjet -> nomPropriété; 
```
On peut aussi créer une méthode __destruct()
On peut donner des valeurs aux propriétés dès la déclaration dans la classe.

```
 public/private $nomPropriété3EnCamelCase = "valeur1";
```

PARTIE VISIBILITE :
* public : autorise l'accès direct aux propriétés et aux méthodes de l'objet depuis "l'exterieur".  __construct est en publique pour pouvoir l'utiliser.

* private : les propriétés et les méthodes de l'objet sont accessibles uniquement depuis le contexte interne de l'objet. Généralement toujours ça pour les propriétés.

#### Ces états répondent à un des 3 principes de la POO:

* L'encapsulation.
  
Il s'agit de regrouper des données avec un ensemble de méthode pour les lire et les manipuler,
sans possibilité d'y avoir accès hors de ces méthodes proposées. Cela permet de garantir leur intégrité.
On accède alors aux propriétés par des méthodes Getters et Setters. Ce sont des méthodes standards qu'on peut mettre grâce à une extension VSC ou manuellement;

* Getter (pour lire une propriété):
 ```public function getPropriété1()
  {
      return $this ->propriété1;
  } 
```
On peut ajouter : 
 string/int/float
 après 
getPropriété1()
 pour forcer à renvoyer un type.

* Setter (pour attribuer une valeur à une propriété):

```
    public function setPropriété1($valeur1) 
{
    if(condition)
    $this->propriété1 = $valeur1;
    return $this;
}
```

On peut aussi mettre le type dans la parenthèse de la méthode directement et celle en retour pour être sûr que ce sera le bon: 
```
setPropriété1(int/string $valeur1): self
```

En mettant la ligne return $this, on peut intégrer les setters dans le construct et enchaîner les déclarations de méthode.
```public function construct ($prop1, $prop2, $prop3=null)
{
$this -> setPropriété1($prop1);
$this -> setPropriété2($prop2);
$this -> setPropriété3($prop3);
}

$nomObject->setProp1("valeur1")->setprop2("valeur2");
```

Méthodes magiques PHP (extension VSC Getter and Setter)

#### Le front controller :

Sur un site web, on garde un point d'entrée unique (=le front controller) qui est appelé. C'est le index.php
Dans celui-ci, on charge/appelle des fichiers de data ou de template différents grâce à des conditions qui utilisent des variables, $_GET et ce qu'il y dans l'url quand on "bouge" de page mais en fait, on reste sur le code de index.php
