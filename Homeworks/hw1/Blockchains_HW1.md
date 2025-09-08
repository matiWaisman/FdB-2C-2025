# Ejercicio 1  

## Punto A  

El objetivo de los Merkle Trees es autenticar que el contenido devuelto por un servidor coincide con el contenido original, sin necesidad de almacenar dicho contenido localmente. De esta forma, el usuario puede verificar la autenticidad de lo recibido, ya sea el archivo completo o un bloque específico, sin necesidad de guardar el archivo original localmente.  

El usuario solo necesita guardar un único hash: la raíz del Merkle Tree, cuyo tamaño es muy inferior al del archivo original. Al recibir información del servidor, el usuario puede comprobar su validez hasheando el contenido y verificando si el resultado coincide con el hash almacenado, mediante una *prueba de inclusión*.  

Las funciones de hash, aunque no necesariamente criptográficas, permiten representar el contenido original de manera mucho más compacta que guardando el archivo completo. Esto se debe a que mapean entradas de tamaño arbitrario a valores de longitud fija. Como el ejemplo que vimos en clase, donde bloques de 1 KB de datos se mapean a hashes de solo 32 bytes.  

El procedimiento consiste en que, antes de enviar el archivo al servidor, el usuario calcula el árbol de hashes, donde cada hoja es el hash del bloque original, y cada nodo que no es una hoja es el hash de la concatenación de los hashes de sus hijos.  

![alt text](image.png)  

Es importante destacar que los Merkle Trees *no* permiten recuperar el contenido en caso de que el servidor devuelva datos incorrectos, pero sí permiten verificar si lo que devuelve coincide o no con lo que se guardó. Esto ocurre por el uso de funciones de hash criptográficas, las cuales son de un solo sentido (*one-way*), por lo que resulta computacionalmente imposible obtener la preimagen de un hash.  

El aspecto fundamental de utilizar funciones de hash criptográficas es que resulta computacionalmente imposible encontrar dos entradas distintas que produzcan el mismo valor de hash. Si esto fuera sencillo, un servidor malicioso podría, al solicitarse un bloque junto con la cadena de hashes correspondiente para verificar su inclusión, entregar un bloque modificado junto con hashes falsos que, sin embargo, reproduzcan la misma raíz que el usuario tiene almacenada. De ese modo, podría engañar al usuario entregándole un bloque incorrecto.  

Como en la práctica es computacionalmente imposible encontrar colisiones, este esquema resulta seguro para realizar pruebas de inclusión o de no inclusión. Porque la única manera de que sea verdadera la prueba de inclusión es enviando el bloque verdadero y los hashes que hacen valer la prueba de inclusión.  

## Punto B
Por lo que investigue, si se quiere formar un arbol con una cantidad de transacciones que no es una potencia de 2, se duplica la ultima para tener un arbol completo con cantidad de hojas igual a una potencia de dos. [Link 1](URLhttps://bitcoin.stackexchange.com/questions/79364/are-number-of-transactions-in-merkle-tree-always-even), [Link 2](https://www.geeksforgeeks.org/software-engineering/blockchain-merkle-trees/)
Otras posible solucion que vi es completar el arbol con un valor por default [Link 3](https://ethresear.ch/t/merkle-tree-formation-with-odd-number-of-leaves/2681). Asi que debajo dejo un esquema que sigue el formato de la clase y repite el ultimo bloque para tener una cantidad de hojas igual a una potencia de dos. 

![alt text](mt.jpg)  

Importante aclarar que, dado que las funciones de hash son determinísticas y para un valor de entrada solo pueden devolver un valor de salida, $H_5 = H_6 = H_7 = H_8$ y $H_{56} = H_{78}$. Pero me parece más claro el esquema numerando los hashes por el orden.


## Punto C
![alt text](patriciatrie.jpg) 

# Punto 2 
Suponiendo que el día en el que nace una persona es equiprobable, cosa que en la realidad no debe ser cierto: 
# Punto 3 
La función de hash propuesta recibe cadenas binarias de cualquier longitud y devuelve una cadena binaria de longitud $n$. La particularidad es que, si se ingresa una cadena de longitud $n$, la función no realiza ningún hash, sino que actúa como la función identidad. Para cadenas de otras longitudes, se comporta como una función de hash criptográfica, similar a las que vimos en clase.

Si el objetivo de esta función es evitar colisiones únicamente para entradas de longitud $n$, entonces cumple su propósito. Sin embargo, no tiene mucho sentido al querer hashear algo, no hashearlo, ya que esto va en contra de los objetivos de las funciones de hash: por un lado, representar la misma cantidad de información en un tamaño menor, y por otro, garantizar que el hash no sea reversible. Al ser conocida la función de hash, un atacante podría fácilmente probar si la preimagen de ese hash no es igual al hash. Por lo tanto es fácil encontrar para todo hash una entrada que este mapeada a ese hash, va en contra de la propiedad de la resistencia a las coliciones.

Si el objetivo es evitar colisiones en general, este diseño no lo logra. Solo asegura que no haya colisiones entre entradas de longitud $n$, pero una entrada de longitud $n$ podría colisionar con el hash de una entrada de longitud distinta. Por lo tanto, la función no garantiza resistencia a colisiones fuera del caso particular de cadenas de tamaño $n$.

La función no va a cumplir tampoco que sea poco probable que dos entradas distintas mapeen a lo mismo, porque para cada entrada de tamaño distinto a N, esta va a compartir salida con su propio hash. 

Algunas propiedades que sí cumple la función propuesta son que, si para cadenas de tamaño distinto a $n$ está bien implementada, es deterministica y es rápida de computar.

# Punto 4
La idea de la demostración la saque principalmente de: [Fuente 1](https://scispace.com/pdf/attacks-on-some-rsa-signatures-4pe9259phw.pdf) (Sección 1.2) y [Fuente 2](https://www.reddit.com/r/crypto/comments/mzv1q6/can_someone_explain_an_existential_forgery_attack/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button).

Si el atacante tiene dos pares (mensaje, firma), como no usamos una función de hash para encriptar las firmas, el atacante va a tener los siguientes datos:

$$
s_1 = (m_1)^d \bmod N, \quad s_2 = (m_2)^d \bmod N \quad \text{(1)}
$$

Donde:

- $m_1$, $m_2$ son los mensajes, cuyos valores se conocen.
- $s_1, s_2$ son las firmas correspondientes, también conocidas.
- $d$ es la clave privada, valor *no* conocido por el atacante.
- $N$ es el módulo de RSA, su valor es conocido.

El atacante va a poder generar un nuevo par (mensaje, firma) haciéndose pasar por el autor legítimo.  

Definimos un nuevo mensaje y firma como:

$$
m_3 = (m_1 \times m_2) \quad \text{(2)}, \quad s_3 = (s_1 \times s_2) \quad \text{(3)}
$$

Recordando propiedades de congruencia, vale que:
$$
(a \times b)^e \equiv a^e \times b^e \pmod{N} \quad \text{(4)}
$$

Por lo tanto, podemos formar la siguiente cadena de congruencias:

$$
s_3^e \;\overset{(3)}{\equiv}\; (s_1 \times s_2)^e \;\overset{(4)}{\equiv}\; s_1^e \times s_2^e \;\overset{(1)}{\equiv}\; m_1 \times m_2 \;\overset{(2)}{\equiv}\; m_3 \pmod{N}
$$

Por lo tanto, es válido que, si alguien quiere verificar si la firma del mensaje es verídica, esta lo es porque cumple la ecuación $(s_3)^e \equiv m_3$ por la forma en la que los armamos.  

Por este motivo, es que no se usan firmas RSA planas, sino que se combinan con funciones de hash, y en vez de firmar el mensaje original, se firma un hash, porque los hashes *no* cumplen que $H(m_1) \times H(m_2) = H(m_1 \times m_2)$. Por lo tanto, por más que podamos encontrar valores que satisfagan la veracidad del mensaje, nunca vamos a poder encontrar un mensaje que hashee a ese hash que inventamos.

# Punto 5
 





