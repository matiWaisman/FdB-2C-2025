# Ejercicio 1  

## Punto A  

El propósito de una Public Key Infrastructure es incrementar la confianza entre los participantes de una red. Permite verificar y confiar en las firmas digitales emitidas por la entidad certificadora, garantizando la autenticidad de los mensajes y la identidad de los participantes.  

Tambien nos sirve para poder llegar a concenso con una mayor cantidad de participantes maliciosos. En la siguiente tabla comparo la cantidad de participantes maliciosos maximos que podemos tener para llegar al concenso si no tenemos setup vs si lo tenemos:
| Problema   | Máximo número de participantes maliciosos sin Setup | Máximo número de participantes maliciosos con Setup |
|-----------|----------------------------------------------------|---------------------------------------------------|
| Consenso  | $\frac{N}{3}-1$ | $\frac{N}{2}-1$ |
| Broadcast | $\frac{N}{3}-1$ | $N-1$ |

Por lo tanto en un problema de Broadcast, usando un setup con PKI podemos llegar a una solucion optima, porque el problema no se puede resolver si hay una cantidad de participantes maliciosos mayor o igual a la cantidad de participantes totales.

Otra ventaja importante de contar con un trusted setup es que la complejidad de los algoritmos de consenso se reduce significativamente: mientras que la solución clasica sin setup (BDDS87) tiene una complejidad de orden exponencial, con PKI la solución es mas simple y es de orden polinomial.

Si no tenemos un "ente confiable" que emite las firmas, tenemos el mismo problema que si no lo tuvieramos, ya que nunca podriamos saber, o confiar, si un mensaje firmado por un miembro realmente lo firmo el y no fue firmado por otro participante haciendose pasar por el. 

Esto podria suceder porque el encargado de las firmas es malicioso y le pudo haber proveido las claves privadas a miembros maliciosos, o porque es "malo" el algoritmo que usa para las claves por lo que es facil encontrar la clave privada de un usuario dada su clave publica y un mensaje firmado por el. 

## Punto B
En la red de Bitcoin, a diferencia de un sistema distribuido tradicional, los nodos no tienen identidades fijas ni están registrados. La red es dinamica, porque cualquiera puede entrar o salir libremente y no hay un listado de participantes.

Por este motivo, el protocolo de consenso de la blockchain corre sin depender de una PKI. En particular, no se usan firmas digitales para autenticar a los mineros, si no que se usa Proof Of Work para autenticar y generar concenso en el problema de quien va a ser el que agregue el bloque a la blockchain. 


Sin embargo, las transacciones sí emplean firmas digitales mediante ECDSA. Cuando un usuario crea una transacción, la firma con su clave privada, y los demás participantes pueden verificar la validez de esa firma usando la transaccion y la clave publica del usuario que firmo, para verificar que el fue el que hizo la transaccion.  

La diferencia clave con un esquema de PKI es que en Bitcoin no existe una autoridad central que emita o certifique las claves que se usan para firmar transacciones. Cada usuario genera su propio par de claves localmente.

Preguntar que onda lo del string publico del principio porque suena a PKI. 

# Ejercicio 2 
Bloque, S puntero al anterior. X datos del bloque y ctr es el numero de la PoW. 

# Ejercicio 3 
## Punto A
Mi idea es usar los prefijos de la cadena y quedandome siempre con la cabeza de ese prefijo para verificar la validez de los bloques del mas viejo al mas reciente. 
# Algorithm 1: The chain validation predicate

```python
def validate(C):
    b = V(x_C)  # validar los datos del contenido de la cadena de bloques C
    
    if b and (C != ε):  # la cadena no está vacía y es válida respecto a V
        s, x, ctr = head(C)
        s_prima = H(ctr, G(s, x))
        
        while C != ε and b:  # repetir hasta que la cadena quede vacía o falle la validación
            s, x, ctr = head(C)
            
            if validblock_q_T((s, x, ctr)) and (H(ctr, G(s, x)) == s_prima):
                s_prima = s   # conservar el hash
                C = C[1:]  # remover el head de C
            else:
                b = False
    
    return b
```
Asumo que tengo a disposicion la funcion `len(C)` que me devuelve el largo de la cadena, si no es facil de codearlo, es quedarse con la cabeza de la cadena hasta llegar a ε y en cada paso sumar uno.

En la posicion 1 esta el bloque genesis, entonces en cada paso tengo que chequear que el bloque 2 este apuntando al 1 agregandole el offset. 
```python
def validate_al_reves(C):
    b = V(x_C)  # validar los datos del contenido de la cadena de bloques C
    
    if b and (C != ε):  # la cadena no está vacía y es válida respecto a V
        s, x, ctr = head(C[:0]) # Me quedo con el mas viejo
        if validblock_q_T((s, x, ctr)): # Si el primer bloque es valido
            i = 1
            while i < len(C) and b:  # repetir hasta que la cadena quede vacía o falle la validación
                s_prima, x, ctr = head(C[:i]) # Me quedo con el siguiente al mas viejo
                
                if validblock_q_T((s_prima, x, ctr)) and (H(ctr, G(s_prima, x)) == s): # Chequeo que el bloque actual sea valido y que el actual apunte al anterior
                    s = s_prima   # conservar el hash
                else:
                    b = False
                i++
    return b
```


## Punto B
Una desventaja de comenzar la validación desde el primer bloque es que, en la práctica, los errores de validez son mucho más probables en los bloques más recientes. Por ejemplo, validar el bloque génesis no aporta demasiado, ya que siempre se asume correcto y es altamente improbable que allí esté el error. En consecuencia, este enfoque podría implicar un costo innecesario de verificación de muchos bloques “viejos” antes de detectar una invalidez en un bloque más reciente.

En términos de complejidad asintótica, ambos algoritmos tienen la misma cota superior (en el peor caso deben verificar toda la cadena). Sin embargo, en la práctica resulta más eficiente comenzar desde el bloque más reciente hacia atrás, ya que aumenta la probabilidad de encontrar un bloque inválido más temprano y, por lo tanto, reducir el trabajo total de verificación.