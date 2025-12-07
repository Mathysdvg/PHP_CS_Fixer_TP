# TP – Utiliser PHP CS Fixer avec PhpStorm

Ce document explique **à quoi sert PHP CS Fixer**, **comment l’installer**, **comment l’intégrer dans PhpStorm**, et propose **une démonstration pratique** à intégrer dans votre projet.

---

## PHP CS Fixer, c’est quoi ?

**PHP CS Fixer** est un outil qui permet de :

* nettoyer et reformater votre code PHP,
* appliquer automatiquement un style cohérent,
* éviter les erreurs de style (indentation, espaces, accolades, etc.),
* respecter les normes standards comme **PSR-12**.

**Rappel rapide : Qu’est‑ce que PSR‑12 ?**

PSR‑12 est la norme officielle de style de code PHP. Elle définit comment organiser, indenter et écrire ton code pour qu’il soit propre, lisible et cohérent entre tous les projets.

Elle impose notamment :

l’organisation des imports (use) et du namespace,

les accolades placées sur la ligne suivante,

des espaces cohérents autour des opérateurs et des mots‑clés,

un style moderne compatible avec PHP 7/8.

PHP‑CS‑Fixer applique automatiquement cette norme lorsque tu utilises la règle @PSR12.

---

## Installation de PHP CS Fixer

### ➤ 1. Installer via Composer

Dans votre projet :

```bash
composer require --dev friendsofphp/php-cs-fixer
```

### ➤ 2. Vérifier l'installation

```bash
vendor/bin/php-cs-fixer -V
```

Vous devriez voir la version afficher.

---

## Configuration : séléctionner le fichier `.php-cs-fixer.php`

À la racine de votre projet, séléctionner ce fichier :

```php
<?php

$finder = PhpCsFixer\Finder::create()
    ->in(__DIR__)
    ->exclude('vendor');

return (new PhpCsFixer\Config())
    ->setRules([
        '@PSR12' => true,
        'array_syntax' => ['syntax' => 'short'],
        'no_unused_imports' => true,
        'binary_operator_spaces' => ['default' => 'align'],
    ])
    ->setFinder($finder);
```

Ce fichier définit :

* les dossiers scannés,
* les standards appliqués,
* les règles supplémentaires.

---

## Démonstration : Corriger un fichier PHP

### Voici le code du fichier `mauvais_code.php` :

```php
<?php

// ❌ Namespace et "use" ne doivent jamais être sur la même ligne.
namespace RoyaumeChampi; use DateTime;

class MarioAdventure {

    // ❌ Mauvaise syntaxe d'array + espaces incohérents.
    public $pieces = array(1 ,2 ,3   );

    // ❌ Espacement incorrect autour du "="
    private  $vie=3;

    function __construct( ){
        // ❌ "Pieces" avec une majuscule : propriété incorrecte.
        $this->Pieces[] =4;

        // ❌ Propriété "nom" inexistante dans la classe.
        $this->nom = 'Mario';
    }

    public function sauter( $hauteur , $force  ){

        // ❌ Pas d'espace après echo.
        // ❌ L'opération $hauteur * $force est ambiguë sans parenthèses.
        echo"Mario saute de ".$hauteur*$force." metres !"; }

    public function prendreChampi ( $type){

        // ❌ Espaces et accolades mal placées.
        // ❌ Comparaison avec == au lieu de ===.
        if($type=="rouge"){return"Mario gagne 1 vie";} else{return"Mario devient plus grand";} }

    public function entrerDansTuyau( $destination){

        // ❌ Même problème : accolades et espaces mal foutus.
        if($destination=="castle"){
            return"Bienvenue dans le chateau de Bowser !"; }else{ return "Mario arrive dans ".$destination; }
    }

}

// ❌ Instanciation sans parenthèses (mauvaise pratique).
$m = new MarioAdventure;

// ❌ Appels tout sur une seule ligne → illisible.
$m->sauter(2 ,5);

// ❌ Paramètre mal espacé + mauvaise indentation.
$r = $m->prendreChampi("rouge");

// ❌ Echo collé au texte.
echo"Résultat : ".$r;

?>

```

### Commande pour corriger :

```bash
vendor/bin/php-cs-fixer fix
```

### Résultat attendu :

```php
<?php

namespace RoyaumeChampi;
use DateTime;

class MarioAdventure
{
    public $pieces = [1 ,2 ,3   ];
    private $vie = 3;

    public function __construct()
    {
        $this->Pieces[] = 4;
        $this->nom = 'Mario';
    }

    public function sauter($hauteur, $force)
    {
        echo'Mario saute de '.$hauteur * $force.' metres !';
    }

    public function prendreChampi($type)
    {
        if ($type == 'rouge') {
            return'Mario gagne 1 vie';
        } else {
            return'Mario devient plus grand';
        }
    }

    public function entrerDansTuyau($destination)
    {
        if ($destination == 'castle') {
            return'Bienvenue dans le chateau de Bowser !';
        } else {
            return 'Mario arrive dans '.$destination;
        }
    }

}

$m = new MarioAdventure();
$m->sauter(2, 5);
$r = $m->prendreChampi('rouge');
echo'Résultat : '.$r;

```

---

## Intégration avec PhpStorm

### ➤ 1. Aller dans les paramètres

**File > Settings > Tools > External Tools**

### ➤ 2. Ajouter PHP CS Fixer

Cliquez sur **+** puis configurez :

* **Name** : PHP CS Fixer
* **Program** : `vendor/bin/php-cs-fixer.php` (ou le chemin complet)
* **Arguments** :

```
fix $FileDir$/$FileName$
```

* **Working directory** :

```
$ProjectFileDir$
```

### ➤ 3. Utilisation dans PhpStorm

* Clic droit sur un fichier → **External Tools → PHP CS Fixer**

---

## Bonus : automatiser PHP CS Fixer avec un Git Hook

dans `.git/hooks/pre-commit` :

```bash
#!/bin/sh
vendor/bin/php-cs-fixer fix --quiet
```

Rendre le hook exécutable :

```bash
chmod +x .git/hooks/pre-commit
```

Votre code sera automatiquement corrigé **avant chaque commit**.

---

## Conclusion

Avec ce TP, vous savez maintenant :

* ce qu'est PHP CS Fixer,
* comment l’installer et le configurer,
* comment l’utiliser en ligne de commande,
* comment l’intégrer dans PhpStorm,
* comment automatiser les corrections.

Votre projet PHP sera désormais **propre, moderne et cohérent** ! 💪

---
