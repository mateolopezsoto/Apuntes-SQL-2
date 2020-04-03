# Apuntes-SQL-2
DDL 
	DDL es un lenguaje (DATA DEFINITION LANGUAGE) es un tipo de lenguaje que permite a los usuarios de las bases de datos implementar las tareas de definición de las estructuras que almacenarán los datos y sus procedimientos
	
	CREATE DOMAIN --> Sirve para crear dominios diferentes a los ya existentes.
		CREATE DOMAIN Nombre_del_dominio Tipo_de_Dato;
		
	CREATE TABLE --> Como su propio nombre indica, se utiliza para crear tablas
		CREATE TABLE Nombre_de_la_tabla (
			Nombre_columna_1 Tipo_de_Dato [DEFAULT valor]
				[restricciones]
			);
			
	A la hora de crear tabla, a continuación de escribir CREATE TABLE podremos colocar IF NOT EXISTS, que verificará que la tabla no existe. Una vez hecho esto creará la tabla. Por lo tanto, si una tabla con el mismo nombre ya está en la base de datos, no la creará
	
	DROP TABLE --> Sirve para eliminar tablas. Podemos eliminar 1 o varias a la vez
	
		DROP TABLE Nombre_de_la_tabla;
		DROP TABLE tabla_1, tabla_2;
		
	Despues de nombrar la tabla o tablas que queremos borrar, tenemos la opción de añadir CASCADE o RESTRICT
	
CASCADE: Eliminará cualquier dato asociado a la tabla que se elimine
RESTRICT: No permite eliminar la tabla si tiene algún dato asociado

	ALTER TABLE --> Se usa para modificar las diferentes tablas y tiene diferentes usos.
	
		1. Para renombrar la tabla, de la forma ALTER TABLE Tabla RENAME TO Otro_nombre;
		2. Para añadir columnas, de la forma ALTER TABLE Tabla ADD COLUMN (Nombre_de_la_columna Tipo_de_Dato);
		3. Para borrar columnas, de la forma ALTER TABLE Tabla DROP COLUMN (Nombre_de_la_columna) [CASCADE | RESTRICT];
		4. Para añadir distracciones, de la forma ALTER TABLE Tabla ADD <restricción>;
		5. Para borrar distracciones, de la forma ALTER TABLE Tabla DROP <restricción>;
		

RESTRICCIONES: Existen diferentes tipo de restricciones. Los más comunes son los siguientes: 

	- NOT NULL --- Para indicar que no queremos que tenga elementos nulos. También se suele utilizar al lado de otras restricciones que sí puedan tener elementos nulos, para diferenciarlas entre si.
	- UNIQUE --- Sirve para que todos los elementos de una columna sean diferentes
	- CHECK --- Se utiliza para establecer condiciones que las columnas deben cumplir
	- PRIMARY KEY --- Su uso es identificar la clave primaria de cada tabla
	- FOREIGN KEY --- Señala la clave primaria de otra tabla para así asegurar su integridad referencial
	- MATCH --- Se usa para especificar el grado de coincidencia que pueden tener la clave referenciada con las claves externas. Puede ser MATCH FULL (no permite que una clave externa sea NULL a no ser que lo sean todas), MATCH SIMPLE (permite que alguna columna de las claves externas sean NULL aunque otras no lo sean. Es el valor por defecto) o MATCH PARTIAL (es válida si una de las columnas de la clave externa es NULL y las demás columnas coincidan con los datos de la tabla referenciada y son NOT NULL o si todas las columnas son NOT NULL y coinciden con la tabla referenciada)
	- ON DELETE / ON UPDATE --- Sirve para indicar que pasará cuando se borren o se modifiquen las tablas respectivamente. Hay 4 acciones diferentes permitidas.
		i. CASCADE = Todo lo que esté asociado al dato borrado o modificado se eliminará o modificará también
		ii. NO ACTION = El valor por defecto. No permite eliminar o actualizar el dato si algún objeto depende de él
		iii. SET NULL = Se permite el borrado o modificado y las columnas de la clave externa se establecen como NULL
		iiii. SET DEFAULT = Parecido a SET NULL, con la única diferencia de que las columnas de la clave externa se establecen con su valor predeterminado. Si no tienen este valor saldrá un error
	
* Cabe destacar que podemos utilizar CONSTRAINT para todas estas restricciones, aunque su uso no sea obligatorio. El único caso cuyo empleo es obligatorio es si tenemos más de una clave primaria
** Las restricciones se pueden colocar tanto en la creación de la tabla como realizando un ALTER TABLE
  
  Restricción de comprobación: Se aplican a cada fila que se inserta, se modifica o se borra
    --Solo se permite si el Check devuelve True--
    Valor predeterminado = [NOT DEFERRABLE] y [INITIALLY INMEDIATE]
    Si el valor es DEFERRABLE, la comprobación de la restricción se puede realizar al final de la transacción
 [CONSTRAINT <nombre-de-la-restriccion>]
    CHECK predicado (atributos)
  [[NOT] DEFERRABLE]
  [INITIALLY INMEDIATE | DEFERRABLE]
  (DEFERRABLE + INITIALLY INMEDIATE = NOT DEFERRABLE)
  Para posponer la comprobación sería DEFERRABLE + INITIALLY DEFERRABLE
  
  **Para borrar una tabla**
  
  DROP SCHEMA
  [IF EXISTS] <nombre-de-la-BD>
  
  DROP TABLE
  [IF EXISTS] <nombre-de-la-tabla>
  [CASCADE | RESTRICT]
  
  CASCADE --> Automáticamente borra todos los objetos que dependen de la tabla
  RESTRICT --> No permite borrar la tabla si algún objeto depende de ella. Es el valor predeterminado
  
  
      ALTER TABLE
      
 ALTER TABLE <nombre-de-la-tabla> ADD [COLUMN] <atributo> <dominio>
                                  DROP [COLUMN] <atributo> [CASCADE | RESTRICT]
                                  ADD <restricción>
                                  DROP <restricción>
    COLUMNA      |   RESTRICCIÓNS
  ADD      DROP    ADD        DROP


DML --> DATA MANIPULATION LANGUAGE

La principal diferencia entre DML y DDL es que DDL define estructuras de datos, mientras que DML permite introducir datos para realizar consultas o modificar los datos

DML: Consta de INSERT, DELETE y UPDATE. Antes tenía SELECT, pero actualmente se ha eliminado.

INSERT: Sirve para añadir datos. Su sintaxis es la siguiente: 

INSERT INTO <nombre-de-la-tabla>
	[(<atributo1>, <atributo2>,...)] 
	(VALUES (<valor1>, <valor2>,...)
	
En lugar de VALUES podemos utilizar SELECT cumpliendo con 2 condiciones: 
	- Mismo número de columnas
	- Mismos dominios
	
**Tipos de datos**
	DATETIME
	NCHAR
	INTEGER
	
EJEMPLO*** --> (1, 'cheese', 9.99), 
	       (2, 'bread', 1.99),
	       (3, 'milk', 2.99)
	   
UPDATE: Sirve para modificar datos. Su sintaxis es la siguiente:

UPDATE <nombre-de-la-tabla>
SET <atributo1> = <valor1>,
    <atributo2> = <valor2>,
[WHERE <predicado>];
	    
**EJEMPLO**
UPDATE world 
SET name='España',
continent='Africa'
WHERE name='Spain';

El WHERE es optativo, pero en este ejemplo lo necesitamos ya que de no tenerlo todos los names de la base de datos se conviertan en 'Spain' y todos los continentes en 'Africa'

DELETE: Sirve para borrar datos. Su sintaxis es la siguiente:

DELETE FROM <nombre-de-la-tabla>
[WHERE <predicado>]
	
**EJEMPLO**
DELETE from world
WHERE population > 100000000
--Eliminaría cualquier país que tuviera más de 100 millones de habitantes--


**TIPOS DE DATOS**
	[Numéricos]: INTEGER, DECIMAL(preciso), REAL(no es preciso)
	[Texto]: CHAR(longitud fija), VARCHAR(longitud variable), TEXT(longitud ilimitada variable)
	[Tiempo]: DATE(dia, mes, año), TIME(hora, minuto, segundo [zona horaria]), TIMESTAMP(DATE + TIME)
	[Otros]: BOOLEAN(true, false o null), MONEY, JSON, INET, CIDR, UUID
