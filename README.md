# UNIDAD-3-BASE-DE-DATOS-EJERCICIO-20
Este diagrama entidad-relación (modelo relacional) representa la estructura lógica de un **sistema de gestión para un espacio de trabajo compartido (*coworking*)**, diseñado para administrar a los miembros, el control de accesos mediante tarjetas RFID, las áreas o salas de trabajo, las reservas y los consumos adicionales realizados.

A continuación se detalla qué representa cada tabla y cómo se relacionan entre sí:

### 1. Entidades Principales (Tablas)

* **`MIEMBRO`**: Almacena la información de los usuarios registrados en el coworking.
* *Campos:* `DNI` (Clave Primaria), `NOMBRE`, `EMPRESA`, `CORREO` y `NUM_IDENTIFICADOR_TARJETA`.


* **`CONTROL_ACCESO`**: Registra los fichajes o entradas de los usuarios a las instalaciones.
* *Campos:* `ID_ACCESO` (Clave Primaria), `FECHA_FICHAJE`, `HORA_FICHAJE`, `TARJETA_RFID` y `DNI_MIEMBRO_VALIDADO` (Clave Foránea).


* **`ESPACIO_TRABAJO`**: Contiene el inventario de salas, despachos o puestos disponibles para alquilar.
* *Campos:* `ID_ESPACIO` (Clave Primaria), `NOMBRE`, `CAPACIDAD_MAXIMA`, `TARIFA_HORA` y `TIPO_ESPACIO`.


* **`RESERVA`**: Gestiona las solicitudes de uso de los espacios por parte de los miembros.
* *Campos:* `ID_RESERVA` (Clave Primaria), `FECHA`, `HORA_INICIO`, `HORA_FIN`, `ESTADO`, además de las claves foráneas `DNI` (miembro que reserva) e `ID_ESAPCIO` (espacio solicitado).


* **`CONSUMO_ADICIONAL`**: Registra gastos extras incurridos por los usuarios (como servicios de cafetería, impresiones o suministros).
* *Campos:* `ID_CONSUMO` (Clave Primaria), `FECHA`, `HORA`, `DESCRIPCION`, `CANTIDAD`, `COSTO` y `DNI` (Clave Foránea del miembro que realiza el consumo).





---

### 2. Relaciones Principales

* **Control de Accesos:** Cada miembro cuenta con un identificador de tarjeta que permite validar sus entradas a través de la tabla de control de acceso (relación de 1 a N).


* **Reservas de Espacios:** Un miembro puede realizar múltiples reservas de diferentes espacios de trabajo a lo largo del tiempo (relación de 1 a N entre miembro/espacio y la tabla de reservas).


* **Consumos Extraordinarios:** Los miembros pueden acumular cargos por consumos adicionales independientes de sus reservas de espacio, los cuales quedan asociados a su perfil mediante su DNI.
<img width="1273" height="832" alt="EJERCICIO 20-MR drawio" src="https://github.com/user-attachments/assets/dd52f822-fd45-4d8f-b57e-eccf3d1b4968" />

Este diagrama muestra el **proceso completo de normalización de bases de datos** aplicado a un sistema de gestión para un espacio de trabajo compartido (*coworking*), transformando una estructura inicial desnormalizada (Forma Normal 0) hasta llegar al diseño relacional optimizado en Tercera Forma Normal (3FN).

A continuación se detalla cada una de las fases representadas:

### 1. Fase Normal 0 (FN0) — Tabla No Normalizada

* Se parte de una **única tabla masiva y plana** que concentra toda la información mezclada en una sola estructura: datos personales de los miembros (DNI, nombre, empresa, correo, número de tarjeta) y múltiples grupos repetitivos con colecciones de datos anidados dentro de las celdas (como la lista de accesos/fichajes RFID, la lista de reservas de espacios y la lista de consumos adicionales).
* **Problema:** Contiene grupos multivaluados y datos anidados en las celdas, lo que provoca una gran redundancia de información (repitiendo los datos del miembro por cada movimiento) y dificulta cualquier operación de consulta, inserción o actualización.



---

### 2. Primera Forma Normal (1FN)

* Se eliminan los grupos repetitivos y se asegura que **todos los atributos sean atómicos** (cada celda contiene un único valor indivisible), expandiendo los registros para que cada fila represente una combinación individual libre de valores anidados.
* Se obtiene una tabla tabular plana (`TABLA PLANTILLA`) donde se definen claves primarias compuestas (`DNI`, `ID_Acceso`, `ID_Reserva`, `ID_Consumo`), aunque todavía persiste una alta duplicidad en los datos estáticos de los miembros y los espacios de trabajo.



---

### 3. Segunda Forma Normal (2FN)

* Se parte de la 1FN y se eliminan las **dependencias parciales**. Se separan los atributos basándose en sus claves principales correspondientes, desglosando la información en cinco tablas independientes y normalizadas:
* **`TABLA MIEMBROS`**: Almacena los perfiles de los usuarios y el identificador de su tarjeta.
* **`TABLA CONTROL_ACCESOS`**: Registra los fichajes de entrada asociados al miembro correspondiente.
* **`TABLA ESPACIO_TRABAJO`**: Contiene el inventario de salas o puestos disponibles, sus capacidades y tarifas por hora.
* **`TABLA RESERVA`**: Gestiona las solicitudes de uso vinculando fechas, horarios, miembros y espacios.
* **`TABLA CONSUMO_ADICIONAL`**: Registra los gastos extras (cafetería, impresiones, etc.) asociados a cada usuario.





---

### 4. Tercera Forma Normal (3FN)

* Se eliminan las **dependencias transitivas**, garantizando que ningún atributo no clave dependa de otro atributo no clave. El diseño se consolida en un modelo relacional robusto y totalmente normalizado (`TABLA MIEMBRO`, `TABLA CONTROL_ACCESO`, `TABLA ESPACIO_TRABAJO`, `TABLA RESERVA` y `TABLA CONSUMO_ADICIONAL`), interconectado mediante claves foráneas (FK) que aseguran la integridad referencial y evitan anomalías para la administración del coworking.
<img width="4272" height="2524" alt="EJERICIO 20-FASES DE NORMALIZACION drawio" src="https://github.com/user-attachments/assets/d261df73-618b-437e-a2e7-ef4250649003" />



