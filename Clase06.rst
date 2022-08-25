.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 06 - PGE 2022
===================
(Fecha: 25 de agosto)
	
Registro en video de algunos temas de la clase de hoy
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

`Copia de objetos - Constructor copia - Operador de asignación 2021 <https://youtu.be/zhq2QI-9IJQ>`_


Copia de objetos
================

- El operador = supone la asignación del operando de la derecha en el izquierdo
- Si ``p1`` y ``p2`` son objetos de la clase ``Persona``

.. code-block:: c
	
	p1 = p2;  // Esta asignación copia los valores de p2 en p1

- El operador = supone la asignación miembro a miembro
- En este caso, el compilador define automáticamente el ``operator=``
- Realiza la asignación miembro a miembro
- Por otro lado, cuando se crea un nuevo objeto

.. code-block:: c
	
	Persona p1 = p2; 

- No implica ninguna asignación sino que se invoca a un constructor especial (constructor copia).
- Es equivalente a:

.. code-block:: c
	
	Persona p1( p2 );


Constructor copia
=================

.. code-block:: c

	Persona( const Persona & persona );	

- "Copiar" un objeto supone la creación de otro objeto
- Siempre que se necesite crear un nuevo objeto se llama (o invoca) al constructor copia. 

:Se invoca al constructor copia en los siguientes momentos:
	- Cuando se pasa un objeto como argumento
	- Cuando algún método o función devuelve un objeto
	- Cuando se crea un objeto mediante asignación
	- Cuando se lanza una excepción

- El compilador crea un constructor copia oficial cuando no se define explícitamente
- Este constructor copia oficial realiza un clon exacto miembro a miembro, que no siempre el nuevo objeto es una réplica exacta del original (aunque pueda parecerlo).
- A veces es necesario definir un constructor copia explícito


Operador de asignación
======================

- El operador de asignación oficial es análogo al constructor copia oficial
- El compilador lo define si no hay una versión explícita
- La diferencia con el operador de asignación es que ambos objetos deben ser creados previamente

.. code-block:: c

	Persona & operator=( const Persona & persona );

- Hay inconvenientes con el constructor copia oficial cuando los miembros son punteros
- Igual sucede con el operador de asignación oficial	


Ejercicio 1
===========

- Crear una clase Persona simple.
- Sobrecargar en Persona el ``operator++`` de tal manera permita incrementar en uno su edad.
- Que puda funcionar algo así:

.. code-block:: c

	Persona carlos( 20, "Carlos" );
	carlos++;
	qDebug() << carlos.getEdad();  // Esto debe devolver 21


Ejercicio 2
===========

- Crear un QVector< QWidget > y agregar 3 objetos.
- Trabajar directamente en la función ``main``
- ¿Puede compilarlo?
- Tratar de solucionarlo.


Ejercicio 3
===========

- Crear una clase Ventana que herede de QWidget.
- Crear un QVector< Ventana > y agregar 3 objetos.
- Tener en cuenta que no son punteros, sino objetos concretos de QWidget.
- Elegir un par de características más para copiar.
- Usar un for para extraerlos y mostrarlos como ventanas independientes.



**Disponible en Drive el Mini Examen 3**