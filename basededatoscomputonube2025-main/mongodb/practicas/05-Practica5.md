# Práctica 5: Agregaciones
1. Cuenta los productos de tipo “medio”, usando un método básico
```json
{
  "tipo": "medio"
}
```
2. Indicar con un distinct, las empresas (fabricantes) que hay en la colección
```json
db.productos.distinct("fabricante")
```

3. Usando aggregate, visualizar los productos que tengan más de 80 unidades
```json
db.productos.aggregate([
  { "$match": { "unidades": { "$gt": 80 } } }
])
```

4. Con $project visualizar solo el nombre, unidades y precio de los productos que tengan menos de 10 unidades
```json
db.productos.aggregate([
  { "$match": { "unidades": { "$lt": 10 } } },
  { "$project": { "nombre": 1, "unidades": 1, "precio": 1 } }
])
```

5. Con $project ponemos el fabricante pero le cambiamos el nombre por “empresa”. Usamos el mismo comando anterior
```json
db.productos.aggregate([
  { "$match": { "unidades": { "$lt": 10 } } },
  { "$project": { "nombre": 1, "unidades": 1, "precio": 1, "empresa": "$fabricante" } }
])
```

6. Añadir a la consulta anterior un campo calculado que se llame total y que multiplique precio por unidades.
```json
db.productos.aggregate([
  { "$match": { "unidades": { "$lt": 10 } } },
  { "$project": { 
      "nombre": 1, 
      "unidades": 1, 
      "precio": 1, 
      "empresa": "$fabricante", 
      "total": { "$multiply": ["$precio", "$unidades"] } 
    } 
  }
])
```

7. Hacer que el nombre salga en mayúsculas con el operador $toUpper
```json
db.productos.aggregate([
  { "$match": { "unidades": { "$lt": 10 } } },
  { "$project": { 
      "nombre": { "$toUpper": "$nombre" }, 
      "unidades": 1, 
      "precio": 1, 
      "empresa": "$fabricante", 
      "total": { "$multiply": ["$precio", "$unidades"] } 
    } 
  }
])
```

8. Añadir un campo calculado que ponga el nombre del producto y el tipo concatenado con el operador $concat. Le llamamos al campo “completo”
```json
db.productos.aggregate([
  { "$match": { "unidades": { "$lt": 10 } } },
  { "$project": { 
      "nombre": { "$toUpper": "$nombre" }, 
      "unidades": 1, 
      "precio": 1, 
      "empresa": "$fabricante", 
      "total": { "$multiply": ["$precio", "$unidades"] }, 
      "completo": { "$concat": ["$nombre", " - ", "$tipo"] } 
    } 
  }
])
```

9. Ordena el resultado por el campo “total”
```json
db.productos.aggregate([
  { "$match": { "unidades": { "$lt": 10 } } },
  { "$project": { 
      "nombre": { "$toUpper": "$nombre" }, 
      "unidades": 1, 
      "precio": 1, 
      "empresa": "$fabricante", 
      "total": { "$multiply": ["$precio", "$unidades"] }, 
      "completo": { "$concat": ["$nombre", " - ", "$tipo"] } 
    } 
  },
  { "$sort": { "total": 1 } }
])
```
10. Haciendo una nueva consulta, averiguar el numero de productos por tipo de producto
```json
db.productos.aggregate([
  { "$group": { 
      "_id": "$tipo", 
      "count": { "$sum": 1 } 
    } 
  }
])
```

11. Añadir el valor mayor y el menor
```json
db.productos.aggregate([
  { "$group": { 
      "_id": "$tipo", 
      "count": { "$sum": 1 },
      "max_value": { "$max": "$precio" },
      "min_value": { "$min": "$precio" }
    } 
  }
])
```

12. Añade el total de unidades por cada tipo
```json
db.productos.aggregate([
  { "$group": { 
      "_id": "$tipo", 
      "count": { "$sum": 1 },
      "total_unidades": { "$sum": "$unidades" }
    } 
  }
])
```

13. Con el operador $set y el operador $substr visualiza todos los datos del producto "Small Metal Tuna" y los primeros 5 caracteres del nombre.
```json
db.productos.aggregate([
  { "$match": { "nombre": "Small Metal Tuna" } },
  { "$set": { "nombre_corto": { "$substr": ["$nombre", 0, 5] } } }
])
```

14. Creamos una salida que tenga el nombre del artículo y el total (precio por unidades) y lo guardamos en una colección denominada productos2
```json
db.productos.aggregate([
  { "$project": { 
      "nombre": 1, 
      "total": { "$multiply": ["$precio", "$unidades"] }
    } 
  },
  { "$out": "productos2" }
])
```

15. Comprobamos que se ha creado
```json
db.productos2.find()
```

16. Hacemos un find para comprobar el resultado
```json
db.productos2.find()
```

17. Usando $cond y $project vamos a visualizar el nombre del producto, el precio y un campo llamado valoración que ponga “barato” si el precio es menor de 250 y caro si es mayor o igual
```json
db.productos.aggregate([
  { "$project": { 
      "nombre": 1, 
      "precio": 1, 
      "valoracion": { 
        "$cond": { 
          "if": { "$lt": ["$precio", 250] }, 
          "then": "barato", 
          "else": "caro" 
        }
      } 
    } 
  }
])
```
```