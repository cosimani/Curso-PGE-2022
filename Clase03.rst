.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 03 - PGE 2022
===================
(Fecha: 16 de agosto)


`Clase Listado con métodos extras 2021 <https://youtu.be/JDt2nDk08L4>`_

`Herencia con clases genéricas 2021 <https://youtu.be/FhUebWDCrXk>`_

`Sobrecarga de operadores 2021 <https://youtu.be/QGTNAjeRdNg>`_



Sobrecarga de operadores 
========================

- Supongamos los siguientes objetos de la clase Poste:

.. code-block:: c

	Poste p1;  // Su único miembro dato es un float para la altura del Poste
	Poste p2;

- Necesitamos unir estos Postes para obtener un único Poste con sus alturas sumadas.
- ¿Podemos hacer lo siguiente?

.. code-block:: c

	Poste unidos = p1 + p2;

**Otro ejemplo**

.. code-block:: c

	class Cliente  {
	private:
	    int saldo;

	public:
	    Cliente() : saldo( 0 )  {
	    }

	    void operator+( int sumando )  {
	        this->saldo += sumando;
	    }

	    void operator-( int sustraendo )  {
	        this->saldo -= sustraendo;
	    }

	    bool operator<( Cliente otroCliente )  {
	        if ( this->saldo < otroCliente.saldo )
	            return true;
	        return false;
	    }
	};

	int main( int argc, char ** argv )  {
	    Cliente juan;

	    Cliente carlos;

	    juan + 50;  // Suma 50 a su cuenta

	    carlos + 100;  // Quita 100 a carlos

	    if ( juan < carlos )  {
	        qDebug() << "juan tiene menos";
	    }

	    return 0;
	}


Siguiendo con clases genéricas
==============================


**La clase Listado quedó así:**

.. code-block:: c

	#ifndef LISTADO_H
	#define LISTADO_H

	template < class T > class Listado  {
	private:
	    int cantidad;
	    T * v;
	    int libre;

	public:
	    Listado( int cantidad = 10 ) : cantidad( cantidad ), v( new T[ cantidad ] ), libre( 0 )  {  }

	    T get( int i )  {  return v[ i ];  }
	    bool add( T contenido );
	    int getCantidad()  {  return this->cantidad;  }
	    int size()  {  return libre;  }
	};


	template < class T > bool Listado< T >::add( T contenido )  {
	    if ( cantidad <= libre )
	        return false;

	    v[ libre ] = contenido;
	    libre++;
	    return true;
	}

	#endif // LISTADO_H



**¿Qué otros métodos sería oportuno agregar?**

- Método que elimine todos los elementos, que vacíe el Listado

.. code-block:: c

	void clear()

- Método que elimine un elemento del final.

.. code-block:: c
	
	void pop_back()
	
- Método que elimine el elemento de la posición i.

.. code-block:: c
	
	void erase( int i )

- Método que inserte un elemento en la posición i desplazando los otros

.. code-block:: c

	bool insert( int i, T elemento )	

- Modificar listado.h para que todos sus métodos queden definidos de manera off-line



Herencia con clases genéricas
=============================

.. code-block:: c

    template< class T > class Lista : public Listado< T >  {
 
        //////////

    };

- Tener presente que si heredamos de una clase genérica ``QVector`` o ``std::vector`` ya no es necesario definir las características de almacenamiento en la clase derivada. Es decir, ya no debemos definir ``libre``, ``T * v``, ``cantidad``, ``get``, ``add`` o ``size``.



Ejercicio 1
===========

- Utilizar el código fuente de la clase Listado.
- Agregar el siguiente método para eliminar el elemento de la posición i.

.. code-block:: c

void erase( int i )

- Agregar el método que elimine un elemento del final.

.. code-block:: c
	
	void pop_back()


Ejercicio 2
===========

- Agregar los siguientes dos métodos: ``borrar_del_final( int cuantos )`` y ``borrar_del_principio( int cuantos )``. 
- Tener en cuenta que tenemos ya definidos métodos que borran elementos, entonces, utilizarlos para ahorrar tiempo de desarrollo.
- En la función main crear un ``Listado< str::string >`` y agregar 8 cadenas
- Borrar 2 elementos del final y borrar 2 elementos del principio
- Recorrer el Listado con un for y mostrar los elementos que quedan


