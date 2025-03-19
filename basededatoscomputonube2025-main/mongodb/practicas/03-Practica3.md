# Practica 3. Updates y Deletes

1. Cambiar el salario del empleado Imogene Nolan. Se le asigna 8000.

db.empleados.updateOne({ 
    $and:[
        {nombre:"Imogene"},
        {apellidos:"Nolan"}
    ]},
    {
        $set:{salario:8000}
    }
)

db.empleados.updateOne({nombre:"Imogene",apellidos:"Nolan"},{$set:{salario:8000}
})

2. Cambiar "Belgium" por "Bélgica" en los empleados (debe haber dos).

db.empleados.updateMany(
{pais:"Belgium"},
{$set:{pais:'Belgica'}}
)
3. Incrementar el salario de todos los empleados de Google en 1000.
db.empleados.updateMany(
    {empresa:{$eq:'Google'}},
    {$set:{salario:1000}}
)

4. Reemplazar el empleado Omar Gentry por el siguiente documento:

```json
{
"nombre": "Omar",
"apellidos": "Gentry",
"correo": "sin correo",
"direccion": "Sin calle",
"region": "Sin region",
"pais": "Sin pais",
"empresa": "Sin empresa",
"ventas": 0,
"salario": 0,
"departamentos": "Este empleado ha sido anulado"
}```

db.empleados.replaceOne(
    {nombre:'Omar', apellidos:"Gentry"},
  {
    "nombre": "Omar",
"apellidos": "Gentry",
"correo": "sin correo",
"direccion": "Sin calle",
"region": "Sin region",
"pais": "Sin pais",
"empresa": "Sin empresa",
"ventas": 0,
"salario": 0,
"departamentos": "Este empleado ha sido anulado"
}
)
5. Con un find comprobar que el empleado ha sido modificado

db.empleados.find({nombre:'Omar', apellidos:'Gentry'})

6. Borrar todos los empleados que ganen mas de 8500. Nota: deben ser borrados 3 documentos
db.empleados.deleteMany({salario:{$gt:8500}})
7. Visualizar con una expresión regular todos los empleados con apellidos que comiencen con "R"
8. Buscar todas las regiones que contenga un "V". Hacerlo con el operador $regex y que no distinga mayúsculas y minúsculas. Deben salir 2.
9. Visualizar los apellidos de los empleados ordenados por el propio apellido.
10. Indicar el número de empleados que trabajan en Google.
11. Borrar la colección empleados y la base de datos


