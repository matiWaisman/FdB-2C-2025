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
