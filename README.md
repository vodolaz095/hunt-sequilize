[![Build Status](https://travis-ci.org/vodolaz095/hunt-sequilize.svg?branch=master)](https://travis-ci.org/vodolaz095/hunt-sequilize)

hunt-sequilize
==============

Use [Sequilize.js](http://sequelizejs.com/) models in [Hunt.js](https://huntjs.herokuapp.com/) framework of **v.0.2.0** and higher

Example
==============

```javascript

    var hunt = require('hunt'),
      huntSequilize = require('hunt-sequilize'),
      Hunt = hunt({
        'sequelizeUrl': 'sqlite://localhost/' //create database in memory
      });
    
    huntSequilize(Hunt); //this it it
    
    Hunt.extendModel('Planet', function (core) {
      return core.sequelize.define('Planet', {
        name: core.Sequelize.STRING
      });
    });
    
    Hunt.on('start', function () {
      Hunt.sequelize.sync().success(function () {
    
        Hunt.model.Planet.create({
          name: 'Earth'
        }).success(function (planet) {
          console.log('Planet "'+planet.name+'" is recorded to database');
          Hunt.model.Planet
            .find({ where: {name: 'Earth'} })
            .success(function (planetFound) {
              console.log('Planet "'+planetFound.name+'" is found to database');
              Hunt.stop();
            });
        });
      });
    });
    Hunt.startBackGround();


```

Configuration parameters
==============

Only one configuration parameter is accepted - `sequelizeUrl`. It have to be passed as `config` object parameter.
It can be populated from environmental values of `DATABASE_URL` (usually used by Heroku PostgreSQL),
`CLEARDB_DATABASE_URL`, or `SQL_URL`. It can have the following syntax:


Settings for sqlite3 database
==============

```javascript

   var Hunt = hunt({
     'sequelizeUrl': 'sqlite://localhost/' //create database in memory
   });

   var Hunt = hunt({
     'sequelizeUrl': 'sqlite://localhost/path/to/database.db' //create database in a local file
   });


```


Settings for MySql database
==============

```javascript

   var Hunt = hunt({
     'sequelizeUrl': 'mysql://myLogin:myPassword@localhost:3306/myDatabaseName'
   });


```


Settings for PostgreSQL database
==============

```javascript

   var Hunt = hunt({
     'sequelizeUrl': 'postgres://myLogin:myPassword@localhost:5432/myDatabaseName'
   });


```