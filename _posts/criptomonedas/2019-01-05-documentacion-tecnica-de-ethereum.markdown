---
layout: post
title: Documentación Técnica de Ethereum
description: "Traducción del whitepaper de Ethereum"
categories: criptomonedas
---

La intención de Ethereum es crear un protocolo alternativo para desarrollar aplicaciones descentralizadas
al introducir un conjunto nuevo de aspectos que creemos serán de gran uso en una clase extensa de aplicaciones descentralizadas, sobretodo en situaciones
donde importan el tiempo de desarrollo, la seguridad para aplicaciones pequeñas y poco usadas
y la capacidad de interacción eficiente de diferentes aplicaciones. Ethereum logra esto construyendo lo que esencialmente es la mayor capa fundamental abstracta: una blockchain con un lenguaje Turing completo incorporado que le permite a cualquiera escribir contratos inteligentes y aplicaciones
descentralizadas donde se pueden crear reglas de propiedad propias y arbitrarias, formatos de transacción y funciones de transición de estado.
Se puede escribir una versión esencial de Namecoin en solo dos líneas de código y otros protocolos tales como divisas y sistemas de reputación en menos de veinte líneas.
También se pueden crear sobre la plataforma contratos inteligentes que son especies de  "cajas" criptográficas que contienen un valor y lo revelan solo si se cumplen ciertas condiciones.
Éstas tienen mucho más poder que las que ofrece el código de Bitcoin debido a las capacidades agregadas de la completitud de Turing, el reconocimiento del valor, de la blockchain y del estado.

### Filosofía

El diseño detrás de Ethereum debe seguir los siguientes principios:

1. **Simplicidad**: el protocolo Ethereum debe ser tan simple como sea posible, incluso sacrificando el amacenamiento de datos y la eficiencia de tiempo. Un programador promedio
debería ser capaz de seguir e implementar una especificación completa, dándose cuenta del potencial sin precedentes de democratización que las criptomonedas traen y extendiendo la visión
de Ethereum como un protocolo abierto a todos. Ninguna optimización que agregue complejidad se debería incluir a menos que esa optimización brinde beneficios considerables.

2. **Universalidad**: una parte fundamental de la filosofía de diseño es que Ethereum no tiene "características".
En vez de eso, Ethereum trae un lenguaje interno de codificación Turing completo que un programador puede usar
para crear cualquier contrato inteligente o tipo de transacción que puede ser definido matemáticamente.
¿Quiere crear su propio producto financiero? Con Ethereum se puede. ¿Quiere crear su propia moneda? Desarróllelo como un contrato en Ethereum. ¿Quiere montar un script de ejecución constante o Skynet completo?
Podría necesitar tener varios miles de contratos interconectados y asegurarse de llenarlos de información, pero también se puede en Ethereum.

3. **Modularidad**: las partes del protocolo Ethereum deben ser diseñados para ser tan modulares y separables como sea posible.
Durante la fase de desarrollo, nuestro objetivo es crear un programa en donde si se necesita hacer una pequeña modificación del protocolo en un lugar, el stack de la aplicación
seguirá funcionando sin más cambios. Las innovaciones como Ethash, los árboles modificados de Patricia y RLP (Recursive Length Prefix)
deberían y son implementados como bibliotecas separadas y completas. Este es el procedimiento para que ciertas características disponibles en Ethereum se puedan transferir a otros protocolos, incluso si Ethereum no las aplica. El desarrollo en Ethereum debería ser para beneficiar lo más posible al ecosistema de criptomonedas en general y no solo a Ethereum.

4. **Agilidad**: los detalles del protocolo Ethereum no son inflexibles. Aunque seremos extremadamente cuidadosos al modificar estructuras de alto nivel.
Un ejemplo es la hoja de ruta de partición (sharding roadmap) donde se abstrae la ejecución dependiendo de la disponibilidad de datos que cumplan con el consenso.
Los tests computacionales posteriores en el proceso de desarrollo nos pueden llevar a descubrir que ciertas modificaciones, por ejemplo, al protocolo de arquitectura de la Máquina Virtual de Ethereum (EVM), van a mejorar la escalabilidad o la seguridad. Si esas ocasiones se presentan, las vamos a aprovechar.

5. **No discriminación** y **no censura**: el protocolo no debería tratar de restringir o prevenir categorías específicas de uso. Todos los mecanismos regulatorios en el protocolo deberían ser diseñados para regular directamente el daño y no tratar de oponerse a aplicaciones específicas no deseadas. Un programador puede incluso ejecutar un bucle infinito encima de Ethereum mientras esté dispuesto a seguir pagando el costo de la transacción de cada instrucción de cómputo.

### Las cuentas de Ethereum

En Ethereum el estado se compone de objetos llamados "cuentas" cada una con una dirección de 20 bytes y con estados de transición que representan transferencias directas de valor e información entre cuentas. Una cuenta de Ethereum contiene cuatro campos:

* El número único (nonce) que se usa para asegurarse de que cada transacción se pueda procesar solo una vez.
* El balance actual de ether de la cuenta
* El código de contrato de la cuenta si existe
* El almacenamiento de la cuenta (vacío por defecto)

"Ether" es el principal criptocombustible de Ethereum y se usa para pagar costos de transacción. En general, hay dos tipos de cuentas: **cuentas de propiedad externa**, controladas por claves privadas y **cuentas de contrato** controladas por código de contrato. Una cuenta de propiedad externa no tiene código y se pueden enviar mensajes desde una de estas cuentas creando y firmando una transacción; en una cuenta de contrato, cada vez que se recibe un mensaje, el código se activa, lo que permite leer y escribir al almacenamiento interno y enviar otros mensajes o crear contratos a su vez.

Nótese que los "contratos" en Ethereum no deben verse como algo que debe ser "ejecutado" o "cumplido"; son más bien como "agentes autónomos" que viven dentro del ambiente de ejecución de Ethereum y siempre están ejecutando un segmento específico de código cuando son "despertados" por un mensaje o una transacción y además tienen control directo sobre su propio balance de ether y tienen su propio almacenamiento tipo clave/valor para registrar variables persistentes.

### Mensajes y Transacciones

El término "transacción" se usa en Ethereum para referirse a un paquede de datos firmado que guarda un mensaje que se va a enviar a una cuenta de propiedad externa. Las transacciones contienen:

* El destinatario del mensaje
* Una firma que identifica al remitente
* La cantidad de ether que se transfiere de remitente a destinatario
* Un campo de datos opcional
* un valor STARTGAS que representa el número máximo de instrucciones computacionales que la ejecución de la transacción tiene permitido hacer
* Un valor GASPRICE que representa pago que el remitente hace por cada instrucción computacional

Los primeros tres son campos estándar que se esperan en cualquier criptomoneda. El campo de datos no tiene una función por defecto, pero la máquina virtual tiene un código de operación que un contrato puede usar para acceder a los datos. Como un ejemplo de caso de uso, si un contrato está funcionando como servicio de registro de dominios basado en blockchain, entonces podría interpretar los datos que está recibiendo como compuestos de dos campos: el primer campo es el dominio que hay que registrar y el segundo campo es la dirección IP a la que hay que asociar. El contrato leería estos valores desde los datos del mensaje y los guardaría apropiadamente en el almacenamiento.

Los campos STARTGAS y GASPRICE son cruciales para el modelo antinegación de servicio (DDoS). de Ethereum. Para evitar bucles infinitos accidentales u hostiles o cualquier otro  desperdicio computacional en código, cada transacción necesita establecer un límite acerca de cuantas instrucciones computacionales de ejecución de código puede utilizar. La unidad fundamental de computación es el "gas"; generalmente una instrucción computacional cuasta 1 gas aunque ciertas operaciones tiene un mayor costo por ser más intensas computacionalmente o porque aumentan la cantidad de datos que debe ser almacenado como parte del estado. También hay un pago de 5 gas para cada byte en los datos de transacción. La intención del sistema de pagos es para exigir a un atacante que pague proporcionalmente a cada recurso que consume incuido computacón, ancho de banda y almacenamiento. De esta forma, cualquier transacción que lleve al consumo de una mayor cantidad de cualquiera de estos recursos debe pagar una comisión en gas justamente proporcional al ese incremento

### Mensajes

Los contratos tienen la capacidad de enviar "mensajes" a otros contratos. Los mensajes son objetos virtuales que nunca se serializan y solo existen en el ambiente de ejecución de Ethereum. Un mensaje contiene:

*  El remitente del mensaje (implícito)
*  El destinatario del mensaje
*  La cantidad de ether para transferir junto con el mensaje
*  Un campo de datos opcional
*  Un valor de STARTGAS

Básicamente un mensaje es como una transición, excepto que es producido por un contrato y no un actor central. Un mensaje se produce cuando el código actual en ejecución del contrato llama al código operacional `CALL` el cual crea y ejecuta un mensaje. Al igual que en una transacción, un mensaje causa que la cuenta del remitente ejecute su código. Así, los contratos pueden relacionarse con otros contratos de la misma forma que los actores externos lo hacen.

### La Función de Transición de Estado de Ethereum


![funcion de transicion estado de etehereum](https://raw.githubusercontent.com/ethereumbuilders/GitBook/master/en/vitalik-diagrams/ethertransition.png)

La función de transición de estado de Ethereum, `APPLY(S, TX) -> S'` se puede definir así:

1. Revisa si la transacción está bien formada (es decir, tiene el número correcto de valores), si la firma es válida y el nonce corresponde al nonce en la cuenta del remitente. De otra forma, devuelve un error.
2. Calcula la comisión de la transacción como `STARTGAS * GASPRICE`, y determina la dirección de remisión a partir de la firma. Sustrae la comisión desde el balance de la cuenta del remitente y aumenta el nonce del remitente. Si no hay balance suficiente para gastar, devuelve un error.
3. Inicia `GAS = STARTGAS` y retira cierta cantidad de gas por byte para pagar los bytes de la transacción.
4. Transfiere el valor de la transacción desde la cuenta del remitente a la cuenta del destinatario. Si la cuenta del destinatario no existe todavía, la crea. Si la cuenta del destinatario es un contrato, ejecuta el código del contrato hasta completarlo o hasta que la ejecución consuma todo el gas.
5. Si transferencia de valor falla porque el remitente carecía de suficiente dinero o porque la ejecución del código consumió todo el gas, revierte todos los cambios en el estado excepto el pago de las comisiones y agrega las comisiones a la cuenta del minero.
6. De otra forma, reembolsa las comisiones correspondientes a los gas que sobraron al remitente y envia las comisiones de los gas consumidos al minero.

Por ejemplo, supongamos que el código del contrato es:

{% highlight serpent %}
if !self.storage[calldataload(0)]:
    self.storage[calldataload(0)] = calldataload(32)
{% endhighlight %}

Hay que anotar que en realidad el código del contrato está escrito en código de EVM de bajo nivel; el ejemplo mostrado está escrito en Serpent, uno de nuestros lenguajes de alto nivel, a modo de ilustración, y pude ser compilado a código EVM. Suponga que el almacenamiento del contrato inicia vacío y se envía una transacción con un valor de 10 ether, 2000 gas, 0.0001 ether de gasprice y 64 bytes de datos con los bytes 0-31 que representan el número `2` y los bytes 32-63 que corresponden a la cadena `CHARLIE`. El proceso para la función de transición de estado es como sigue:

1. Revisa que la transacción es válida y bien formada.
2. Revisa que el remitente de la transacción posee por lo menos 2000 * 0.001 ether. Si es así, sustrae 2 ether de la cuenta del remitente.
3. Inicializa gas = 2000; al asumir que la transacción es de 170 bytes y que la comisión por byte es de 5, resta 850 de modo que quedan 1150 gas.
4. Retira 10 ether más de la cuenta del remitente y los agrega a la cuenta del contrato.
5. Ejecuta el código. En este caso, es muy simple: revisa si se usa el almacenamiento del contrato en el índice 2, nota que no es así, así que asigna al almacenamiento en el índice 2 el valor `CHARLIE`. Suponga que esto toma 187 gas. El resto del gas es 1150 - 187 = 963.
6. Regresa 963*0.001 ether a la cuenta del remitente y devuelve el estado resultante.

Nótese que los mensajes funcionan de forma equivalente a transacciones en términos de capacidad de revertir: si la ejecución de un mensaje agota el gas, entonces esa ejecución y todas las ejecuciones desencadenadas por ella se revierten, pero las ejecucuines anteriores no tienen por que hacer esto. Esto quiere decir que es "seguro" para un contrato llamar a otro contrato debido a que si A invoca a B con G cantidad de gas, entonces se garantiza que la ejecución de A pierde a lo mucho G gas. Finalmente, nótese que hay un código operacional, `CREATE`, que que crea un contrato; la mecánica de su ejecución es generalemnte similar a `CALL` con la excepción de que la salida de la ejecución determina el código de un contrato recién creado.

### Ejecución de Código

El código en los contratos Ethereum está escrito en un lenguaje bytecode de bajo nivel basado en stacks, conocido como "código de máquina virtual de Ethereum" o "código EVM". El código compone de una serie de bytes donde cada byte representa una operación. En general, la ejecución de código es un bucle infinito que consiste en llevar a cabo de forma repetitiva la operación en el contador actual del programa (que comienza en cero) y luego incrementando este contador unitariamente hasta alcanzar el final del código o hasta detectar una instrucción `STOP` o `RETURN`. Las operaciones tienen acceso a a tres tipos de espacio en donde almacenar los datos:

* El **stack**, un contenedor de tipo entra-último-sale-primero donde se ingresan y se retiran los valores
* La **memoria**, una arreglo de bytes que se puede expandir infinitamente
* El **almacenamiento** a largo plazo del contrato de tipo clave/valor. A diferencia del stack y la memoria que se reinician cuando se termina el cálculo, el almacenamiento persiste a largo plazo.

El código pude acceder al valor, remitente y datos del mensaje entrante, al igual que a los datos de la cabecera del bloque, y el código puede también retornar un arreglo de bytes de datos como salida.

El modelo de ejecución formal del código EVM es sorprendentemente simple. Mientras la máquina virtual Ethereum está corriendo, su estado computacional completo se puede definir por la tupla `(block_state, transaction, message, code, memory, stack, pc, gas)`; aquí `block_state` es el estado global que contiene todas las cuentas, incluyendo los balances y el almacenamiento. Al comienzo de cada ronda de ejecución, la instrucción actual se encuentra tomando el byte `pc`-ésimo del `code` (ó 0 si `pc >= len(code)`), y cada instrucción tiene su propia definición de acuerdo a su efecto en la tupla. Por ejemplo, `ADD` retira dos elementos del stack e ingresa su suma, reduce el `gas` en una unidad e incrementa `pc` en una unidad, luego `SSTORE` retira dos elementos del stack e inserta el segundo objeto en el almacenamiento del contrato en el índice especificado por el primer objeto. Aunque hay muchas formas de optimizar la ejecución de la máquina virtual de Ethereum a través de compilación *just-in-time*, se puede hacer una implementación básica de Ethereum en unos cuantos cientos de líneas de código.

### Blockchain y Minería

![diagrama de bloques](https://raw.githubusercontent.com/ethereumbuilders/GitBook/master/en/vitalik-diagrams/apply_block_diagram.png)

La blockchain de Ethereum es en esencia muy similar al de Bitcoin, aunque tiene algunas diferencias. La principal diferencia entre Ethereum y Bitcoin con respecto a la arquitectura blockchain es que a diferencia de Bitcoin (que sólo contiene una copia de la lista de transacciones), los bloques de Ethereum contienen una copia tanto de la lista de transacciones como del estado más reciente. Aparte de eso, otros dos valores, el número del bloque y la dificultad, también se almacenan en el bloque. El algoritmo básico de validación de Ethereum es así:

1. Revisar si el bloque previo referenciado existe y es válido.
2. Revisar que la marca cronológica del bloque es mayor que la del bloque previo referenciado y menor que 15 minutos en el futuro.
3. Revisar que el número del bloque, la dificultad, la raíz de la transacción, la raíz "tío" y el límite de gas (conceptos de bajo nivel específicos a Ethereum) son válidos.
4. Revisar que la prueba de trabajo en el bloque es válido.
5. Asignar a `S[0]` el estado al final del bloque anterior.
6. Asignar a `TX` la lista de transacciones del bloque, con `n` transacciones. Para todo `i` en `0...n-1`, establecer `S[i+1] = APPLY(S[i], TX[i])`. Si alguna aplicación retorna un error, o si el gas total consumido en el bloque hasta este punto excede el `GASLIMIT`, retornar un error.
7. Asignar a `S_FINAL` el valor de `S[n]` pero agregando la recompensa del bloque al minero.
8. Revisar si la raíz del árbol de Merkle del estado `S_FINAL` es igual a la raíz del estado final que aparece en la cabecera del bloque. Si es así, el bloque es válido; de otra forma, no es válido. 