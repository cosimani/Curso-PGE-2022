.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 04 - PGE 2021
===================
(Fecha: 18 de agosto)

Registro en video de algunos temas de la clase de hoy
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

`Herencia con clases genéricas 2021 <https://youtu.be/FhUebWDCrXk>`_

`Sobrecarga de operadores con clases genéricas 2021 <https://youtu.be/qEvTlUbpsZc>`_

`Variantes de herencia con clases genéricas 2021 <https://youtu.be/rttDmneFJ3s>`_




Herencia con clases genéricas
=============================

.. code-block:: c

    template< class T > class Lista : public Listado< T >  {
 
        //////////

    };

- Tener presente que si heredamos de una clase genérica ``QVector`` o ``std::vector`` ya no es necesario definir las características de almacenamiento en la clase derivada. Es decir, ya no debemos definir ``libre``, ``T * v``, ``cantidad``, ``get``, ``add`` o ``size``.




Sobrecargando operadores en la clase genérica Listado
=====================================================

- Modificar la clase Listado sobrecargando ``operator+`` de tal forma que al sumar dos listados se obtenga un nuevo objeto Listado con los elementos consecutivos y la cantidad de celdas disponibles acumuladas.

.. code-block:: c

	template < class T > class Listado  {
	private:
	    int maxSize;
	    T * v;
	    int libre;

	public:
	    Listado( int maxSize = 10 );

	    T get( int i ) const;
	    int getMaxSize() const;
	    int size() const;

	    bool add( T contenido );

	    void clear();
	    bool pop_back();
	    bool erase( int i );
	    bool insert( int i, T contenido );

	    Listado< T > operator+( const Listado< T > otro );
	};


	template < class T > Listado< T >::Listado( int maxSize ) : maxSize( maxSize ),
	                                                            v( new T[ maxSize ] ),
	                                                            libre( 0 )
	{

	}

	template < class T > int Listado< T >::getMaxSize() const {
	    return this->maxSize;
	}

	template < class T > int Listado< T >::size() const {
	    return libre;
	}

	template < class T > T Listado< T >::get( int i ) const {
	    return v[ i ];
	}

	template < class T > bool Listado< T >::add( T contenido )  {
	    if ( maxSize <= libre )
	        return false;

	    v[ libre ] = contenido;
	    libre++;
	    return true;
	}

	template< class T > Listado< T > Listado< T >::operator+( const Listado< T > otro )  {
		T vAux[ this->size() + otro.size() ];

		int contador = 0;

		for ( ; contador < this->size() ; contador++ )
			vAux[ contador ] = this->get( contador );

		for ( int i = 0 ; contador < ( this->size() + otro.size() ) ; contador++, i++ )
			vAux[ contador ] = otro.get( i );

		Listado< T > res( this->getMaxSize() + otro.getMaxSize() );

		for ( int j = 0 ; j < contador ; j++ )
			res.add( vAux[ j ] );

		return res;
	}


Ejercicio 1
===========

- Utilizar el código fuente del proyecto en el cual se creó la clase Poste.
- Definir estos nuevos operadores en la clase Poste: ``float operator+( Poste poste )`` y ``Poste operator+( float altura )``.
- El primer operador suma la altura de los dos Postes y devuelve la altura total.
- El segundo operador le suma una altura particular a un Poste y devuelve un nuevo Poste con esa altura sumada.
- En la función main probar estos operadores.
