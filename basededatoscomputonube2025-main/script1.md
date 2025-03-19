
# Base de Datos

## Visualiza en que base de datos estamos
db 
## Muestra todas las bases existentes en java 
show dbs

## Crear o usar la Base de Datos
use db1

## Mostrar las colecciones
show collections

## Crear una collección 
db.createCollection 

# Eliminar una coleccion
db.empleados.drop()

## Insertar un documento


## Inserción de documentos mas complejos con arrays


## Inserción de documentos mas complejos con anidados y ID


## Insertar Multiples Documentos


## Practica

## Cargar datos 
libros.json

## Búsquedas. Condiciones Simples de igualdad. Metodo find()
db.libros.find({ editorial: "Biblio"})
db.libros.find({ precio: 25})
db.libros.find({ titulo: 'JSON para todos'})

## Operadores de Comparación

[Operadores de Comparacion](https://www.mongodb.com/docs/manual/reference/operator/query/)

### Mostrar todos los documentos donde el precio sea mayor a 25
db.libros.find({precio:{$gte:25}})

### Mostrar los documentos cuya cantidad sea menor a 5
db.libros.find({ cantidad:{$lt:5}})

### Mostrar los documentos que pertenezcan a la editorial biblio o planeta
db.libros.find({editorial:{$in:['Biblio', 'Planeta']}})

### Mostrar todos los documentos de libros que cuesten 20 o 25
db.libros.find({precio:{$in:[20,25]}})

### Mostrar todos los documentos de libros que no cuesten 20 o 25
db.libros.find({precio:{$nin:[20,25]}})

## Recuperar una sola Fila (Devuelve el primer elemento que cumpla la condición)
db.libros.findOne({precio:{$in:[20,25]}})

db.libros.findOne()

## Operadores Lógicos
[Operadores Lógicos](https://www.mongodb.com/docs/manual/reference/operator/query/)

### Operador AND

Dos posibles opciones de AND

1. La simple, mediante condiciones separadas por comas 
db.libros.find({condicion1,condicion2})  -> Con esto asume que es un and

1. Usando el operador $and
db.libros.find({$and: [{condicion1},{condicion2}]})

#### Mostrar todos aquellos libros que cuesten mas de 25 y cuya cantidad sea inferior a 15
db.libros.find({precio:{$gt:25}, cantidad:{$lt:15}})
db.libros.find({ precio:{$gt:25}, cantidad:{$lt:15}, _id:4})
db.libros.find({ precio:{$gt:25}, cantidad:{$lt:15}, _id: {$eq:4}})

db.libros.find( { $and: [ {precio: {$gte:25}}, {cantidad: {$lt:15}}]})

### Operador OR
#### Mostrar todos aquellos libros que cuesten mas de 25 o cuya 
      cantidad sea inferior a 15

db.libros.find({ $or:[{precio:{$gt:25}},{cantidad:{$lt:15}}] })

### Ejemplo de AND y OR combinados

#### Mostrar los libros de la editorial Biblio con precio mayor a 40 o libros de la editorial Planeta con precio mayor 30

 db.libros.find( {
                    $or: [
                           { $and: [{editorial: "Biblio"}, {precio:{$gt:40}}]},
                           { $and: [{editorial: "Planeta"}, {precio:{$gt:30}}]} 
                         ]
                 }
                )   

 db.libros.find( {
                    $or: [
                           { $and: [{editorial: "Biblio"}, {precio:{$gt:30}}]},
                           { $and: [{editorial: "Planeta"}, {precio:{$gt:20}}]} 
                         ]
                 }
                )                                     

## Ver Ciertas Columnas

db.libros.find(filtro, columnas)
db.libros.find({},{titulo:1})
db.libros.find({},{titulo:1, _id:0, editorial:1})
db.libros.find({editorial:'Planeta'},{titulo:1, _id:0, editorial:1})

## Operador exists (Permite saber si un campo se encuentra o no en un documento)
db.libros.find({editorial:{$exists:true}})

db.libros.insertOne(
... {
...    _id:10,
...    ttulo: 'Mongo en entornos geograficos',
...    editorial: 'Terra',
...    precio: 125
... }
... )

db.libros.find({cantidad:{$exists:false}})

db.libros.deleteOne({_id:10}) -> No borrar el documento

## Operador Type (Permite preguntar si un determinado campo corresponde con un tipo)

[Operador Type](https://www.mongodb.com/docs/manual/reference/operator/query/type/#mongodb-query-op.-type)



db.libros.find({ precio: {$type:1}})

db.libros.find({ precio: {$type:16}})

db.libros.insertOne(
 {
    _id: 11,
    titulo: 'IA',
    editorial: 'Terra',
    precio: 125.4, 
	cantidad: 20
  }
  )

  db.libros.find({ precio: {$type:16}})

  db.libros.insertMany([
 {
    _id: 12,
    titulo: 'IA',
    editorial: 'Terra',
    precio: 125, 
	cantidad: 20
  },
  {
    _id: 13,
    titulo: 'Python para todos',
    editorial: 2001,
    precio: 200, 
	cantidad: 30
  }]
  )

  db.libros.find({ editorial: {$type:2}})

  db.libros.find({ editorial: {$type:'string'}})

  db.libros.find({ editorial: {$type:'int'}})

  ## Practica de consultas

  1. Instalar las tools de mongodb
  [DatabaseTools](https://www.mongodb.com/try/download/database-tools)

  2. Cargar el json empleados (Debemos estar ubicados en la carpeta donde se encuentra el jSON empleados.json)

  con el comando: 

  mongoimport --db curso --collection empleados --file empleados.json

  mongoimport --db curso --collection empleados --file empleados.json --port 27018
 
   
# Modificando documentos
## Comandos Importantes

1. UpdateOne -> Modificar un solo documento
1. UpdateMany -> Modificar multiples documentos
1. replaceOne -> Sustituir el contenido completo de un documento

Tiene el siguiente formato:

``` json

db.collection.UpdateOne(
{fitro}, 
{operador: })
```

[Operadores Update](https://www.mongodb.com/docs/manual/reference/operator/update/)

#### Operador $Set

1. Modificar un documento
``` json
db.libros.updateOne({titulo:'Python para todos'},{$set:{titulo:'Java para todos'}})
```

si no tiene un campo el documento lo agrega
``` json
db.libros.insertOne({
... _id:10,
... titulo: 'MongoDB en entornos geograficos',
... editorial:'Terra',
... precio:10
... })

```
``` json
db.libros.updateOne({ _id:10 }, { $set: { precio:100,cantidad: 50 } })
```
1. Modificar Multiples documentos
``` json
db.libros.updateMany(
{precio:{$gt:100}},
{$set:{precio:150}})
``` 
2. Operador $inc y $mul

``` json
 db.libros.updateMany(
... {},
... {$inc:{precio:5}})
``` 
``` json
db.libros.updateMany( {cantidad:{$gt:20}}, { $mul: { cantidad: 2 } })
```
``` json
db.libros.updateMany( {cantidad:{$gt:20}}, { $mul: { cantidad: 2, precio:2 } })
``` 

3. Reemplazar Documentos

``` json
 db.libros.replaceOne({_id:2},{titulo: 'De la Tierra a la luna', autor:'Julio Verne', editoria:'Terra', precio:100})
```
 # Borrar Documentos

1. DeleteOne -> Elimina un solo documento
1. DeleteMany -> Elimina multiples documentos

``` json
db.libros.deleteOne({_id:2})

db.libros.deleteMany(
{cantidad:{$gt:150}})
``` 

# Expesiones Regulares
db.libros.find({titulo:/t/})

db.libros.find({titulo:/JSON/})

Todos los documentos que en titulo terminen en tos
db.libros.find({titulo:/tos$/})

Todos los documentos que en titulo que comiencen con J

db.libros.find({titulo:/^J/})

# Operador $regex

[Operador Regex](https://www.mongodb.com/docs/manual/reference/operator/query/regex/)

que contengan para
db.libros.find({titulo:{$regex: 'para'}})

db.libros.find({titulo:{$regex: 'JSON'}})

db.libros.find({titulo:{$regex: /JSON/}})

Que distinga entre mayusulas y minusculas

db.libros.find({titulo:{$regex: /json/}}) -- No devuelve nada por que no distingue entre mayusculas y minusculas

db.libros.find({titulo:{$regex: /json/, $options:"i"}})

db.libros.find({titulo:{$regex: /^j/, $options:"i"}})


# Metodo sort (Ordenar Documentos)

Ordenar los libros de manera accendente por el precio
db.libros.find({}, {titulo:1, precio:1, _id:0}).sort({precio:1})

Ordenar los libros de manera dscendente por el precio
db.libros.find({}, {titulo:1, precio:1, _id:0}).sort({precio:-1})

db.libros.find({}, {titulo:1, precio:1, _id:0, editorial:1}).sort({editorial:1, precio:-1})


# Otros Métodos skip, limit, size

 db.libros.find({}, {titulo:1, precio:1, _id:0, editorial:1}).size()
 db.libros.find({}, {titulo:1, precio:1, _id:0, editorial:1}).limit(2)
 db.libros.find({}, {titulo:1, precio:1, _id:0, editorial:1}).skip(2)
 db.libros.find({}, {titulo:1, precio:1, _id:0, editorial:1}).skip(2).size()
 db.libros.find({}, {titulo:1, precio:1, _id:0, editorial:1}).skip(2).size()

 db.libros.find({}, {titulo:1, precio:1, _id:0, editorial:1}).sort({titulo:-1}).skip(2).size()
 db.libros.find({}, {titulo:1, precio:1, _id:0, editorial:1}).sort({titulo:-1}).limit(2)

 # Borrar colecciones y base de datos
  use db5
  db.createCollection('ejemplo')
  show collections

  db.ejemplo.insertOne({nombre:'Pedro'})
  db.ejemplo.drop()
  db.dropDatabase()

  # Practica_updates_delete


  ---------------------------------------------

  # Busqueda en Arrays y Documentos Anidados
  1. cargar data de ciudades en MongoCompas
  - Crear la coleccion ciudades en db1

 ## Buscar dentro de un documento anidado

  db.ciudades.find({
     alcalde: {
     nombre: "Crystal",
     apellidos: "Long",
     edad: 76
   }
 })

{"alcalde.nombre":"Crystal"}

db.ciudades.find({"alcalde.nombre":"Crystal"})


## Guardar consultas y otras peraciones interesantes en mongocompas

1. Guardar consultas en favoritos, ver en el reloj
2. Abrir mongoshell en mongocompas

## Otras Consultas con Documentos Anidados

1. Seleccionar las ciudades donde los alcaldes tengan mas de 70 años

{"alcalde.edad": {$gt:70}}

2. con proección 

db.ciudades.find({'alcalde.edad':{$gt:70}},{'alcalde.edad':1, _id:0, nombre:1})  -> Hacerla con mongoCompas Opciones

3. Seleccionar las ciudades donde el alcalde tenga 76 años o 41 años

{$or:[ {"alcalde.edad":76},{"alcalde.edad":41} ]}

db.getCollection('ciudades').find(
  {
    $or: [
      { 'alcalde.edad': 76 },
      { 'alcalde.edad': 41 }
    ]
  },
  { 'alcalde.edad': 1 }
);

## Consultas en Arrays

1.  Crear colección cazas
2.  Cargar los documentos de la colleción cazas, de la data cazas

### Ejemplos con consulta con Arrays

1. Buscar los docuemntos que tengan USA

{paises:'usa'}

2. Buscar los documentos donde cualquier elemento del array dimensiones sea mayor a 15
{dimensiones:{$gt:15}}

db.getCollection('cazas').find({
  dimensiones: { $gt: 15 }
});

3. Buscar los documentos donde las dimensiones del avion por ancho que es la posición 1, sea superior a 14

{'dimensiones.1':{$gt:14}}

db.getCollection('cazas').find({
  'dimensiones.1': { $gt: 14 }
});


### Arrays. Operador $all

1. Buscar los aviones que estan sirviendo en egipto y en israel

{paises:{$all:['egipto', 'israel']}}

db.getCollection('cazas').find({
  paises: { $all: ['egipto', 'israel'] }
});

Equivalente

{$and:[{paises:'españa'},{paises:'inglaterra'}]}

db.getCollection('cazas').find({
  $and: [
    { paises: 'españa' },
    { paises: 'inglaterra' }
  ]
});

### Arrays Operador $elementMatch -> Permite hacer busquedas dentro de arrays, va a buscar la lista de condiciones que estan dentro de este operador 

1. Buscar que aviones tienen uso dentro de inglaterra

{paises:{$elemMatch: {$eq:'inglaterra'}}}



2. Buscar los aviones cuyas dimensiones sean mayores que 20


{dimensiones:{$elemMatch: {$gt:20}}}


db.getCollection('cazas').find({
  dimensiones: { $elemMatch: { $gt: 20 } }
});

3. Buscar los aviones donde las dimensiones sean menores a 20 o mayores a 15

{dimensiones:{$elemMatch: {$lt:20,$gt:15}}}


### Buscar en arrays de documentos anidados

1. Utilizamos la coleccion ciudades

2. Buscar por consejeros de nombre Jeri Flower de edad 78

{consejeros:{identificador:1, nombre:'Jeri Flowers', edad:78}}

3. Buscar en consejeros edad sea mayor a 70

{'consejeros.edad':{$gt:70}}

4. Buscar en consejeros en la posicion o sea mayor a 70

{'consejeros.0.edad':{$gt:70}}


4. Buscar en consejeros en la posicion2 qe esten entre 70 y 80
{'consejeros.2.edad':{$gt:70, $lt:80}}