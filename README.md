# INSTALLATION DE GRUNT

## 1- Prérequis:

La procédure décrite ci-dessous concerne l'installation de Grunt sur un système **OS X** (10.9 Mavericks).
Certains détails peuvent différer sous MS Windows et GNU/Linux. Mais dans l'ensemble, on peut appliquer

**IMPORTANT :** Avant de commencer l'installation et le paramètrage de Grunt, **[Node.js ](http://nodejs.org/ "Node.js")** doit être installé sur le système.


## 2- Installation de GRUNT sur le système:
	sudo npm install -g grunt-cli

**Note:** Il est important d'utiliser `sudo` sous Mac OS X et GNU/Linux pour donner au script d'installation tous les droits nécessaires sur le système. En effet, certains fichiers seront installés en dehors de l'environnement lié à l'utilisateur courant.

## 3- Installation du module GRUNT

**Note**: l'installation du module se fait uniquement de façon spécifique pour un projet et non de façon générale pour le système hôte.

1/ Création d'un fichier `package.json` en ligne de commande (dans le dossier du projet) :

	npm init

2/ Installation du module Grunt en lui-même:

	npm install grunt --save-dev

Action de la commande -> création d'un dossier `node_modules` dans le dossier courant du projet

**Note #1**: si travail avec Hammer, indiquer au logiciel d'ignorer ce dossier, ainsi que le fichier `package.json`

**Note #2**: seul le fichier `package.json` est indispensable car il contient les infos du project ainsi que toutes les dépendances qui sont liées. Pour installer les dépendances à partir du fichier `package.json`, il suffit de se placer dans le dossier du projet, à la racine (là où se trouve le fichier `package.json`) et de lancer la commande:

	npm install

3/ Grunt a également besoin d'un fichier `gruntfile.js` qui doit aussi se trouver à la racine du projet. Ce fichier va contenir l'ensemble des fonctions et automatisations que devra réaliser Grunt.

	module.exports = function(grunt){

	}

- - -

# Installation des plugins par défaut

Les plugins décrits ci-dessous sont les plus utilisés pour un projet Web classique (fichiers HTML, CSS/SCSS, JS et PNG/JPG/JPG). Il en existe un très grand nombre référencés sur le site de **[Grunt](http://gruntjs.com/plugins "Liste des plugins pour Grunt")**.

## 1- grunt-contrib-concat

Le plugin **[grunt-contrib-concat](https://npmjs.org/package/grunt-contrib-concat)** permet de concaténer plusieurs fichiers de même type (CSS et JS) dans un seul et même fichier de type identique. Pour l'installer, il faut entrer la commande suivante dans un terminal:

	npm install grunt-contrib-concat --save-dev

Note : `--save-dev` permet d'ajouter automatiquement la dépendance au fichier `package.json`. Il est dont important de ne pas omettre ce paramètre.

On charge ensuite le plugin Grunt `grunt-contrib-concat` dans le fichier `gruntfile.js`

	module.exports = function(grunt){
		grunt.loadNpmTasks('grunt-contrib-concat');
	}

Puis on charge la configuration du plugin. Le contenu du fichier `gruntfile.js` sera donc le suivant:

	module.exports = function(grunt){

		grunt.initConfig({

		  concat: {
		    options: {
		      separator: ';'
		    },
		    dist: {
		      src: ['src/intro.js', 'src/project.js', 'src/outro.js'],
		      dest: 'dist/built.js'
		    }
		  }

		});

		grunt.loadNpmTasks('grunt-contrib-concat');
	}


La section suivante permet d'initialiser l'ensemble des tâches que Grunt devra executer:

	grunt.initConfig({

	});

On y ntégre les paramètres du plugin et donc de la tâche à réaliser à l'intérieur de cette section.

**Note**:

- Dans l'exemple ci-dessus, le paramètre `dist:` peut être remplacé par n'importe quelle autre dénomination. On pourrait le nommer `concat` ou autrement. On indique ensuite comme option les fichiers à concaténer (`src:`) et quel sera le fichier de sortie, ainsi que sa destination: `dest:`.

- Le paramètre `options:` permet de définir l'ensemble des options disponibles pour le plugin qui est utilisé. Il faut consulter la documentation du plugin pour connaître l'ensemble des options qui sont disponibles.

**IMPORTANT**: il est possible, pour le paramètre `src:`, d'utiliser un astérix (\*) pour indiquer au plugin de prendre tous les fichiers du même type. Il faut prendre en compte qu'avec un astérix, le plugin `concat` va concaténer tous les fichiers identiques spécifiés sans ordre spécifique. Cela peut toutefois poser problème important avec certaines librairies Javascript, telles que **[jQuery](http://jquery.com/ "jQuery")** ou **[angular.js](http://angularjs.org/ "angular.js")** par exemple, qui doivent être chargés en premier. Il est donc important de faire bien attention à l'odre de concaténation donné en paramètre au plugin `concat`.


### Exécution des tâches par Grunt

Toujours dans l'exemple ci-dessus, une tâche spécifique a été paramétrée dans le fichier `gruntfile.js`. Pour que Grunt exécute cette tâche, il faut, toujours dans un terminal, taper la commande suivante:

	grunt concact:dist

La syntaxe est relativement simple puisqu'on invoque simple `grunt` en lui indiquant le nom de l'action à lancer, ainsi que les paramètres. À noter que le nom de l'action est optionel, et qu'il est possible de simplement invoquer `grunt` avec le nom de l'action à réaliser, c'est-à-dire, dans l'exemple ci-dessus: `dist`.

	grunt concact


## 2- grunt-contrib-uglify

Le plugin **[grunt-contrib-uglify](https://npmjs.org/package/grunt-contrib-uglify)** permet de minifier le contenu d'un ou plusieurs fichiers JS (Javascript) pour en réduire la taille. Pour l'installer, il faut entrer la commande suivante dans un terminal:

	npm install grunt-contrib-uglify --save-dev

On charge ensuite le plugin Grunt `grunt-contrib-uglify` dand le fichier `gruntfile.js`

	module.exports = function(grunt){
		grunt.loadNpmTasks('grunt-contrib-uglify');
	}

Puis, comme précédemment, on charge la configuration du plugin dans le fichier `gruntfile.js` :

	module.exports = function(grunt){

		grunt.initConfig({

		  uglify: {
		    dist: {
		      files: {
		        'dest/output.min.js': ['src/input1.js', 'src/input2.js']
		      }
		    }
		  }

		});

		grunt.loadNpmTasks('grunt-contrib-uglify');
	}


**Note**: Nous avons au total 2 tâches à exécuter : `concat` et `uglify`. Lorsqu'il est nécessaire d'exécuter plusieurs tâches à partir du fichier `gruntfile.js`, il est possible de les regrouper dans une nouvelle tâche unique qui sera représentée par un tableau comme décrit ci-dessous:

	grunt.registerTask("default", ["concact:dist", "uglify:dist"]);

`default` est le nom de la tâche principale a exécuter, suivie de la liste de toutes les tâches secondaires qui doivent être exécutées (`concat` et `uglify`).

Pour exécuter cette tâche, il suffit simplement, dans un terminal, d'invoquer `grunt` :

	grunt

La tâche `default` va exécuter automatiquement les deux tâches secondaires qui ont été placées dans le tableau.
