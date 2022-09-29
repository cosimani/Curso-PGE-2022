.. -*- coding: utf-8 -*-

.. _rcs_subversion:

Clase 14 - PGE 2022
===================
(Fecha: 29 de septiembre)




Clase QThread
============

- Permite crear hilos de ejecución para realizar varias tareas a la vez. 
- Proporciona el método start() para iniciar el hilo.
- Emite señales para indicar el inicio y fin de la ejecución del hilo.
- Se necesita reimplementar el método run() en una clase derivada de QThread.
- El código dentro de run() se ejecuta en un hilo y finaliza cuando retorna.
- La programación multihilo es útil para realizar tareas que consumen tiempo sin congelar la interfaz de usuario.

.. code-block:: c++

	class MiHilo : public QThread  {
	    Q_OBJECT

	protected:
	    void run();
	};

	void MiHIlo::run()  {

	    ...

	}

	
- Las clases no GUI (QTimer, QTcpSocket, QFtp, etc.) fueron diseñadas para funcionar en un hilo independiente.
- Las clases GUI (QWidget y derivadas) sólo se puede usar desde el hilo principal.
- Para consultar el estado del hilo podemos utilizar isFinished() o isRunning().
- Podríamos terminar un hilo a fuerza bruta con terminate().
- Dormimos el hilo con: ``sleep(int seg)`` o ``msleep0(int miliseg)`` o ``usleep(int microseg)``


Ejemplo: Clase Factorial
========================

.. code-block:: c++

	class Factorial : public QThread  {
	    Q_OBJECT

	public:
	    Factorial( QObject * parent = nullptr );
	    ~Factorial();
	    void setNumero( unsigned int numero );

	private:
	    unsigned int numero;

	protected:
	    void run();

	signals:
	    void signal_resultado( double resultado );
	}

.. code-block:: c++

	Factorial::Factorial( QObject * parent ) : QThread( parent ), numero( 1 )  {

	}

	Factorial::~Factorial()  {  

	}

	void Factorial::setNumero( unsigned int numero )  {
	    if ( numero <= 0 )  {
	        this->numero = 1;
	    }
	    this->numero = numero;
	}

	void Factorial::run()  {
	    double resultado = 1;

	    for ( unsigned int i = 0 ; i <= numero ; i++ )  {
	        resultado = resultado * i;
	    }

	    emit signal_resultado( resultado );
	}


Ejercicio 1:
============

- Implementar la clase Factorial en una aplicación
- Crear una GUI que solicite el número para calcular el factorial.
- La interfaz no se debe colgar/tildar.

Ejercicio 2:
============
	
- Diseñar una aplicación GUI que escriba en un archivo muchísimos caracteres de tal forma se note que la interfaz de usuario se bloquea hasta finalizar la escritura.
- Luego de esto, utilizar un hilo distinto para escribir la misma cantidad de caracteres.

Ejercicio 3:
============

- Diseñar una clase Medidor que sirva para saber si la conexión a internet es buena
- Es un singleton
- ``bool isOk();`` indica si la conexión es buena o no.
- Debe ser una clase independiente
- La instancia de esta clase permitirá hacer lo siguiente:

.. code-block:: c++

	if ( Medidor::getInstancia()->isOk() )  {
	    manager->get( QNetworkRequest( QUrl( "http://mi.ubp.edu.ar" ) ) );
	} 
	else  {
	    QMesaggeBox::critical( this, "Internet", "Muy lenta" );
	}


Ejercicio 4:
============

- Diseño de GUI pensando en smart phone
- Usar fuentes propias. ``QFontDatabase::addApplicationFont( ":/resources/fuentes/angelina.ttf" );``
- Diseñar un interfaz con botones propios que usen estas fuentes.


Ejercicio 5:
============

- Pensar en el diseño de una API propia para validar usuarios
- Disponer de un servidor con PHP y MySQL para tener la base de datos con una tabla para usuarios
- Escribir un script para validar los usuarios en esa API
- Desarrollar un Login independiente que use un ``QNetworkAccessManager`` para validar contra la API








