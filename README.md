# Apuntes-SQL-2
dept (id, name)
teacher (id, name, dept, IBAN)

[ON DELETE <nombre-de-la-restriccion>]
	NO ACTION | CASCADE | SET NULL | SET DEFAULT
	
	Las restricciones de integridad referencial (definición)
	
		Cascade- Borra los registros en cascada, es decir, si tenemos dos tablas, la tabla que tiene la clave principal se irá borrando, por lo que la tabla secundaria lo hará también
		No action- Sería su accionamiento predetermindado, no haría nada 
		Set null- Establecería los valores de la clave secundaria en Null. Es la solución más fácil, pero provocaría un fallo en el rendimiento
		Set default- Establece los datos por defecto en el departamento 0
	
    [MATCH FULL/PARTIAL]
		Match Full nos indica que tienes el valor completo para ir a las tablas padre o tienes su ausencia, o lo que es lo mismo, solo vale si tienes todos los valores o si todos son nulls
		Match Partial funciona aunque tenga un valor null si tiene otro valor capaz de referenciar a las tablas padre
	
  Restricción de unicidad:   
[CONSTRAINT <nombre-de-la-restriccion>]
  UNIQUE(<atributos>
  
  UNIQUE --> PRIMARY KEY
  
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
