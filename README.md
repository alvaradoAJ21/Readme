# Readme
SOD_PROYECTO_2P
Protección de recursos en Amazon Virtual Private Cloud (VPC): Subnets, NAT Gateway 
Pasos de Implementación:

Implementación
1.	Dentro de Amazon console, buscamos en la sección de servicios redes y entrega de contenido. Seleccionamos VPC (Virtual Private Cloud).
 
2.	Comprobamos en que región se encuentra corriendo nuestro servicios
 
3.	Podemos visualizar un VPC predeterminado del cual contamos en el momento que obtenemos una credencial de AWS.
 
4.	Creamos nuestro VPC personalizado.
Asignamos un nombre y una etiqueta opcional
En base al bloque de CIDR asignamos una dirección IP de la cual le proporcionamos 10.0.0.0/16.  
 
 
5.	Una vez creada la VPC personalizada podemos crear nuestras subredes dentro de ella.
Entramos al modulo de Subnets y comenzamos a crear las redes más pequeñas.
Seleccionamos la VPC que creamos.
 
Debemos asignar un bloque cidr para la subred que estamos creando y que sea perteneciente al bloque que asignamos anteriormente. 
10.0 permanecerá fijo para los 16bits dispuesto en el bloque anterior, por lo que no se puede cambiar. Debemos modificar el tercer octeto y terminando con el ultimo en 0. De esta manera estamos bloqueando esta subred que será privada.
 

6.	Creamos la subred publica 
 
7.	Asignamos el nombre de la subred pública.
Para esta subred, asignamos una dirección ip diferente a la subred privada. Con la misma lógica de la anterior el bloque quedará: 10.0.2.0/24. Asi obteniendo el bloque para esta subred.
 
Podemos visualizar que las dos subnet creadas están compuestas por el mismo VPC:
 
8.	Después de estar creada las subnet, nos dirigimos a crear la tabla de ruta.
Las tablas de ruta son el conjunto de reglas que se utilizan para determinar dónde se encuentra la red. El tráfico tiene que ser enrutado, especifica la subred de destino y el enrutador de la puerta de enlace para alcanzarlo, por lo que básicamente una tabla de rutas es un candado donde mantenemos todas las reglas o especificamos todas las reglas por las que decimos qué subred comunicarnos con internet de qué manera nos deja mejor.
8.1.	Creamos las tablas de rutas. Una para nuestra subred pública y otra para la subred privada.
 
9.	Una vez creada las tablas de rutas, les asignamos las subredes.
9.1 Seleccionamos una  de las tablas de ruta. Al seleccionar, abajo nos aparecerá una sección de menú. Seleccionamos Subnet Associations y editamos.
9.2 De acuerdo a la tabla de ruta que seleccionamos debemos asignarla la subred correspondiente. 
En este caso seleccionamos la tabla de ruta privada por lo cual asignamos la subred privada.

 
Esto básicamente significa todas las reglas, todas las condiciones de cómo esta subred se comunica con el enrutador, se bloqueará en esta tabla que este privado.

9.3 De manera similar asignamos la subred correspondiente a la tabla de ruta publica.
 

Implementando Gateway
Proporcionando un puerta de enlace
La puerta de enlace de Internet es un componente de VPC que ayuda a las instancias a comunicarse a través de Internet utilizando los destinos proporcionados en la tabla de rutas.

Para que podamos establecer conexión a internet por medio de las subredes creadas, aws cuenta con un servicio para lograr esta implementación.
 
10.	Creamos una nueva puerta de enlace. 
 
Una vez creada, el estado por dafault será Separado. 
 
Por lo consiguiente se deberá adjuntarlo a uno de los VPC creados.
 
11.	Actualizamos las tablas de rutas.
Seleccionamos la tabla de ruta publica para que esta pueda acceder al internet ya proporcionado por la puerta de enlace.

Damos clic en edit routes:
 

11.1 Colocamos las dirección ip inicial y seleccionamos en target Internet Gateway y adjuntamos el VPC creado. Guardamos
 

Con esto hemos creado una ruta para nuestra subred para conectarse a internet para lo que hemos proporcionado la información en el enrutado. 
Por lo que hasta hora hemos creado nuestro VPC, dos subredes, una puerta de enlace de internet y hemos actualizado nuestra tabla de rutas mencionando que la subred pública puede conectarse a internet utilizando el Gateway de Internet, del cual hemos asignado la puerta de enlace que habíamos creado.

12.	Para continuar con la implementación creamos 2 instancias para cada una de las dos subredes que habíamos creado.
12.1 Seleccionamos la que viene por defecto
 
 
12.2 Seleccionamos el VPC personalizadio. 
	Para esta instancia seleccionaremos la subred Privada y deshabilitamos la opción de que se asigne automáticamente una dirección IP pública
 
12.3 Dejamos el almacenamiento con la configuración predeterminada
	 
13.	Configuramos el grupo de seguridad. AWS nos proporciona un grupo de seguridad que esta a nivel de instancia, por lo cual podemos determinar quiénes pueden conectarse a esta instancia.
13.1 Añadimos nuevas reglas, con los protocolos HTTP y HTTPS.
Y lanzamos la instancia

 
Y continuando con lo propuesto, creamos una instancia para la subred pública

14.	Para poder comunicarnos con la instancia con subred pública podemos hacer la demostración por medio del terminal.
14.1 Nos dirigimos a conectar la instancia y seleccionamos la pestaña cliente SSH
 
Copiamos la dirección IP publica y hacemos la relación de confianza en el terminal.

Comprobamos si tenemos acceso al internet por medio del comando ping
 
15.	Para poder conectarnos a nuestra instancia privada, podemos hacerlo a través de nuestra instancia pública. Por medio de esta instancia daremos un certificado que se requiere para acceder a la instancia privada.
15.1 Accedemos a la instancia pública por medio del terminal
  15.1.1 Escribimos el siguiente comando: sudo su –
  15.1.2 Creamos un archivo en blanco, donde posteriormente contendrá la clave para conectarse a nuestra instancia privada.
 

15.1.3 Insertaremos la clave necesaria de nuestra instancia publica, copiamos.
            
 
   Abrimos y pegamos la clave
 
 
