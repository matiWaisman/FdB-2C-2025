# Ejercicio 1
## Punto A

El mecanismo que sigue un minero en la aplicación de Bitcoin para insertar transacciones en un bloque es el siguiente:

- En primer lugar, el minero recolecta las transacciones que considera válidas, utilizando predicados como `Validate`. En este proceso verifica, por ejemplo, que no existan casos de double spending, que los fondos a transferir realmente pertenezcan al emisor y que la firma de la transacción realizada con la clave privada corresponda con la clave pública del usuario. Además, por cada transacción calcula las fees que serán incorporadas en la coinbase transaction, como parte de su recompensa.

- Luego, crea un bloque con la secuencia de transacciones acumuladas, el cual apunta al último bloque minado de la cadena.

- A continuación, busca un `nonce` tal que `H(header) < T`, donde `T` es el target definido por la dificultad de minado de la época actual.

- Una vez que encuentra un `nonce` válido, el minero realiza un broadcast anunciando que ha agregado un nuevo bloque a la cadena. Los demás mineros verifican la validez de las transacciones y del bloque, y si todo es correcto, adoptan dicho bloque o la cadena más larga que haya sido comunicada. La decisión de qué bloque agregar depende del protocolo de consenso de Nakamoto. 

Seguir este protocolo garantiza las propiedades de Consistencia, asegurando que todos los mineros honestos compartan la misma ledger, y de Liveness, garantizando que todas las transacciones válidas eventualmente se incorporen a la ledger.

En la funcion validate tambien tiene que verificar que las firmas de las transacciones sean validas, esten los fondos disponibles para transferir de una cuenta a otra.
En la funcion input hay que tambien tener en cuenta el POW.
La funcion receive tambien tendria que recibir un parametro K, con el cual "filtra" las transacciones que todavia no estan establecidas en la cadena. Y solo devuelve las transacciones que estan establecidas hace mas de K rondas. 

## Punto B
La coinbase transaction tiene como propósito recompensar al minero que logra minar un bloque. A diferencia de una transacción típica, en la que los fondos se debitan de una dirección y se acreditan en otra, en una coinbase transaction no se requiere un input previo, ya que los fondos se crean para esta transaccion.

Esta transacción es siempre la primera dentro del conjunto de transacciones de un bloque, y transfiere la recompensa al minero que lo generó. Dicha recompensa está compuesta por un monto fijo determinado por el protocolo y las fees acumuladas de todas las transacciones incluidas en el bloque.


Fuente: https://www.geeksforgeeks.org/computer-networks/what-is-coinbase-transaction/

# Ejercicio 2 

## Punto A
En el problema de consenso para construir una blockchain, basándonos en las propiedades Common Prefix, Chain Quality y Chain Growth, es posible obtener una solución probabilística al problema de consenso que tolera hasta un tercio de participantes maliciosos.

El inconveniente de este enfoque es que la propiedad de Chain Quality no garantiza una cantidad suficiente de bloques generados por mineros honestos. 

Para mejorar esto, se introduce un nuevo protocolo de consenso capaz de soportar hasta la mitad de los participantes maliciosos.
La idea es extender el uso de las PoW no solo para el minado de bloques, sino también para el minado de transacciones.
En este caso con transacciones, no nos referimos a movimientos de monedas como en Bitcoin, si no a “votos” de un participante dentro del proceso de consenso.
De esta forma, la cantidad de votos válidos producidos por cada participante es proporcional a su poder de cómputo, garantizando que los nodos honestos conserven una representación justa dentro del sistema.

Sin embargo, este protocolo introduce un nuevo problema:
un adversario podría concentrar todo su poder de cómputo en una sola tarea (por ejemplo, únicamente en minar transacciones o únicamente en minar bloques), rompiendo el equilibrio esperado.

Para evitar esto surge la técnica de 2x1 PoW.
La idea principal es unificar el minado de bloques y de transacciones en un solo minado donde se relacionan los dos minados. Por eso estamos haciendo dos PoW en una. 
Esto se logra modificando la estructura de las pruebas, que pasan de ser tuplas (w, ctr) a triplas (w, ctr, label), donde label es un identificador que vincula el minado de bloques con el de transacciones.

## Punto B
La probabilidad de exito de acertarle al target de una PoW es de $P = t/2^{k}$. 

Como ahora no solo tenemos que resolver una PoW, si no que hay que hacer dos a la vez, $T_1$ y $T_2$. La probabilidad de acertar va a ser $P = P_{t_1} \times P_{t_2}$ que es igual a $P = \frac{t_1}{2^{k}} \times \frac{t_2}{2^{k}} = \frac{t_1 \times t_2}{2^{2k}}$. 

Si queremos que la probabilidad de exito de la 2x1 PoW sea igual a la de la PoW "normal" tenemos que igualar: 

$\frac{t_1 \times t_2}{2^{2k}} = \frac{T}{2^{k}} \iff t_1 \times t_2 = 2^{k} \times T$, siendo T el target del PoW original. 

## Punto C
La idea de abstraer la cantidad de Proof of Works que hacemos en 
𝑙, sirve para tener 𝑙 pruebas en paralelo de las distintas capas de la blockchain.

Por ejemplo, en la PoW 2x1 teníamos una prueba para los bloques y otra para las transacciones, en una 3x1 podríamos tener una tercera capa para las firmas digitales o para validar algún otro tipo de información.

Esto permite conectar las 𝑙 capas de consenso en una sola PoW, haciendo que todo el sistema avance sincronizado por el mismo esfuerzo computacional.

Tambien se tiene que sigue evitando que un atacante pueda concentrar su poder de minado en una sola capa, ya que todas las pruebas están unificadas.
De esta forma se logra mantener el equilibrio entre las distintas partes del protocolo (bloques, transacciones, firmas, etc.) y se mejora la eficiencia general del sistema.

Una 𝑙x1 PoW permitiría tener una blockchain paralela, donde varias tareas del consenso se resuelven al mismo tiempo con una sola PoW, combinando seguridad y rendimiento.

# Ejercicio 3
## Punto A
En strong consensus no basta con que termine, si no que el valor final si o si tiene que haber sido propuesto por un participante honesto. Si solo hay dos opciones de posibles outputs, se ve trivialmente que salvo los casos extremos siempre el resultado va a provenir de al menos un participante honesto. El tema es que pasa si tenemos mas de dos opciones. 

Para poder lograr esto, no basta con que la cantidad de participantes honestos sea mayor a la cantidad de participantes maliciosos, como exigia la asuncion de Honest Majority, si no que ahora necesitamos una mayor proporcion de participantes honestos versus maliciosos. 

Para poder llegar a un concenso fuerte, se necesita que la cantidad de participantes honestos sea igual a dos tercios de la cantidad de participantes totales. 

## Punto B
El lema de strong validity nos dice que si por el protocolo se llega a que un valor $v$ es el elegido, $v$ tiene que haber sido propuesto por un participante honesto. 

Sea $v$ el valor que deciden los participantes honestos, por la propiedad de Chain Quality sabemos que mas de dos tercios de los bloques del prefijo comun vienen de mineros honestos. Como en los bloques van a estar los input de los mineros que lo generaron, el valor mas frecuente en el prefijo comun va a ser el de los mineros honestos.

Por lo tanto el valor $v$ decidido por el protocolo, va a pertenecer a alguno de los inputs de un minero honesto. 

# Ejercicio 4  

El código y las pruebas del contrato fueron realizados junto a **Tiago Guerra**.

---

## Punto A  

Las variables internas que utilicé fueron:  
- Un **mapping** para registrar la cantidad de tokens que posee cada dirección.  
- Una variable interna para responder de manera más rápida cuántos tokens hay en circulación.  
- Otro **mapping** para almacenar el balance en Ether que tiene cada usuario al realizar el *withdraw*.  

Este último mapping se utiliza para respetar el patrón **Pull over Push** al momento de vender tokens.  
Además, declaré constantes para definir el **precio del token en wei**, el **nombre** y el **símbolo** del token.  

El proceso de compra de tokens consiste en enviar Ether al contrato.  
Esto ejecuta la función `receive`, que **mintea** tokens y los acredita al balance del usuario.  
Si luego el usuario desea venderlos, se le descuentan los tokens y se le acredita el correspondiente balance en Ether en el mapping.  

Un usuario puede acceder a su balance llamando a la función `balanceOf`, pasándole su dirección.  
También podría implementarse otra función llamada `balance` que llame internamente a `balanceOf(msg.sender)` para simplificar el acceso.

---

## Punto B  

- **Costo de deploy del contrato:** 1,162,812 gas.  
- Todas las funciones que son *getters* cuestan **0 gas**.  
- **Costo de enviar Ether para recibir tokens:** 70,824 gas.  
- **Costo de vender un token:** 56,903 gas.  
- **Costo de retirar fondos (withdraw):** 36,309 gas.  
- **Costo de transferir tokens:** 47,536 gas.  

---

## Punto C  

Una vulnerabilidad posible (más relacionada con el token que con el contrato) es que el **dueño del contrato (owner)** puede mintear tokens sin tener Ether de respaldo.  
Esto podría perjudicar a los usuarios, ya que el propietario podría crear tokens “gratis” sin el respaldo en Ether, **diluyendo o robando el valor** del resto de los tokens.

### Patrones de diseño utilizados

- **Checks–Effects–Interactions:**  
  Aplicado en la función `withdraw` para evitar ataques de *reentrancy*.  
  Primero se actualiza el estado (efecto) y luego se realiza la transferencia (interacción externa),  
  de modo que una llamada reentrante no pueda volver a retirar fondos.

- **Pull over Push:**  
  También utilizado para evitar ataques de *reentrancy*.  
  En lugar de enviar Ether directamente al usuario al vender, se acumula el saldo en un mapping y  
  el usuario debe retirarlo manualmente mediante la función `withdraw`.

---

## Punto D  

**Link al contrato y transacciones de prueba:**  
[https://sepolia.etherscan.io/address/0x9e9ecf0f439e3e9baf78346f78e99428c19d9984](https://sepolia.etherscan.io/address/0x9e9ecf0f439e3e9baf78346f78e99428c19d9984)

---

## Punto E  

```solidity
// SPDX-License-Identifier: GPL-3.0
pragma solidity >=0.7.0 <0.9.0;

contract PastelDePapa {
    address payable public addr_owner;
    mapping(address => uint256) private balance;
    mapping(address => uint256) private amountToWithdraw;
    uint256 public supply = 0;
    uint128 constant tokenValue = 600 wei;
    string constant name = "Pastel de Papa";
    string constant symbol = "$PDP";
    
    event Transfer(address indexed from, address indexed to, uint256 amount);
    event Mint(address indexed to, uint256 value);
    event Sell(address indexed from, uint256 value);
    event Withdrawal(address customer);

    constructor() {
        addr_owner = payable(msg.sender); 
    }

    function totalSupply() public view returns (uint256) {
        return supply; 
    }

    function balanceOf(address from) public view returns (uint256) {
        return balance[from];
    }   
    
    function getName() public pure returns (string memory) {
        return name;
    }  

    function getSymbol() public pure returns (string memory) {
        return symbol;
    }

    function getPrice() public pure returns (uint128) {
        return tokenValue;
    }
    
    function transfer(address to, uint256 value) public returns (bool) {
        require(balance[msg.sender] >= value, "Insufficient funds to transfer");

        balance[msg.sender] -= value;
        balance[to] += value;

        emit Transfer(msg.sender, to, value);
        return true;  
    }

    function mint(address to, uint256 value) public returns (bool) {
        require(msg.sender == addr_owner, "Only the owner can mint tokens");

        supply += value; 
        balance[to] += value;

        emit Mint(to, value);
        return true;
    }

    function sell(uint256 value) public returns (bool) {
        require(value > 0, "Cant sell less than zero Pasteles de Papa");
        require(balance[msg.sender] >= value, "Cant sell more tokens than you own");

        supply -= value; 
        balance[msg.sender] -= value;

        amountToWithdraw[msg.sender] += value * tokenValue;
        
        emit Sell(msg.sender, value);
        return true;
    }

    function withdraw() public {
        require(amountToWithdraw[msg.sender] > 0, "There is no balance to withdraw");
        uint256 b = amountToWithdraw[msg.sender];
        amountToWithdraw[msg.sender] = 0;
        payable(msg.sender).transfer(b);
        emit Withdrawal(msg.sender);
    }

    function close() public payable {
        require(msg.sender == addr_owner, "Only owner can close the contract");
        selfdestruct(addr_owner); // Está deprecado, pero de lo contrario habría que poner un condicional en todo el código.
    }

    receive() external payable {
        require(msg.value > 0, "Must send at least some wei");
        require(msg.value % tokenValue == 0, "Must send a multiple of 600 wei");
        mint(msg.sender, msg.value / tokenValue);
    }
}
