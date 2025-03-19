#  Práctica 4: Arrays y Documentos Anidados en MongoDB

1. Buscar las personas que vivan en Colombia
 db.personas.find({ "direccion.pais": "Colombia" })


2. Personas a las que les guste el color rosa 
 db.personas.find({ "colores": "pink" })


3. Personas cuyo tercer color preferido sea el amarillo
 db.personas.find({ "colores.2": "yellow" }).limit(3)


4. Personas cuyo tercer color preferido sea el amarillo y que vivan en Mauritania
 db.personas.find({ "colores.2": "yellow", "direccion.pais": "Mauritania" })


5. Personas que les gusta el plata  y el salmón (salmon) usando $all
 db.personas.find({ "colores": { $all: ["silver", "salmon"] } })


6. Personas que les guste el rosa  o el rojo  usando $elemMatch
 db.personas.find({ "colores": { $elemMatch: { $in: ["pink", "red"] } } })
