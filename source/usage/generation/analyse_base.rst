.. _analyse_base:

====================
L'analyse de la base
====================

Les informations de la base sont analysées par la méthode « constructeur » de
gen.class.php.

La construction des formulaires se fait suivant 5 types de champs reconnus par le générateur: ::

    - string : chaîne de caractère 
    - int : nombre (entier)
    - float : nombre decimal
    - date 
    - blob : texte
    - geom : geometry (pour postgres)

Type de champs
==============

la champ String est du type openMairie (méthode setType()):

- text dans le cas général

- hiddenstatic si modification pour clé primaire

- select pour clé secondaire

Le champ date est du type openMairie date avec calendrier et java script de contrôle de saisie de date

(La date est au format français JJ/MM/AAAA)

le champ Int est du type openMairie (methode setType())

- hidden si clé primaire en ajout

- hiddenstatic si clé primaire en modification

- text avec contrôle numérique en javascript

- select pour clé secondaire

Le champ Blob est du type openMairie textarea

La longueur et la largeur sont définis en fichier de paramétrage form.inc

La taille n est pas pris en compte dans la longueur d'enregistrement

Les paramètres de dyn/form.inc permettent d'établir la longueur et la largeur d'affichage d'un blob : ::

    $max=6; // nombre de ligne blob
    $taille=80; // taille du blob

Les champs de type geometry sont des champs geom (accès a la fenetre tab_sig.php)


Equivalence type pgsql / type openMairie
========================================

L'information fournie par postgresql est moins complète que celle de mysql
surtout au niveau de la longueur des champs « string » où il est fourni :la
longueur de stockage  qui est égal à -1 quand le stockage est variable type
pgsql (longueur) type tableinfo si different -> type openMairie ::

    Bigint      (8)                 int8        -> int
    Smallint    (2)                 Int2        -> Int
    Integer     (4)                 Int4        -> Int
    Real        (4)                 Float4      -> float
    Doubleprecision (8)             Float8      -> float
    Numeric     (20)                Numeric     -> float
    Money       (8)                 Money       -> float
    Char        (1)                 Char        -> String   (Quelque soit la longueur= 1)
    Character   (-1)                Bpchar      -> String (Utilisation de la longueur d'affichage)
    Character varying (-1)          Varchar     -> String (Utilisation de la longueur d'affichage)
    Text        (-1)                text        -> blob  (Utilisation des paramètres de form.inc)
    Date        (4)                 Date        -> Date (Utilisation des paramètres de form.inc -
                                                   $pgsql_longueur_date)
    geometry    -5                              -> geom
    boleen                          bolean      -> checkbox

Pour postgresql, il est proposé dans form.inc 2 variables qui sont avec la version 4.2.0 inutiles car les longueurs sont gérées
par le générateur (valeurs négatives) ::

    $pgsql_taille_defaut = 20; // taille du champ par défaut si retour pg_field_prtlen =0
    $pgsql_taille_minimum = 10; // taille minimum d affichage d un champ

Attention, pour les champs geom, il faut gérer la carte à chercher pour l affichage de la carte en fenêtre ::

    exemple de surcharge de la méthode setSelect pour afficher la carte dossier (de la table om_sig_map)

    if($maj==1){ //modification
        $contenu=array();
        $contenu[0]=array("dossier",$this->getParameter("idx"));
        $form->setSelect('geom',$contenu);
    }

Nom de champ et nom de table
============================

Attention au nom de tables ou de champs, évitez les termes SQL : match, table,
index, type, len ... ou openMairie : objet pour les noms de champs ou table.
