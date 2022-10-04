.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 15 - PGE 2022
===================
(Fecha: 4 de octubre)




typeid
======

- Comprueba si un objeto es de un tipo específico.
- O bien, si dos objetos son del mismo tipo.
- Este tipo de identificador en tiempo de ejecución se conoce como RTTI (Run-Time Type Identification).
- typiid devuelve una referencia a un objeto de tipo const type_info.

.. code-block:: c++

	#include <QApplication>
	#include <QDebug>
	#include <typeinfo>

	int main( int argc, char ** argv )  {
	    QApplication a( argc, argv );

	    qDebug() << typeid( int ).name();

	    int x = 10;
	    qDebug() << typeid( x ).name();

	    const std::type_info & ref = typeid( int );
	    qDebug() << ref.name();

	    return 0;
	}


**Clase type_info**

- Dispone de un método para preguntar si es puntero y otro método para saber si es puntero a función:
		    
.. code-block:: c++
			
	virtual bool __is_pointer_p() const;
   
	virtual bool __is_function_p() const;


- Dispone de los operadores ``==`` y ``!=``
- Con os cuales podremos preguntar:

.. code-block:: c++
			
	if ( typeid( a ) == typeid( b ) )
	    cout << "Son iguales";

- Es necesario incluir ``<typeinfo>``
- Dispone del método ``name()`` que nos da datos del objeto devuelto por ``typeid``.
- Los nombres de los tipos no está normalizado, dependerá del compilador.


.. code-block:: c++

	ClaseA aa;
	ClaseB bb;
	ClaseA * pa = &aa;
	
	qDebug() << typeid( *pa ).name();  // 6ClaseA
	qDebug() << typeid( pa ).name();   // P6ClaseA
	qDebug() << typeid( bb ).name();   // 6ClaseB

	if ( typeid( aa ) == typeid( *pa ) )
	    qDebug() << "Son iguales";
	else
	    qDebug() << "Son distintos";


Ejercicio 1:
============

.. figure:: images/ejercicio_typeid.png



Uso de dynamic_cast. Uso de typeid
==================================


.. code-block:: c++

	#ifndef PERSONA_H
	#define PERSONA_H

	#include <QString>

	class Persona  {
	public:
	    Persona() : dni( 0 )  {  }
	    virtual ~Persona()  {  }

	private:
	    int dni;
	};

	class Cliente : public Persona  {
	public:
	    Cliente() : cuenta( 0 ), tipo( "Corriente" )  {  }

	private:
	    int cuenta;
	    QString tipo;
	};

	class Empleado : public Persona  {
	public:
	    Empleado() : sueldo( 0 )  {  }

	private:
	    int sueldo;
	};

	#endif // PERSONA_H


.. code-block:: c++

	#ifndef PRINCIPAL_H
	#define PRINCIPAL_H

	#include <QWidget>
	#include <QVector>
	#include "persona.h"

	namespace Ui {
	    class Principal;
	}

	class Principal : public QWidget
	{
	Q_OBJECT

	public:
	    explicit Principal( QWidget * parent = nullptr );
	    ~Principal();

	private:
	    Ui::Principal * ui;
	    QVector< Persona * > vector;

	private slots:
	    void slot_agregar();
	};

	#endif // PRINCIPAL_H


.. code-block:: c++

	#include "principal.h"
	#include "ui_principal.h"
	#include <QDebug>

	Principal::Principal( QWidget * parent ) : QWidget( parent ),
	                                           ui( new Ui::Principal )  {
	    ui->setupUi( this );

	    connect( ui->pbCliente, SIGNAL(pressed()), this, SLOT(slot_agregar()) );
	    connect( ui->pbEmpleado, SIGNAL(pressed()), this, SLOT(slot_agregar()) );
	}

	Principal::~Principal()  {
	    delete ui;
	}

	void Principal::slot_agregar()  {

	    if ( this->sender() == ui->pbCliente )  {
	        if ( vector.isEmpty() )  {
	            vector.push_back( new Cliente );
	            qDebug() << "Se agrega como primer elemento un Cliente";
	        }
	        else  {
	            Cliente * pd = dynamic_cast< Cliente * >( vector.at( 0 ) );

	            if ( pd != 0 )  {
	                vector.push_back( new Cliente );
	                qDebug() << "Se agrega otro Cliente. El Cliente numero" << vector.size();
	            }
	            else  {
	                qDebug() << "Este QVector no contiene Clientes. No se agrega nada.";
	            }
	        }
	    }
	    else  {
	        if ( vector.isEmpty() )  {
	            vector.push_back( new Empleado );
	            qDebug() << "Se agrega como primer elemento un Empleado";
	        }
	        else  {
	            Empleado * pd = dynamic_cast< Empleado * >( vector.at( 0 ) );

	            if ( pd != 0 )  {
	                vector.push_back( new Empleado );
	                qDebug() << "Se agrega otro Cliente. El Empleado numero" << vector.size();
	            }
	            else  {
	                qDebug() << "Este QVector no contiene Empleados. No se agrega nada.";
	            }
	        }
	    }
	}


Ejercicio 2
===========

- Crear objetos comparables: Persona (sin herencia), Login (hereda de QWidget), Imagen (hereda de QImage)

Ejercicio 3
===========

- AdminDB con todas las funcionalidades y singleton

Ejercicio 4
===========

- Crear una clase Archivador, que sea singleton y permita almacenar Logs en un archivo de texto.

Ejercicio 5
===========

- LineaDeTexto listo para promocionar y con dos opciones de sugerencias: desde Google o desde lista fija. 
- Que permita setear la segunda columna con el dato que se desee.

Ejercicio 6
===========

- QMainWindow que tenga programado un método que reciba una enumeración en su constructor (SUBLIME, DARK, GALERIA, CLASSIC) que permita combinar (por ejemplo, DARK|SUBLIME o DARK|GALERIA)

Ejercicio 7
===========

- Preparar instructivo de instalación de herramientas para el desarrollo en Android.
- Android Studio, NDK, JDK, Apache Ant, Qt Creator con Android.
- Explorar, instalar, probar, configurar, anotar las versiones que se instalan, compatibilidades entre versiones, anotar lo que se hace.


Desafío para el jueves 6 de octubre
===================================

`Personas comparables <https://youtu.be/wEtAjT7Lwb4>`_ - Código `aquí <https://github.com/cosimani/Curso-PGE-2021/blob/main/desafios/PersonasComparables.rar?raw=true>`_


