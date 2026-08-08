# outer-joins-ministore
--README--

--¿Por qué usé LEFT JOIN en la Consulta 1 y no INNER JOIN?--

--La pregunta de negocio era: “¿Qué productos del catálogo nunca fueron vendidos?”

--Para responderla, necesitaba ver todos los productos, incluso aquellos que no tienen ventas asociadas.  
--El `LEFT JOIN` garantiza exactamente eso:  
--conserva todas las filas de la tabla de la izquierda(`productos`)  
--completa con datos de la tabla de la derecha (`ventas`) cuando hay coincidencia  
--deja NULL cuando no existe ninguna venta

--Si hubiera usado `INNER JOIN`, solo aparecerían los productos que sí tienen ventas.  
--Los productos 108 (Hub USB-C) y 109 (Parlante Bluetooth) desaparecerían del resultado, porque nunca fueron vendidos.  
--Por eso el INNER JOIN no sirve para esta pregunta.


--¿Por qué usé RIGHT JOIN en la Consulta 2?--

--La pregunta de negocio era:  
--“¿Existen ventas registradas con productos que no figuran en nuestro catálogo?”

--Para responderla, necesitaba ver todas las ventas, incluso aquellas cuyo `producto_id` no existe en la tabla `productos`.

--En un `RIGHT JOIN`:
--la tabla de la derecha (`ventas`) se conserva completa
--la tabla de la izquierda (`productos`) aporta información solo cuando hay coincidencia
--cuando no hay coincidencia, las columnas de productos quedan en NULL
--Esto permite detectar la venta con `producto_id = 999`, que es un registro huérfano.

--Si hubiera usado `LEFT JOIN`, la tabla preservada sería `productos`, y perdería las ventas sin coincidencia.  
--Por eso el RIGHT JOIN es el correcto.


--¿Qué representan los valores NULL en cada resultado?--

--Los valores NULL no son errores, sino información clave para la auditoría de datos.

--Ejemplo en la Consulta 1 (LEFT JOIN)
--Cuando aparece:producto_id (de productos) = NULL

--significa:La venta hace referencia a un producto que no existe en el catálogo.**

--Ejemplo real del dataset:  
--Venta 10 tiene `producto_id = 999`, que no existe en `productos`.  
--Por eso las columnas de productos aparecen en NULL.


--¿Cuándo usaría FULL OUTER JOIN en un caso real de negocio?--

--El `FULL OUTER JOIN` es ideal cuando se necesita una vista completa de auditoría, sin perder ninguna fila de ninguna tabla.
--Casos reales:

--Detectar productos sin ventas (catálogo incompleto o stock inmovilizado)
--Detectar ventas sin producto (errores de carga, IDs mal ingresados)
--Comparar dos sistemas distintos (ERP vs CRM) para ver:
  --registros que coinciden  
  --registros que existen solo en un sistema  
  --registros faltantes o inconsistentes

--En MiniStore, el FULL OUTER JOIN permite ver todo el catálogo y todas las ventas, mostrando claramente:
--productos sin ventas (NULL en ventas)
--ventas sin producto (NULL en productos)

--Es la vista más completa para validar la calidad de los datos antes de construir reportes.

--Conclusión--
--Este ejercicio demuestra cómo los JOINs externos permiten detectar:
--datos faltantes  
--registros huérfanos  
--inconsistencias  
--productos sin movimiento  
--ventas con errores  
--Son herramientas esenciales para cualquier analista que audita datos antes de construir dashboards o modelos de negocio.



