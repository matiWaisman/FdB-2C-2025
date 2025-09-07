Las estructuras de datos autenticadas se usan para un servicio de almacenamiento. Tengo todos estos datos y tengo que estar seguro del que los mantiene me los devuelva siempre. Aserciorarme que hace lo que tiene que hacer el servidor. 

Un protocolo es una coleccion de algoritmos distribuidos. Hay instrucciones que dicen como es la interaccion entre las partes.

Con una función hash el que guarda el archivo se puede asegurar que si guarda el archivo y lo vuelve a pedir el contenido va a seguir siendo el mismo. Fingerprinting. El problema con esto es que el archivo se perdio pero al menos sabes que lo que te dieron no es lo mismo. 

Un merkle tree, sirven para obtener parte de un archivo sin tener que guardar todo. Es un arbol binario autenticado.
La direccion de la flecha marca para que lado se aplica la funcion. Le aplicamos la funcion de hash a cada pedazo. Pasan de medir 1 KB a 32 bytes. 

El || es concatenacion. 

El chiste es que el usuario se quede con el root del arbol y al pedir un cacho de un archivo verificar que ese cacho que le dieron esta sin modificar. 

Solo te mandan un pedazo del arbol por nivel para que no sea necesario que te manden todo. Entonces es necesario que te manden por un nivel un solo dato. 

Tambien se pueden usar para almacenar sets de claves en vez de listas. 

Para hacer la prueba de no inclusion el cliente va a probar que el que estaba buscando no esta. Va a probar que el que viene antes y que viene despues son correctos y que son aledaños. 
La prueba de no inclusion tiene la misma complejidad que la prueba de no inclusion. 

Si el servidor quiere pretender darle algo distinto tiene que encontrarse algo igual que hashee a lo mismo. Imposible. 

El merkle tree permite demostrar que hay elementos en el arbol y tambien demostrar que hay elementos que no estan. Despues de que el cliente haga un preprocesamiento. Son criptograficamente seguros. No se usan solo para comprimir la comunicacion, si no para que hayan ciertas propiedades de seguridad que se puedan garantizar. 

## Tries
Es una estructura de datos que esta ordenada, sirven para almacenar claves valor. 
Permite hacer operaciones dinamicas. 
La query es si esta el elemento en las claves o no. Las aristas que salen de la raiz van a estar asociadas a un caracter. 

En los merkle trees los arboles siempre estan balanceados, con un trie se va a tirar para un lado o otro. 
Trie vienen de retrieval, porque es muy facil conseguir o agregar un dato. 

El arbol de patricia condensa caracteres en una sola arista. 

Los bloques son una especie de estructura de datos, son la forma en que estan organizados. Lo que hablamos es como se organizan los datos dentro de los bloques. 

Ahora vamos a ver como se organizan los bloques como tal, van a ser funciones de datos autenticadas tambien porque usan funciones de hash.

## Bloques o como se llame el titulo
La referencia al bloque anterior es con funciones hash tambien. 

La flecha apunta en sentido de inclusion. El segundo bloque sigue al primero, los bloques se van generando de izquierda a derecha. 

El nonce es el contador que se uso para calcular una pow. 
EL nonce, los dtos (que puede ser un merkle tree o una raiz de un merkle tree) y la referencia va a ser un hash de lo anterior. Todo esto va a ser el header del bloque. 

Los datos van a depender de la aplicacion. Utxo significa unspent transaction output.

En eth tambien puede tener parte del codigo de un contrato. Account based significa que los clientes que hacen transacciones estan asociados a una cuenta. 

El protocolo se llama gossip por susurrar, pasar un chimento. Se llama asi porque como se conectan los clientes entre mineros y los mineros entre si es una red P2P donde los servidores estan conectados con otros servidores, cuando alguien pone alguien en la red se distribuye por la red. 

Para validar la transaccion hay que validar la verificacion de esa firma que referencia esa transaccion. 

Todas las transacciones son visibles. 

El output de la transferencia es una clave publica que sirve para verificar la transferencia con el script. 

Las sybil resistance sale de la PoW. 

La diferencia de la state machine tradicional de sistemas distribuidos con la de eth es que la tradicional los participantes se conocen pero la de eth no. 

Las transacciones en eth pueden ser contratos que pueden hacer cosas. Cuando aparecen las transacciones una maquina virtual van a hacer cambios al estado global.
Los mineros pueden estar corriendo programas. 

En etherum hay cuentas como cuentas del banco. 
Es distinto al txo. 

Tenemos cuentas para guardar eter, otros para contratos. Las de contratos van a ser mas complicados que el de las cuentas personales. 
El contrato lo que va a necesitar es que se ejecute, que todos los mineros lo ejecuten, va a correr por los mineros. Va a haber que pagar por ejecutar ese codigo y almacenar en memoria. Cuando un contrato tenga que correr va a haber una oferta y que el contrato no termine. Van a correr el programa hasta que se quede sin plata para evitar el halting problem. Si no termino y te quedaste sin plata sonaste. 