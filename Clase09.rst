.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 09 - PGE 2022
===================
(Fecha: 13 de septiembre)


Registro en video de algunos temas de la clase de hoy
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

`Declaración adelantada - miembros static - operador global 2021 <https://youtu.be/falta subirlo a youtube Declaración adelantada - miembros static - operador global.mp4>`_

`eventFilter 2021 <https://youtu.be/ldUi95t49mo>`_



static
======

**Variables estáticas**

- Al salir de su ámbito no pierde su valor
- Sólo son conocidas dentro de su ámbito (pero igual "recuerdan su valor")
- Se inicializan sólo la primera vez

.. code-block:: c

	#include <QApplication>
	#include <QDebug>

	int funcion( int a = 2 )  {
	    static int suma = 0;
	    return ( suma += a );
	}

	int main( int argc, char ** argv )  {
	    QApplication a( argc, argv );

	    qDebug() << funcion();	    
	    qDebug() << funcion( 10 );	
	    qDebug() << funcion();	    

	    return 0;
	}

**Miembros estáticos**

- Para cada instancia de una clase existe una copia de los miembros no-estáticos.
- Pero hay una única copia de los estáticos para todas las instancias.
- Pueden ser accedidas sin referencia a ninguna instancia concreta de la clase.
- Los miembros estáticos no dependen de ninguna instancia para su existencia.
- Existen incluso antes que la primera instancia de una clase.

**¿Qué problema tiene este código?**

.. code-block:: c

	#include <QApplication>
	#include <QDebug>

	class A  {
	public:
	    static int x;
	};

	int main( int argc, char ** argv )  {
	    QApplication a( argc, argv );

	    A a1;
	    qDebug() << a1.x;		// No reconoce x

	    return 0;
	}

**¿Qué se publica?**

.. code-block:: c

	#include <QApplication>
	#include <QDebug>

	class A  {
	public:
	    static int x;
	};

	int A::x = 5;

	int main( int argc, char ** argv )  {
	    QApplication a( argc, argv );

	    A a1, a2;
	    qDebug() << a1.x;		
	    qDebug() << a2.x;		

	    a1.x = 9;
	    qDebug() << a1.x;		
	    qDebug() << a2.x;		

	    return 0;
	}

- La modificación del valor ``x`` en el objeto a1 cambia dicha propiedad ``x`` en ``a2``.
- La definición ``int A::x = 5;`` solo son permitidas para miembros estáticos.

**¿Qué error tiene el siguiente código?**

.. code-block:: c

	class B  {
	    static const char * p1;        // privado por defecto

	public:
	    static const char * p2;        // declaración
	    const char* p3;
	};

	const char * B::p1 = "Adios";     // Ok.  Definición
	const char * B::p2 = "mundo";     // Ok
	const char * B::p3 = "cruel";     // Error. No es estática. No se puede definir así.


- No significa que las propiedades estáticas (privadas o protegidas) puedan ser accedidas directamente desde el exterior. Depende del modificador de acceso:

.. code-block:: c

	int main( int argc, char ** argv ) {
	    QApplication a( argc, argv );

	    qDebug() << B::p1;    // Error: no accesible!
	    qDebug() << B::p2;    // Ok: -> "mundo"

	    return 0;
	}

**Definición de miembros estáticos**

- Si los miembros estáticos existen antes de cualquier instancia, entonces hay que definirlos. 
- Los métodos estáticos sólo pueden acceder a miembros estáticos.

**¿Qué problema tiene el siguiente código?**

.. code-block:: c

	class C  {
	    static int y;

	public: 
	    int x;
	    static int * p;
	    static const char * c;
	    static int getY()  { return y; }
	    static int getX()  { return x; }	// No compila. x no es estático.
	};

	int C::y = 1;          		// no se debe poner static
	int * C::p = &C::y;     		
	const char * C::c = "ABC";   

**El constructor y miembros estáticos**

- La inclusión de un constructor no evita tener que definir los miembros estáticos.
- Recordar que el constructor es invocado cuando se instancia.
- El constructor puede modificar los valores de los miembros estáticos pero no inicializarlos.

**¿El siguiente código compila?**

.. code-block:: c

	class D  {
	    static int y;

	public: 
	    int x;

	    // El constructor no puede modificar así los miembros estáticos
	    D() : y( 10 ), x( 20 )  {  }  
	};

	int D::y = 1;

- Se debería usar un constructor como el que sigue:

.. code-block:: c

	D() : x( 20 )  {
	    y = 10;
	}

**Particularidades de la notación**

- Los miembros estáticos pueden ser accedidos con :: con la notación C::miembro.
- No es necesario utilizar ninguna instancia concreta de la clase.

**¿Qué publicaría el siguiente código?**

.. code-block:: c

	#include <QApplication>
	#include <QDebug>

	class E  {
	public:
	    static int x;  // miembro estático
	    E( int i = 12 )  {  x = i;  }   

	};

	int E::x = 13;  // definicion de miembro

	int main( int argc, char ** argv )  {
	    QApplication( argc, argv );

	    qDebug() << E::x;   
	    E e1;
	    qDebug() << E::x;  

	    return 0;
	}



Captura de eventos con eventFilter
==================================

- En un QWidget dibujar una imagen de 50x50.
- Con un QTimer hacer rotar la imagen cada 20 mseg.
- Permitir desplazar la imagen con las teclas y usando eventFilter.
- Utilizar los métodos rotate y translate de QPainter.

.. code-block::

	#include <QWidget>

	class QTimer;
	class QImage;

	namespace Ui  {
	    class Principal;
	}

	class Principal : public QWidget  {
	    Q_OBJECT

	public:
	    explicit Principal( QWidget * parent = 0 );
	    ~Principal();

	private:
	    Ui::Principal * ui;
	    QTimer * timer;
	    int rotacion, transX, transY;
	    QImage * image;

	protected:
	    void paintEvent( QPaintEvent * e );
	    bool eventFilter( QObject *, QEvent * );

	private slots:
	    void slot_ciclos();
	};


	#include "principal.h"
	#include "ui_principal.h"
	#include <QTimer>
	#include <QPainter>
	#include <QKeyEvent>
	#include <QImage>

	Principal::Principal( QWidget * parent ) : QWidget( parent ), 
	                                           ui( new Ui::Principal ),
	                                           rotacion( 0 ), 
	                                           transX( 0 ), 
	                                           transY( 0 )
	{
	    ui->setupUi( this );
	    this->installEventFilter( this );
	    image = new QImage( ":/User.png" );
	    timer = new QTimer;
	    timer->setInterval( 20 );
	    connect( timer, SIGNAL( timeout() ), SLOT( slot_ciclos() ) );
	    timer->start();
	}

	Principal::~Principal()  {
	    delete ui;
	}

	void Principal::paintEvent( QPaintEvent * e )  {
	    QPainter* p = new QPainter( this );
	    p->translate( this->width() / 2 + transX, this->height() / 2 + transY );
	    p->rotate( 10*++rotacion );
	    p->drawImage( 0, 0, *image );
	    if ( rotacion >= 360 )
	        rotacion = 0;
	}

	void Principal::slot_ciclos()  {
	    this->repaint();
	}

	bool Principal::eventFilter( QObject * obj, QEvent * e )  {
	    if ( obj == this )  {
	        if ( e->type() == QEvent::KeyPress )  {
	            int key = static_cast< QKeyEvent * >( e )->key();

	            switch( key )  {

	            case Qt::Key_Up:
	                transY -= 10;
	                break;
	            case Qt::Key_Down:
	                transY += 10;
	                break;
	            case Qt::Key_Left:
	                transX -= 10;
	                break;
	            case Qt::Key_Right:
	                transX += 10;
	                break;
	            case Qt::Key_Escape:
	                this->close();
	                break;
	            }

	            // Devolvemos true para indicar que este evento ya lo controlamos aquí
	            // y no es necesario que se propague para que alguien más lo controle.
	            return true;  
	        }
	    }

	    // Esto es para que la clase base decida si necesita controlar los eventos
	    // que no estamos capturando nosotros.
	    return QWidget::eventFilter( obj, e );
	}



Ejercicio 1
===========

- Mencionar y usar todos los métodos estáticos que hemos visto en POO y PGE hasta ahora.
- Por ejemplo, el ``QString::number( int )``
- Trabajar entre estudiantes porque hay que recordarlos a todos y cada uno de ellos, quizás revisando la clases en GitHub.
- El desafío está en no olvidarse ninguno.
- Realizar un ejemplo de cada uno (puede ser en el mismo proyecto de Qt).


Ejercicio 2
===========

- Utilizar la clase Poste para sobrecargar operator+ para que sume un objeto Poste con un int.

- Esa sobrecarga nos permitirá hacer ``Poste suma = p1 + 5;``

- Qué pasa si queremos		 ``Poste suma = 5 + p1;``

- Proponer una solución.


Ejercicio 3
===========

- Modificar el ejemplo de la imagen rotando y detectar click derecho para cambiar el sentido de rotación.


Desafíos para el jueves 15 de septiembre
=======================================


`Clase ListadoEnteros <https://youtu.be/hwA4Yqcnl7Y>`_ - Código `aquí <https://github.com/cosimani/Curso-PGE-2021/blob/main/desafios/ListadoEnteros.rar?raw=true>`_

`Clase Figura <https://youtu.be/qEZY3__zwFg>`_ - Código `aquí <https://github.com/cosimani/Curso-POO-2021/blob/master/sources/desafios/Figura.rar?raw=true>`_




Opciones para regularizar y rendir final
========================================

- Primer parcial: 20 de octubre - Enunciado para resolver durante la clase
- Segundo parcial - opción 1: Promedio con Desafíos y Mini Exámenes
- Segundo parcial - opción 2: Enunciado para resolver durante la clase - 10 de noviembre
- Proyecto - opción 1: Hasta 2 estudiantes. Proponer un proyeto para ejecutar en Android.
- Proyecto - opción 2: Hasta 3 estudiantes. Proponer un proyeto para ejecutar en Android y dejar disponible en Google Play Store.
- Junto al proyecto se debe entregar un video en Youtube donde se expongan los cada uno de los temas de los contenidos mínimos. `Ver aquí un ejemplo de cómo entregar proyecto <https://youtu.be/VRrMuFUz61w>`_ 
- Con el proyecto se puede rendir el final y se evita rendir el primer parcial (no así el segundo).
- Contenidos mínimos: `Ver aquí los temas teóricos <https://github.com/cosimani/Curso-PGE-2022/blob/main/Desafios.rst>`_ 
- Objetivos de la asignatura: Poder explicar oralmente los contenidos mínimos, escribir código relacionado y tener destreza para crear aplicaciones que incluyan estos temas (tener presente que PGE es la continuación de POO, por lo tanto, se requieren también los conocimientos de POO).


