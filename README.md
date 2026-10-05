# P2_ADBD
## P2 - Modelo entidad/relación. Viveros
### 1. Imagen del modelo entidad/relación del escenario descrito

### 2. Descripción de cada una de las entidades definidas.
- **Vivero** - Representa cada establecimiento de la empresa.
- **Zona** - Representa una zona específica dentro de un vivero.
- **Producto** - Representa un producto de la empresa
- **Stock** - Es una entidad asociativa entre Zona y Producto, para indicar cuanta cantidad de ese producto hay disponible en una zona concreta.

- **Empleado** - Un trabajador de la empresa.
- **Puesto** - Tipo de puesto que puede desempeñar un empleado.
- **Destino** - Histórico de donde trabaja un empleado y con que puesto.

- **ClientePlus** - Representa un cliente del programa de fidelizacioón
- **Venta** - Representa una compra de un cliente de la fidelización Tajinaste Plus.
- **Bonificación** - Representa la bonificación asignada a un cliente Plus para un mes.

### 3. Descripción y ejemplos ilustrativos del dominio de cada uno de los atributos de las entidades y de las relaciones.
#### VIVERO
- id_vivero → Identificador entero positivo **PRIMARY KEY**
- nombre → Texto
- latitud → Número decimal 
- longitud → Número decimal 
#### ZONA
- id_zona → Identificador entero positivo **PRIMARY KEY**
- nombre → Texto
- tipo → Texto
- latitud → Número decimal 
- longitud → Número decimal
- id_vivero → Vivero al que pertenece la zona **FOREIGN KEY**
#### PRODUCTO
- id_producto → Identificador entero positivo **PRIMARY KEY**
- nombre → Texto
- precio → Número decimal no negativo con dos decimales
#### STOCK
- id_zona → Zona donde está el stock **FOREIGN KEY**
- id_producto → Producto almacenado **FOREIGN KEY**
- cantidad → Número entero positivo
#### EMPLEADO
- id_empleado → Identificador entero positivo **PRIMARY KEY**
- nombre → Texto
- apellidos → Texto
- fecha_entrada → Fecha de incorporación
#### PUESTO
- id_puesto → Identificador entero positivo **PRIMARY KEY**
- nombre → Texto
#### DESTINO
- id_destino → Identificador entero positivo **PRIMARY KEY**
- fecha_inicio → Fecha de incorporación
- fecha_fin → Fecha de finalizacion
- id_empleado → Empleado destinado **FOREIGN KEY**
- id_zona → Zona de destino **FOREIGN KEY**
- id_puesto → Puesto que se va a desempeñar **FOREIGN KEY**
#### CLIENTEPLUS
- id_cliente → Identificador entero positivo **PRIMARY KEY**
- nombre → Texto
- fecha_ingreso → Fecha de incorporación
- fecha_baja → Fecha de finalización
#### VENTA
- id_venta → Identificador entero positivo **PRIMARY KEY**
- fecha → Fecha en la que se realiza la venta
- precio → Número decimal no negativo con dos decimales
- id_cliente → Cliente Plus que realiza la venta **FOREIGN KEY**
- id_empleado → Empleado que tramita la venta **FOREIGN KEY**
#### BONIFICACIÓN
- id_bonificacion → Identificador entero positivo **PRIMARY KEY**
- mes → Texto
- volumen_compras → Número entero positivo que ha realizado el cliente
- impporte_bonificación → Número decimal no negativo con dos decimales
- id_cliente → Cliente Plus que tiene la bonificación **FOREIGN KEY**

### 4 . Descripción de cada una de las relaciones definidas. Describa con detalle la cardinalidad de cada relación.
VIVERO — ZONA
- Un VIVERO tiene 1-N ZONAS.
- Una ZONA pertenece a 1 y solo 1 VIVERO.
- Ejemplo: el vivero Vivero La Orotava puede tener las zonas Exterior, Almacén e Invernadero 1.

ZONA — PRODUCTO mediante STOCK
- Una ZONA puede contener 0-N productos.
- Un PRODUCTO puede estar asignado a 0-N zonas.
- STOCK registra la cantidad disponible para cada pareja zona-producto.
- Ejemplo: en Almacén puede haber 40 unidades de Ficus, mientras que en Exterior puede haber 15.

EMPLEADO — DESTINO
- Un EMPLEADO puede tener 0-N destinos históricos.
- Cada DESTINO pertenece a 1 empleado.
- Los destinos tienen fechas para reconstruir el histórico.

ZONA — DESTINO
- Una ZONA puede tener 0-N destinos a lo largo del tiempo.
- Cada DESTINO se realiza en 1 zona.
- Como una zona pertenece a un vivero, el destino determina indirectamente el vivero.

PUESTO — DESTINO
- Un PUESTO puede aparecer en 0-N destinos.
- Cada DESTINO tiene 1 puesto.
- Ejemplo: Vendedor puede ser desempeñado por muchos empleados y en distintos periodos.

CLIENTEPLUS — VENTA
- Un cliente puede realizar 0-N ventas.
- Cada pedido pertenece a 1 y solo 1 cliente.

EMPLEADO — VENTA
- Un empleado puede gestionar 0-N ventas.
- Cada pedido tiene exactamente 1 empleado responsable.
- No se permite que un pedido tenga dos responsables.

CLIENTEPLUS — BONIFICACION
- Un miembro Plus puede tener 0-N bonificaciones mensuales.
- Cada bonificación pertenece a 1 miembro Plus.
- Debe existir como máximo una bonificación para cada combinación (cliente, año, mes).

### 5 . Restricciones semánticas propuestas.
- No solapamiento de destinos: un empleado no puede tener dos destinos cuyos intervalos de fechas se solapen, nunca tiene dos destinos simultáneamente.

- Destino siempre en una zona: todo destino debe indicar exactamente una zona. No se permite asignar un empleado solamente al vivero sin especificar zona.

- Coherencia vivero-zona: una zona pertenece a un único vivero. Por ello no es necesario guardar también id_vivero en DESTINO; se obtiene a través de ZONA.

- Fechas de destino válidas: fecha_inicio < fecha_fin cuando exista fecha_fin. Una fecha de fin NULL representa un destino activo.

- Stock no negativo: cantidad_disponible >= 0.

- No puede existir más de un registro para la misma pareja (zona, producto).

- Venta con responsable único: id_empleado de VENTA es obligatorio y cada pedido tiene exactamente un responsable.

- Bonificación mensual única: un cliente Plus no puede tener dos registros de bonificación para el mismo año y mes. Restricción de unicidad: (id_cliente, anio, mes).

- Coherencia de la bonificación: volumen_compra debería calcularse a partir de los pedidos del cliente durante ese mes, y importe_bonificacion según las reglas vigentes del programa.
