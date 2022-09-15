.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 05 - PGE 2022
===================
(Fecha: 23 de agosto)

Registro en video de algunos temas de la clase de hoy
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

`Variantes de herencia con clases genéricas y Ejemplos con Mundos y Personas 2021 <https://youtu.be/rttDmneFJ3s>`_

`Clases genéricas con argumento por defecto 2021 <https://youtu.be/Gc_F-GfuI4E>`_




Para analizar
=============

**¿Qué pasa en memoria cuando se agrega un nuevo elemento a un vector?**


**Tener presente qué sucede cuando se crea un Listado de algún tipo de puntero. ¿Qué sucede en memoria?**




Diversas maneras de usar la Herencia con clases genéricas
=========================================================

.. code-block::

    template< typename T > class ListaGenerica : public Listado< T >  {
 
        //////////

    };

.. code-block::

    class ListaDeEnteros : public Listado< int >  {
 
        //////////

    };

.. code-block::

    template< class T > class ListadoStd : public std::vector< T >  {
 
        //////////

    };

.. code-block::

    template< class T > class ListadoQt : public QVector< T >  {
 
        //////////

    };

.. code-block::

    class ListaDePersonas : public QVector< Persona >  {
 
        //////////

    };

.. code-block::

    template< class T > class DerivadaDeCualquiera : public T  {
 
        //////////

    };


Ejemplo 1
=========

- Es posible también que una clase derive de una u otra según se requiera.

.. code-block::

	#include <QString>
	#include <QDebug>
	#include <typeinfo>

	class Real  {
	private:
    	    int colores;

	public:
    	    Real( int colores ) : colores( colores )  {  }
     	    int getDato()  {  return colores;  }
	};


	class Virtual {
	private:
    	    int bits;

	public:
    	    Virtual( int bits ) : bits( bits )  {  }
    	    int getDato()  {  return bits;  }
	};

	template< class T > class Mundo : public T  {
	private:
    	    QString nombre;

	public:
    	    Mundo( QString nombre, int dato ) : T( dato ), nombre( nombre )  {  }

    	    QString toString()  {
        	QString descripcion = "El mundo " + nombre + " es de ";
        	descripcion.append( QString::number( T::getDato() ) );

        	if ( typeid( T ) == typeid( Real ) )
            	    descripcion.append( " colores." );
        	if ( typeid( T ) == typeid( Virtual ) )
            	    descripcion.append( " bits." );

        	return descripcion;
    	    }
    	};

	int main( int, char ** )  {
    	    Mundo< Real > mundo1( "Tierra", 10000 );
    	    Mundo< Virtual > * mundo2 = new Mundo< Virtual >( "StarCraft", 64 );

    	    qDebug() << mundo1.toString();
    	    qDebug() << mundo2->toString();

	    return 0;
	}



Ejemplo 2
=========

- Las Personas se pueden identificar con lo que sea.

.. code-block:: c

	template< class T > class Persona  {
	private:
	    T id;
	    int edad;

	public:
	    Persona( T id ) : id( id ), edad( 0 )  {
	    }

	    T getId()  {
	        return id;
	    }
	};

.. code-block:: c

	class Cliente : public Persona< int >  {
	private:
	    int cantDolares;

	public:
	    Cliente() : Persona( 1001 ), cantDolares( 10 )  {
	    }
	};

	// Se puede instanciar con:    Cliente cliente;


.. code-block:: c

	template< class T > class Cliente : public Persona< T >  {
	private:
	    int cantPesos;

	public:
	    Cliente( T id ) : Persona< T >( id ), cantDolares( 600 )  {
	    }
	};

	// Se puede instanciar con:    Cliente< QString > cliente( "Algun nombre" );


.. code-block:: c

	struct Credencial  {
	    int dni;
	    QString nombre;
	};

	int main( int argc, char ** argv )  {
	    Persona< int > juan( 36242 );

	    Persona< QString > carlos( "Carlos" );	 
	    
	    Credencial credencial1;
	    credencial1.dni = 44123456;
	    credencial1.nombre = "Lucas";

	    Persona< Credencial > lucas( credencial1 );	 

	    return 0;
	}



Ejercicio 1
===========

- Punto de partida: Utilizar el código fuente del proyecto de la clase Listado que tiene definido el ``operator+``.
- Sobrecargar el ``void operator+( int cuantasNuevasCeldas )`` de tal manera permita agregar nuevas celdas vacías al final del Listado. Este operador no deberá modificar el contenido que ya tenga el Listado.
- También completar el código con las definiciones de los métodos ``clear``, ``pop_back``, ``erase`` e ``insert`` que ya hemos trabajado anteriormente.
- En la función main crear un ``Listado< QString >`` para 5 elementos como máximo y agregar 3 cadenas.
- Utilizar el operador definido en este entregable para aumentar a 10 la cantidad de celdas disponibles.
- Agregar 3 nuevas celdas y completar con QString todas las celdas disponibles.



Ejercicio 2
===========

- Crear una clase genérica SuperListado que herede de QVector
- Que tenga las mismas características que la clase Listado del ejercicio anterior.




**Para ver el registro de los Mini Exámenes**

- Entrar al siguiente `link para ver el registro de los mini exámenes <https://docs.google.com/spreadsheets/d/1Qza70R_ClLLmL0Cmw7cy4F1pwqAMejPwamK9Jmks4ic/edit?usp=sharing>`_ 


