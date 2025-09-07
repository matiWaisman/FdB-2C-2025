# Lecture 2 

## Blind RSA Signature 
Bob le manda a alice para que firme pero Alice no sabe que es lo que esta firmando. Oculta al mensaje de alguna forma Alice lo firma, Bob lo desoculta pero la firma sigue siendo valida. 

## Proof Of Work
Uno va a tener que invertir esfuerzo en hacer algo y el esfuerzo significa tener que trabajar, estas demostraciones de trabajo van a estar basadas en busquedas exhaustivas. Va a tener que buscar una solucion de un espacio de soluciones. 

Las moderatly hard functions son faciles de calcular de un lado, y moderadamente dificil para el otro y imposible pasar de un lado a otro. 

Va a tener un tipo que demuestra el esfuerzo y uno que verifica que el esfuerzo se hizo. El que verifica tiene mucho menos trabajo. 
Ejemplo multiplicar puede ser dificil pero verificar que una multiplicacion esta bien hecha es mucho mas facil. La busqueda de la solucion del desafio puede ser exponencial o un polinomio muy grande. 

La verificacion del predicado es muy facil y las formas que vamos a usar estas verificaciones de trabajo no es facil de conseguir, la probabilidad de que uno lo pueda conseguir va a ser muy chica. Esto va a ser necesario para que las blockchains pasadas en PoW funcionen. La idea es que si es muy dificil nadie va a hacer nada, no hay progreso y si es muy facil va a ser un lio y todos lo resuelven y no vamos a saber quien gano. 

Si ponemos un Target muy chico va a ser muy dificil encontrar argumentos, pero si es muy grande cualquier valor del rango va a estar bien. 

Una vez que tienen un valor los mineros van a hacer una busqueda exhaustiva hasta encontrar un testigo. 

Como la funcion de hash es deterministica y para la misma entrada te da la misma salida por eso la verificacion es muy facil. 

Se va cambiando el target dependiendo de la epoca. 

Los mineros van a competir de forma justa, porque es independiente el proceso de busqueda, como el de la moneda. 

Otros PoW son de demostrar que recorri toda la memoria que va a insumir tiempo porque no va a estar en la memoria si tengo que hacer entrada y salida y pasar por disco. 

Nonce generador numero aleatorio. 

Por lo general en bitcoin los mining pools suele ser un solo nodo pero que detras hay muchas maquinas corriendo un pool. 

## Bitcoin in practice
Todos los bloques a partir de un punto son inmutables. 

Los mineros van a reemplazar el banco y van a ser los que mantienen el sistema. Mantienen el registro para ser recompensados. 

En BTC los nodos no se conocen entre ellos, los nodos pueden entrar y salir y no hay ninguna restriccion fuera de la capacidad computacional, para participar. 

Los ataques sybil si no hay autenticacion cualquier participante puede pretender ser otro participante, eso no puede pasar por mas que no se conocen entre si los participantes. 

Se puede tener distintas direcciones publicas para cada transaccion. 

Las testnets son muy chicas para jugar. No requieren mucho trabajo. Las monedas no tienen valor real si no que son ficticias. 