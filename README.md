# outer-joins-ministore



1\. ¿Por qué usaste LEFT JOIN para la Consulta 1 y no INNER JOIN? ¿Qué se perdería si usaras INNER JOIN?



La pregunta de negocio es qué productos del catálogo nunca se vendieron. Para eso necesito que la consulta conserve todas las filas de productos, incluso las que no tienen ninguna venta asociada. El LEFT JOIN devuelve todas las filas de la tabla de la izquierda (productos) y completa con NULL las columnas de ventas cuando no hay coincidencia. Con un INNER JOIN solo me devuelve las filas que tienen par en ambas tablas. 



2\. ¿Por qué usaste RIGHT JOIN para la Consulta 2? ¿Qué tabla está a la izquierda y cuál a la derecha en tu consulta?



Necesito conservar todas las filas de ventas y ver cuáles no encuentran producto. En mi consulta, productos está a la izquierda (es la tabla del FROM) y ventas está a la derecha (es la tabla que va después del RIGHT JOIN). Como el RIGHT JOIN conserva todas las filas de la tabla de la derecha, se mantienen las 10 ventas, y la venta 10 aparece con NULL en las columnas de productos.



3\. ¿Qué representan los valores NULL en cada resultado? Explicá con un ejemplo concreto de los datos qué significa que venta\_id sea NULL en la Consulta 1 y que producto\_id de productos sea NULL en la Consulta 2.



Los NULL no son un error de la consulta: son la señal que estoy buscando.



Consulta 1: que venta\_id sea NULL en la fila del producto 108 (Hub USB-C 7p) significa que no existe ninguna fila en ventas con producto\_id = 108. Es decir, ese producto está en el catálogo pero nunca se vendió. Lo mismo vale para el 109 (Parlante Bluetooth).

Consulta 2: que producto\_id de productos sea NULL en la fila de la venta 10 significa que el producto\_id = 999 que figura en la venta no existe en el catálogo. Como una venta no debería poder referirse a un producto inexistente, esto indica un posible error de carga de datos (un ID mal tipeado o un producto que se borró del catálogo).



Por eso el filtro WHERE ... IS NULL es lo que aísla los casos de interés. Sin él, las consultas devolverían todas las filas, incluidas las coincidencias normales.



4\. ¿Cuándo usarías FULL OUTER JOIN en un caso real de negocio?



Lo usaría cuando necesito comparar dos fuentes que deberían coincidir y quiero ver las diferencias en ambos sentidos a la vez. Un ejemplo típico es la conciliación entre el sistema de facturación y el de inventario o entre lo que registra la tienda y lo que informa el proveedor: puede haber facturas sin movimiento de stock y movimientos de stock sin factura, y un FULL OUTER JOIN me muestra ambos casos en una sola consulta.



Otro caso es auditar una migración de datos: al comparar la tabla de origen con la de destino, las filas con NULL de un lado son registros que no se migraron, y las filas con NULL del otro lado son registros que aparecieron de más.



