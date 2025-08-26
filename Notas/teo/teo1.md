Criptografia moderna habilita nuevas formas de interactuar en medios digitales. 

Demostrar que hicieron "esfuerzo" para participar en la blockchain.

Proof of work hizo esfuerzo computacional para poder participar. 
Proof of stake cuanto invirtio y en base a eso va a tener su voz y participar. 

Mejor libro el numero 4. El primero el que mas se basa la materia. Dividir las aplicaciones de las aplicaciones que corren en la blockchain. Establecer que propiedades uno puede establecer sobre si la blockchain funciona bien. Basado en esas propiedades determinar si las aplicaciones estan bien. Una aplicación puede ser una que verifique si en Bitcoin existen los fondos para hacer una transferencia. 
La ventaja de separar es que podes definir las propiedades de la blockchain funcionan, entonces las propiedades de la aplicacion se van a satisfacer. 

# Lecture 1
En sistemas centralizados hay un solo participante que define quien tiene acceso a los datos. El problema basico de hacer las cosas de esta manera es que el que esta a cargo desaparece el sistema deja de funcionar. 

Los sistemas distribuidos hay varios nodos que controlan el sistema en conjunto, la ventaja es que este sistema es tolerante a fallos, si alguno de los nodos funciona mal el sistema sigue funcionando. La diferencia es que la participacion es controlada. Solo los participantes que estan autorizados pueden participar. Para esto necesitamos criptografia para demostrar que los participantes estan autorizados. Todos conocen quienes son los participantes.

Open access, todos pueden participar, todos son autorizados. Son dinamicos, los participantes pueden unirse, trabajar, participar e irse cuando quieran. Pero si se van y vuelven tienen que ponerse al tanto de que es lo que paso, asi funcionan las blockchains actuales.

Definicion basica de blockchain es que es una base de datos distribuida, que cumple propiedades de safety y liveness. Safety significa que no pueden haber inconsistencias en el registro de transacciones, lo que la blockchain considera válido, nunca va a ser incorrecto. Por ejemplo si se efectua un pago no se puede hacer un rollback y hacer de cuenta que nunca sucedio.
Liveness significa que no se tiene que parar si los participantes quieren hacer cosas el sistema sigue funcionando. Estas propiedades son clasicas en sistemas distribuidos controlados, donde todos saben quienes estan participando. 

Un ledger es un registro donde se anotan todas las transacciones. Es un libro que mantiene todas las transacciones. La aplicacion basica es tener un ledger distribuido a traves de una blockchain. 
Diagrama izquierda centralizado. Derecha cada cliente que participa en la aplicacion. Cuando una transaccion le llega a un participante todos tienen que estar al tanto de la transaccion. 

Como se va a implementar esto, tenemos clientes a la aplicacion y los que mantener el sistema son los servidores, se van a tener que encargar de que todos vean lo mismo (consistencia). 

Consistencia y safety se usan de manera intercambiable. 



## La paradoja del libro. 
La importancia del consenso es que cada uno escribe una pagina pero como pones estos libros juntos. Las paginas que no se comitearon las llamamos huerfanas. Hay una competencia entre escritores para ver quien puede agregarle una pagina al libro. 

Las reglas para añadir paginas: Anuncias que agregaste una pagina y todos los demas se enteran que agregaste una pagina y van a usar esa version del libro. 

Producir una pagina no es gratis, requiere un esfuerzo. Uno va a producir una pagina si al tirar los dados gana y ese escritor va a ser el que agregue una pagina al libro. Es un proceso probabilistico, todos van a intentar con los dados y solo algunos van a ganar y esos van a ser los que extiendan el libro. 
Si fuese gratis producir una pagina tendria que haber una forma de resolver quien va a ser el ganador. Va a haber una remuneracion por crear una pagina. 

Muy facil verificar que el rompecabezas fue resuelto pero dificil de resolverlo. 

## Ejemplo de Bitcoin. 

La gente participaria en Bitcoin por ser beneficiadas economicamente. Usuarios le van a pagar a otros para que corran el sistema. 
Gran parte del analisis va a ser criptografico, tratar de garantizar que esto va a funcionar siempre independientemente de que haya una fraccion de participantes que sean maliciosos. Para que esto sea seguro y funcione siempre el adversario va a ser maligno. 

Tiene que tener miles de transacciones por segundo, la respuesta es automatica. Cuando hay una transaccion tendria que resolverse de forma inmediata. Con tarjetas de credito esto pasa porque hay una entidad centralizada que aprueba inmediatamente, si se cumplen las condiciones. Como lo vamos a hacer de manera descentralizada con un registro va a tomar tiempo para que se distribuya y se pongan de acuerdo los nodos. 

Cuando la transaccion es registrada en la blockchain el seller le da la bicicleta. 

Las ventajas de usar este tipo de criptomoneda es: 

Resilence, el libro vive en la red y la red se va a mantener descentralizada y no hay ningun componente que deja de funcionar se cae el sistema. 

## Smart Contracts
Si tenemos este libro que creamos, porque no lo usamos para otras cosas. Podemos usarlo sobre relaciones arbitrarias entre cuentas. Los escritores van a tener que verificar que estas relaciones se cumplen. Los mineros van a tener que verificar que ese programa haga lo que tiene que hacer y va a quedar registrado en la blockchain. Los miners van a tener que activamente ejecutar programas y eso va a costar. Va a requerir que el miner ejecute programas y va a costar mas que una transferencia de fondos. Se va a medir la cantidad de pasos que toma ejecutar estos programas y se le va a transferir.