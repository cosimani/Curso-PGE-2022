.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 17 - PGE 2022
===================
(Fecha: 11 de octubre)

GUI
===

- Con `este código fuente <https://github.com/cosimani/Curso-PGE-2022/blob/main/recursos/gui_botonera.rar?raw=true>`_ se genera la siguiente interfaz.

.. figure:: images/gui_botonera.png


Ejercicio 1:
============

- Abrir el proyecto gui_botonera y analizarlo.
- Modificar el comportamiento de los Block para que al hacer click se mantenga en verde durante 500 ms.



Otros ejercicios con punteros a funciones
=========================================



Ejercicio 2:
============

- Modificar el ejercicio de la clase ListadoEnteros para usar funciones globales de ordenamiento, es decir, que no se encuentren dentro de Ordenador ni de ninguna clase.

.. code-block:: c++	

	class ListadoEnteros : public QVector<int>  {
	public:
	    void ordenar( void ( * pFuncionOrdenamiento )( int *, int ) )  {
	        ( *pFuncionOrdenamiento )( this->data(), this->size() );
	    }
	};

.. code-block:: c++		
	///// Desde main se puede utilizar así:

    void ( * ordenador )( int *, int ) = &burbuja;

    listado.ordenar( ordenador );

Ejercicio 3:
============

- Modificar el ejercicio anterior usando también funciones globales de ordenamiento pero con la clase ListadoGenerico que sea un template:

.. code-block:: c++	

	template< class T > class ListadoGenerico : public QVector< T >  {
	public:
	    void ordenar( void ( * pFuncionOrdenamiento )( T *, int ) )  {
	        ( * pFuncionOrdenamiento )( this->data(), this->size() );
	    }
	};

Ejercicio 4:
============

- Necesitamos conocer el rendimiento de cada algoritmo de ordenamiento midiendo su tiempo.
- Utilizar un array de punteros a funciones que apunte a cada función global de ordenamiento.
- Utilizar Archivador para almacenar los tiempos en un archivo.
- Utilizar un ListadoEnteros de 50.000 números generados con qrand()

.. code-block:: c++		

	///// Desde main se puede utilizar así:

    void ( * ordenador[ 2 ] )( int *, int );
    ordenador[ 0 ] = &burbuja;
    ordenador[ 1 ] = &insercion;

    listado.ordenar( ordenador[ 1 ] );

**Ejemplo del uso de callback en el mecanismo de SIGNAL y SLOT de Qt**

.. code-block:: c++

	#ifndef CONTADOR_H
	#define CONTADOR_H

	#include "ventana.h"
	#include <QDebug>
	#include <QThread>

	class Contador : public QThread  {

	public:
	    Contador() : contador( 0 ),
	                 hastaCuanto( 0 ),
	                 isRunning( false),
	                 puntero( nullptr ),
	                 ventana( nullptr )
	    {

	    }

	void setInterval( unsigned int hastaCuanto )  {
	    this->hastaCuanto = hastaCuanto;
	}

	void run()  {
	    if ( ! puntero || ! hastaCuanto )
	        return;

	    isRunning = true;

	    while( isRunning )  {
	        while ( contador < hastaCuanto )
	            contador-=-1;

	        contador = 0;
	        ( ventana->*puntero )();  // Esto es emitir la signal
	    }
	}

	void conectar( Ventana * ventana, void ( Ventana::*puntero )() )  {
	    this->puntero = puntero;
	    this->ventana = ventana;
	}

	void stop()  {
	    isRunning = false;
	}

	private:
	    unsigned int contador;
	    unsigned int hastaCuanto;
	    bool isRunning;
	    void ( Ventana::*puntero )();
	    Ventana * ventana;
	};

	#endif // CONTADOR_H

.. code-block:: c++

	#ifndef VENTANA_H
	#define VENTANA_H

	#include <QWidget>

	class Contador;

	class Ventana : public QWidget  {
	    Q_OBJECT

	public:
	    Ventana( QWidget * parent = nullptr );
	    ~Ventana();

	    void slot_sinSerSlot();

	private:
	    Contador * contador;
	};

	#endif // VENTANA_H

.. code-block:: c++

	#include "ventana.h"
	#include "contador.h"
	#include <QDebug>

	Ventana::Ventana( QWidget * parent ) : QWidget( parent ),
	                                       contador( new Contador )
	{
	    // Con setInterval se define hasta que numero debera contar 
	    // para realizar la retrollamada (o devolucion de llamada)
	    contador->setInterval( ( unsigned int )500000000 );

	    // Para conectar se puede definir un puntero a funcion y apuntarlo al metodo
	    //    void ( Ventana::*puntero )() = &Ventana::slot_sinSerSlot;
	    //    contador->conectar( this, puntero );

	    // O se puede apuntar al metodo sin declarar un puntero a funcion
	    contador->conectar( this, &Ventana::slot_sinSerSlot );

	    // También las siguientes expresiones son equivalentes:
	    //    connect( sender, SIGNAL( valueChanged( QString, QString ) ), 
	    //             receiver, SLOT( updateValue( QString ) ) );
	    //
	    //    connect( sender, &Sender::valueChanged, 
	    //             receiver, &Receiver::updateValue );

	    contador->start();
	}

	Ventana::~Ventana()  {
	    contador->stop();
	}


	void Ventana::slot_sinSerSlot()  {
	    qDebug() << "timeout";

	    // Tener en cuenta que Contador tiene un metodo stop para finalizar el contador
	    //    contador->stop();
	}

.. code-block:: c++

	#include "ventana.h"
	#include <QApplication>

	int main( int argc, char ** argv )  {
	    QApplication a( argc, argv );

	    Ventana w;
	    w.show();

	    return a.exec();
	}

Ejercicio 5:
============

- Analizar y hacer funcionar el ejemplo del callback con SIGNAL y SLOT

Ejercicio 6:
============

- Las expresiones equivalentes del tipo ``connect( sender, &Sender::valueChanged, receiver, &Receiver::updateValue );`` se pueden usar
- Modificar algún proyecto anterior para utilizar estas expresiones equivalentes.


Conversatorio del jueves 13 de octubre
======================================

- Compartir experiencias laborales, entrevistas, cursos, tendencias, requisitos, innovaciones, motivaciones, ...

- Francisco Andreoli: Entrevista laboral y presentación
- Matias: ¿Cómo es tu día a día en el trabajo?
- Lucía: ONIET
- Santiago Schuf: ¿AWS es un camino que hay que tomar?
- Francisco Aguiar: Cursos de capacitación.
- Tomás: ¿Qué hay en el mundo Unity?
- Santiago Arteta: Desarrollo web para ASOMA y trabajo en empresa.
- Ignacio: ¿A qué te querés dedicar?
- Luciano: ¿En qué negocio te gustaría estar?
- Marcos: ¿Qué oportunidades hay con los sistemas embebidos?
- Luis: ¿Cómo es tu trabajo en empresa?







