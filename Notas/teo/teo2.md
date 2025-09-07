## Repaso

Consistencia todos los servidores tienen la misma vista de lo que esta pasando. 
Liveness todos van a poder insertar nuevas transacciones, en cualquier momento. 
Es fundamental que el proceso de escribir una pagina sea probabilistico.
Si dos tienen suerte y tiran los mismos dados, tarde o temprano va a ganar uno solo. 
Vamos a usar poder computacional para resolver el rompecabezas y para comprobarlo.

Fairness, cada uno va a conseguir lo que quiere. 

Los smart contracts permiten tener relaciones arbitrarias entre los integrantes. 

Mining pools se ponen de acuerdo para resolver el acertijo criptografico y un grafico de torta que reparte. Si dos mining pools se juntan pueden dominar el sistema. 

# Lecture 2 

## Crypto background
El uso basico de funciones de hash es como estructura de datos. Para mapear un valor mucho mas grande a uno mas chico de tamaño fijo. Se usa para organizar los datos, como en una db tenemos la clave de un elemento que estamos buscando y hace que sea mucho mas facil encontrar los datos. 
Como mapeamos de algo mas grande a algo mas chico vamos a tener colisiones. 

Tenemos que tener un procedimiento para resolver estas colisiones, como una lista enlazada, recorriendo la lista uno va a encontrar el dato. 

Una funcion de hash criptografica significa que ademas de las propiedades anteriores queremos que sean faciles de calcular en una direccion pero dificil la vuelta (one way) y la otra propiedad importante es que tenga resistencia a la colicion. 

Las funciones de hash sirven para firmar versiones. 

Con MAC uno manda un mensaje grande y un hash chiquito y el receptor recibe el mensaje grande y comprueba que al hashearlo de lo mismo que el recibido. Ambos tienen que tener un secreto para que el que recibe comparte el secreto con el que mando para saber que viene del mensajero.

Las firmas digitales no son computacionalmente triviliales como las funciones de hash, son exponenciales y requieren de muchas multiplicaciones. Si queremos firmar algo muy largo como un libro, va a requerir un gran esfuerzo, por lo que uno lo compacta con un hash y despues firma el valor del hash. Si le mandamos muchos bloques va a tener que hacer la exponenciacion en cada bloque. 

Las funciones de hash no tienen ninguna clave secreta, es una funcion que uno establece que tiene buenas propiedades y son publicas, todos saben quienes las tienen.

Es dificil de encontrar una colision para una maquina de turing probabilistic polinomial time. 

Un ataque de cumpleaños por la paradoja del cumpleaños que se trata de si tenemos q personas en la misma habitacion, cual es la probabilidad de que dos de ellos compartan su cumpleaños. 

P a es la probabilidad de que dos personas cumplan años el mismo dia. 

La expresion viene de particiones. Se multiplica la probabilidad de que una persona no comparta cumpleaños. 

Los ataques de cumpleaños son independientes de la manera en la que la funcion de hash este diseñada. Podemos encontrar colisiones en cualquier funcion de hash. En la practica si agrandamos el tamaño del rango la raiz cuadrada del tamaño va a ser muy grande por lo que encontrar una colision va a ser dificil. 

Resistencia a la pre imagen, es inviable si te doy un hash encontrar su pre imagen. Es muy dificil "volver para atras". One way solo se puede calcular para un lado. Calcular para un lado es facil, para el otro dificil. 

Tambien es dificil encontrar un segundo elemento que vaya al mismo hash que otro. Ayudan a las firmas digitales. 

Para las funciones de hash uno propone una funcion y se arma una competencia para romperla, si romperla es inviable gana y se estandariza. 

MD5 y SHA-1 se conocen colisiones.SHA-1 es mucho más costoso que MD5 pero si se pueden encontrar colisiones. 

El numero significa el numero de los bits del hash de salida. 

Uno tiene un numeor aleatorio de entrada y lo separa en bloques, usa una compresion y se hace un or y hace una cadena de compreciones. 

Las hash functions se usan para apuntar a la pagina anterior, para concatenar los bloques. Vamos a tener una concatenacion de los bloques y con la funcion hash hacemos que un bloque apunte al que si le haces una hash function te da ese bloque. 

## Digital signatures
