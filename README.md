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
