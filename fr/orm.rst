Accès Base de Données & ORM
###########################

La manipulation de données de la base de donnés dans CakePHP est réalisée
avec deux types d'objets principaux. Les premiers sont des **repositories** ou
**table objects**.
Ces objets fournissent un accès aux collections de données. Ils vous permettent
de sauvegarder de nouveaux enregistrements, de modifier/supprimer des
enregistrements existants, définir des relations et d'effectuer des opérations
en masse. Les seconds types d'objets sont les **entities**. Les Entities
représentent des enregistrements individuels et vous permettent de définir
des comportements et des fonctionnalités au niveau des lignes/enregistrements.

Ces deux classes sont habituellement responsables de la gestion de presque
tout ce qui concerne les données, leur validité, les interactions et
l’évolution du flux d’informations dans votre domaine de travail.

L'ORM intégré dans CakePHP se spécialise dans les bases de données
relationnelles, mais peut être étendu pour supporter des sources de données
alternatives.

L'ORM de CakePHP emprunte des idées et concepts à la fois aux patterns
de ActiveRecord et de Datamapper. Il a pour objectif de créer une implémentation
hybride qui combine les aspects des deux patterns pour créer un ORM rapide et
facile d'utilisation.

Avant de commencer à explorer l'ORM, assurez-vous de :ref:`configurer votre
connexion à la base de données <database-configuration>`.

Exemple Rapide
==============

Pour commencer, vous n'avez à écrire aucun code. Si vous suivez les
:ref:`conventions de CakePHP pour vos tables de base de données <model-and-database-conventions>`,
vous pouvez simplement commencer à utiliser l'ORM. Par exemple si vous voulez
charger des données de la table ``articles``, il suffit de créer votre table de
classe ``Articles``. Créez le fichier **src/Model/Table/ArticlesTable.php** avec
le code suivant::

    <?php
    namespace App\Model\Table;

    use Cake\ORM\Table;

    class ArticlesTable extends Table
    {
    }

Ensuite, dans un controller ou une command CakePHP pourra créer une instance
pour nous::

    public function uneMethode()
    {
        $this->loadModel('Articles');
        $resultset = $this->Articles->find()->all();

        foreach ($resultset as $row) {
            echo $row->title;
        }
    }

Dans d'autres contextes, vous pouvez utiliser ``LocatorAwareTrait``, qui ajoute
des accesseurs pour les tables de l'ORM::

    use Cake\ORM\Locator\LocatorAwareTrait;

    public function uneMethode()
    {
        $articles = $this->getTableLocator()->get('Articles');
        // le reste du code.
    }

Depuis une méthode statique, vous pouvez utiliser
:php:class:`~Cake\\Datasource\\FactoryLocator` pour obtenir le locator de la
table::

     $articles = TableRegistry::getTableLocator()->get('Articles');

Les classes de tables représentent des **collections** ou **entities**. À
présent, créons une classe d'entity pour nos Articles. Les classes Entity vous
permettent de définir des accesseurs et mutateurs, une logique personnalisé pour
les enregistrements pris individuellement, et bien d'autres choses. Commençons
par ajouter ceci à **src/Model/Entity/Article.php** après la balise d'ouverture
``<?php``::

    namespace App\Model\Entity;

    use Cake\ORM\Entity;

    class Article extends Entity
    {
    }

Les Entities utilisent la version CamelCase du nom de la table comme nom de
classe par défaut. Maintenant que nous avons créé notre classe entity, quand
nous chargeons les entities de la base de données, nous obtenons les instances
de notre nouvelle classe Article::

    use Cake\ORM\Locator\LocatorAwareTrait;

    $articles = $this->getTableLocator()->get('Articles');
    $resultset = $articles->find()->all();

    foreach ($resultset as $row) {
        // Chaque ligne est maintenant une instance de notre classe Article.
        echo $row->title;
    }

CakePHP utilise des conventions de nommage pour relier les classes Table
et Entity. Si vous avez besoin de personnaliser l'entity qu'une table utilise,
vous pouvez utiliser la méthode ``entityClass()`` pour définir un nom de
classe spécifique.

Regardez les chapitres sur :doc:`/orm/table-objects` et les :doc:`/orm/entities`
pour plus d'informations sur la façon d'utiliser les objets table et les
entities dans votre application.

Pour en savoir plus
===================

.. toctree::
    :maxdepth: 2

    orm/database-basics
    orm/query-builder
    orm/table-objects
    orm/entities
    orm/retrieving-data-and-resultsets
    orm/validation
    orm/saving-data
    orm/deleting-data
    orm/associations
    orm/behaviors
    orm/schema-system
    console-commands/schema-cache
