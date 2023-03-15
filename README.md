[![1](https://github.com/rozoandrescamilo/Smart-Contract-para-consultar-estados-de-una-Purchase-Order/blob/main/img/1.jpg?raw=true "1")](https://github.com/Smart-Contract-para-consultar-estados-de-una-Purchase-Order/blob/main/img/1.jpg?raw=true "1")

- [BLCH-3v1](#blch-3v1)
  - [Inicio rápido](#inicio-rápido)
    - [Elegir tipo de entorno de trabajo](#elegir-tipo-de-entorno-de-trabajo)
  - [Desplegar recursos de FireFly](#desplegar-recursos-de-fireFly)
    - [Configuración del Supernodo](#configuración-del-supernodo)
    - [Inicializar FireFly](#inicializar-fireFly) 
  - [Despliege de smart contract en la API](#despliege-de-smart-contract-en-la-api)
    - [Creación y despliegue de nuevo smart contract](#creación-y-despliegue-de-nuevo-smart-contract)
    - [Gestión de smart contract con REST API Gateway](#gestión-de-smart-contract-con-rest-api-gateway)
  - [Gestión del flujo del contrato y documentación de REST API con POSTMAN](#gestión-del-flujo-del-contrato-y-documentación-de-rest-api-con-postman)
    - [Crear smart crontract con el POST/constructor()](#crear-smart-crontract-con-el-post/constructor())
    - [Registrar una nueva Purchase Order con POST/{address}/registerPO](#registrar-una-nueva-purchase-order-con-post/{address}/registerpo)
    - [Validar estado de Purchase Order con POST/{address}/POvalidator](#validar-estado-de-purchase-order-con-post/{address}/povalidator)
    - [Registrar paso a siguiente estado de la Purchase Order con POST/{address}/registerStep](#registrar-paso-a-siguiente-estado-de-la-purchase-order-con-post/{address}/registerstep)

# BLCH-3v1

En este repositorio se registra la documentación necesaria para la integración con Frontend por medio de POSTMAN, del smart contract que permite consultar los diferentes estados estados de una Orden de Compra que se encuentra desplegado en la API de Kaleido. 

El funcionamiento del contrato se basa en el flujo de órdenes de compra representando en el siguiente diagrama, muestra el proceso de negocios en el que se describe la interacción entre diferentes actores en la cadena de suministro, desde cuando un cliente realiza una solicitud de compra, la cual es enviada al proveedor. El proveedor revisa la solicitud y envía una confirmación de despacho completo del pedido a Dawipo, si son entregas parciales se debe tener autorización del cliente. Finalmente, el proveedor entrega los productos y emite un Booking Request.

[![35](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/FlujoOC.png "35")](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/FlujoOC.png "35")

Por lo tanto, el contrato inteligente puede verificar si cada etapa se ha completado correctamente y actualizar el estado de la orden de compra en consecuencia. Esto permite que todo el proceso se gestione de manera segura, ya que el contrato inteligente está diseñado para garantizar que todas las partes involucradas en el proceso cumplan con sus obligaciones.

## Inicio rápido

Se debe visitar la web:[https://console.kaleido.io](https://console.kaleido.io "https://console.kaleido.io"), Crear una cuenta y elegir uno de los diferentes planes..

Para este proceso se crea una cuenta tipo Starter con el fin de realizar pruebas de la herramienta, en caso de elegir Kaleido será mejor opción de cuenta Enterprise que tiene mejores beneficios:

[![36](https://github.com/rozoandrescamilo/Contrato-inteligente-en-la-interfaz-FireFly-de-Kaleido/blob/main/img/1.png?raw=true "36")](https://github.com/Contrato-inteligente-en-la-interfaz-FireFly-de-Kaleido/blob/main/img/36.png?raw=true "36")

Precios de los diferentes planes: 
[https://www.kaleido.io/pricing](https://www.kaleido.io/pricing "https://www.kaleido.io/pricing")

[![37](https://github.com/rozoandrescamilo/Contrato-inteligente-en-la-interfaz-FireFly-de-Kaleido/blob/main/img/2.png?raw=true "37")](https://github.com/Contrato-inteligente-en-la-interfaz-FireFly-de-Kaleido/blob/main/img/2.png?raw=true "37")

Después de elegir el plan se debe Crear una red para construir la primera red blockchain. Proporcionar un nombre para la red comercial y seleccionar una región para conectar con los servicios en la nube.

[![38](https://github.com/rozoandrescamilo/Contrato-inteligente-en-la-interfaz-FireFly-de-Kaleido/blob/main/img/3.png?raw=true "38")](https://github.com/Contrato-inteligente-en-la-interfaz-FireFly-de-Kaleido/blob/main/img/3.png?raw=true "38")

### Elegir tipo de entorno de trabajo

Kaleido ofrece dos tipos de entorno de nivel superior: Servicio estándar de Blockchain y Servicio estándar de Blockchain + FireFly SuperNode. Se elige el Servicio estándar de Blockchain + FireFly SuperNode:

[![39](https://github.com/rozoandrescamilo/Contrato-inteligente-en-la-interfaz-FireFly-de-Kaleido/blob/main/img/4.png?raw=true "39")](https://github.com/Contrato-inteligente-en-la-interfaz-FireFly-de-Kaleido/blob/main/img/4.png?raw=true "39")

Actualmente los únicos protocolos de blockchain compatibles para entornos FireFly son Ethereum e Hyperledger Fabric. Asi que se trabajará con la red de Ethereum.

[![40](https://github.com/rozoandrescamilo/Contrato-inteligente-en-la-interfaz-FireFly-de-Kaleido/blob/main/img/5.png?raw=true "40")](https://github.com/Contrato-inteligente-en-la-interfaz-FireFly-de-Kaleido/blob/main/img/5.png?raw=true "40")

A continuación, se elige el cliente Ethereum blockchain subyacente y un algoritmo asociado. Las opciones son Geth / PoA, Quorum / IBFT, Quorum / Raft, Besu / PoA y Besu / IBFT. Se recomienda una configuración simple de Geth / PoA.

[![41](https://github.com/rozoandrescamilo/Contrato-inteligente-en-la-interfaz-FireFly-de-Kaleido/blob/main/img/6.png?raw=true "41")](https://github.com/Contrato-inteligente-en-la-interfaz-FireFly-de-Kaleido/blob/main/img/6.png?raw=true "41")

[![42](https://github.com/rozoandrescamilo/Contrato-inteligente-en-la-interfaz-FireFly-de-Kaleido/blob/main/img/7.png?raw=true "42")](https://github.com/Contrato-inteligente-en-la-interfaz-FireFly-de-Kaleido/blob/main/img/7.png?raw=true "42")

## Desplegar recursos de FireFly

### Configuración del Supernodo

Hacer clic en el botón CREAR SUPERNODO para comenzar a configurar el primer FireFly Supernode.

[![43](https://github.com/rozoandrescamilo/Contrato-inteligente-en-la-interfaz-FireFly-de-Kaleido/blob/main/img/8.png?raw=true "43")](https://github.com/Contrato-inteligente-en-la-interfaz-FireFly-de-Kaleido/blob/main/img/8.png?raw=true "43")

Se debe elegir un nombre y membresía para el nodo. La membresía predeterminada está disponible para su uso en esta etapa.
En el siguiente ejemplo n1 está obligado a Sample Organization membresía (incumplimiento).

[![44](https://github.com/rozoandrescamilo/Contrato-inteligente-en-la-interfaz-FireFly-de-Kaleido/blob/main/img/9.png?raw=true "44")](https://github.com/Contrato-inteligente-en-la-interfaz-FireFly-de-Kaleido/blob/main/img/9.png?raw=true "44")

A continuación, asignará su nodo FireFly a un nodo blockchain nuevo o existente y un tiempo de ejecución ipfs nuevo o existente.
Dado el contexto de este inicio rápido, se creara un nuevo elemento para cada uno.

[![45](https://github.com/rozoandrescamilo/Contrato-inteligente-en-la-interfaz-FireFly-de-Kaleido/blob/main/img/10.png?raw=true "45")](https://github.com/Contrato-inteligente-en-la-interfaz-FireFly-de-Kaleido/blob/main/img/10.png?raw=true "45")

Actualmente solo están disponibles nodos FireFly de pequeño tamaño por el tipo de cuenta.

[![46](https://github.com/rozoandrescamilo/Contrato-inteligente-en-la-interfaz-FireFly-de-Kaleido/blob/main/img/11.png?raw=true "46")](https://github.com/Contrato-inteligente-en-la-interfaz-FireFly-de-Kaleido/blob/main/img/11.png?raw=true "46")

### Inicializar FireFly

Cuando los tres tiempos de ejecución hacen la transición a un Started se podrá Inicializar el Nodo FireFly que se ha creado.

Detrás de escena, Kaleido te está generando un espacio de nombres de aplicaciones, compilar el contrato inteligente central de FireFly ( ya que FireFly requiere un contrato inteligente implementado para sincronizar la actividad con la cadena ) y promover la fuente en su entorno FireFly recién creado.

[![47](https://github.com/rozoandrescamilo/Contrato-inteligente-en-la-interfaz-FireFly-de-Kaleido/blob/main/img/12.png?raw=true "47")](https://github.com/Contrato-inteligente-en-la-interfaz-FireFly-de-Kaleido/blob/main/img/12.png?raw=true "47")

Si el nodo FireFly se inicializa correctamente, se verá una pantalla de descripción general del entorno con todos los tiempos de ejecución en un estado Iniciado.

[![48](https://github.com/rozoandrescamilo/Contrato-inteligente-en-la-interfaz-FireFly-de-Kaleido/blob/main/img/13.png?raw=true "48")](https://github.com/Contrato-inteligente-en-la-interfaz-FireFly-de-Kaleido/blob/main/img/13.png?raw=true "48")

[![49](https://github.com/rozoandrescamilo/Contrato-inteligente-en-la-interfaz-FireFly-de-Kaleido/blob/main/img/14.png?raw=true "49")](https://github.com/Contrato-inteligente-en-la-interfaz-FireFly-de-Kaleido/blob/main/img/14.png?raw=true "49")

## Despliege de smart contract en la API

### Creación y despliegue de nuevo smart contract 

Como requisito es necesario tener creada una App para la gestión de contratos empresariales en sección Apps de la consola de Kaleido.

Dentro de la App creada se inicia un nuevo smart contract, para este caso se importa el archivo del contrato desde una carpeta local:

[![1](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/1.png?raw=true "1")](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/1.png?raw=true "1")

Luego, se deben ingresar los detalles del proyecto como nombre, versión y seleccionar el archivo del contranto.

Para este proceso se utiliza el contrato <POstatusValidator.sol>, un contrato inteligente que tiene como fin crear un método para registrar y consultar los diferentes estados posibles de una Purchase Order desde TO BE CONFIRMED hasta BOOKING REQUEST. Se recomienda ver el funcionamiento de este contrato para entender de mejor manera: [https://github.com/rozoandrescamilo/Smart-Contract-para-consultar-estados-de-una-Purchase-Order](https://github.com/rozoandrescamilo/Smart-Contract-para-consultar-estados-de-una-Purchase-Order "https://github.com/rozoandrescamilo/Smart-Contract-para-consultar-estados-de-una-Purchase-Order")

[![2](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/2.png?raw=true "2")](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/2.png?raw=true "2")

Creada la nueva instancia del contrato se adiciona la Gateway API que es una consola Swagger para la gestión del contrato y transacciones.

[![3](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/3.png?raw=true "3")](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/3.png?raw=true "3")

Una vez finalizado el ingreso de detalles se mostrará los parametros de la versión y el contrato debe pasar de status Compiling a Compiled para poder continuar.

[![4](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/4.png?raw=true "4")](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/4.png?raw=true "4")

Después se mostrará detalles del Smart Contract donde se podran realizar varias actividades con API GATEWAY:

- Crear una API de puerta de enlace: crea una consola swagger dinámica y una especificación descargable para la interacción RESTful con métodos de contrato inteligente.

- Implementar contratos: use la API de Gateway para implementar su contrato inteligente mediante programación o directamente a través de la consola.

- Enviar transacciones: Aproveche la API para interactuar con contratos inteligentes en cadena existentes al pasar la dirección del contrato como parámetro en la llamada.

[![5](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/5.png?raw=true "5")](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/5.png?raw=true "5")

### Gestión de smart contract con REST API Gateway

La interfaz de usuario Swagger incorporada se despliega dando clic en el boton VIEW GATEWAY API y se pueden visualizar los servicios web de acceso y gestión de datos de organizaciones y proyectos a través de REST APIs.

[![6](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/6.png?raw=true "6")](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/6.png?raw=true "6")

En el **POST/constructor()** se debe agregar el archivo ABI generado en la compilación del contrato y hacer clic en EJECUTAR.

[![7](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/7.png?raw=true "7")](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/7.png?raw=true "7")

ABI del contrato compilado:

```javascript
{
  "input": {
    "abi": [
      {
        "anonymous": false,
        "inputs": [
          {
            "indexed": false,
            "internalType": "uint256",
            "name": "POID",
            "type": "uint256"
          },
          {
            "indexed": false,
            "internalType": "uint256",
            "name": "poType",
            "type": "uint256"
          },
          {
            "indexed": false,
            "internalType": "enum StatusValidator.Status",
            "name": "status",
            "type": "uint8"
          },
          {
            "indexed": false,
            "internalType": "string",
            "name": "metadata",
            "type": "string"
          },
          {
            "indexed": false,
            "internalType": "address",
            "name": "author",
            "type": "address"
          }
        ],
        "name": "RegisteredStep",
        "type": "event"
      },
      {
        "inputs": [
          {
            "internalType": "uint256",
            "name": "",
            "type": "uint256"
          },
          {
            "internalType": "uint256",
            "name": "",
            "type": "uint256"
          }
        ],
        "name": "POvalidator",
        "outputs": [
          {
            "internalType": "enum StatusValidator.Status",
            "name": "status",
            "type": "uint8"
          },
          {
            "internalType": "string",
            "name": "metadata",
            "type": "string"
          }
        ],
        "stateMutability": "view",
        "type": "function"
      },
      {
        "inputs": [
          {
            "internalType": "uint256",
            "name": "POID",
            "type": "uint256"
          }
        ],
        "name": "registerPO",
        "outputs": [
          {
            "internalType": "bool",
            "name": "success",
            "type": "bool"
          }
        ],
        "stateMutability": "nonpayable",
        "type": "function"
      },
      {
        "inputs": [
          {
            "internalType": "uint256",
            "name": "POID",
            "type": "uint256"
          },
          {
            "internalType": "string",
            "name": "metadata",
            "type": "string"
          },
          {
            "internalType": "uint256",
            "name": "poType",
            "type": "uint256"
          }
        ],
        "name": "registerStep",
        "outputs": [
          {
            "internalType": "bool",
            "name": "success",
            "type": "bool"
          }
        ],
        "stateMutability": "nonpayable",
        "type": "function"
      }
    ]
  }
}

```

Como respuesta se obtiene un archivo JSON donde se realiza una solicitud POST usando la API de Kaleido. Esta solicitud se utiliza para crear una nueva puerta de enlace en la red Kaleido. El contenido del archivo JSON incluye información sobre los parámetros de la solicitud, tales como la dirección de la cuenta de la que se va a realizar la solicitud, la ABI de la aplicación, entre otros.

[![8](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/8.png?raw=true "8")](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/8.png?raw=true "8")

## Gestión del flujo del contrato y documentación de REST API con POSTMAN

Para generar la documentación necesaria para la integración con Frontend, se utiliza la aplicación de escritorio POSTMAN que es una herramienta para el desarrollo de APIs que facilita el diseño, el testing y el monitoreo de servicios REST. Sirve para probar endpoints de aplicaciones web basadas en APIs o REST. Con POSTMAN, los desarrolladores pueden configurar, enviar y guardar solicitudes HTTP y ver sus respuestas. Esto les permite hacer pruebas más rápidas y los ayuda a comprender mejor sus APIs y cómo interactúan con otros sistemas. Postman también ayuda a los desarrolladores a documentar mejor sus APIs, lo que facilita su uso para otros desarrolladores.

Se selecciona el boton SEND A REQUEST para comenzar la interacción.

[![9](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/9.png?raw=true "9")](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/9.png?raw=true "9")

### Crear smart crontract con el POST/constructor()

Teniendo en cuenta el archivo JSON de respuesta por ejecutar el ABI en la REST API de Kaleido, se debe extraer la URL del Curl: "https://u0cqnbwz76-u0r2m5xw62-connect.us0-aws.kaleido.io/gateways/u0vl2r5dli/?kld-from=0x527e08ea398d607e898eb39176894f1815bfeb0b&kld-sync=true" y agregarla como un llamado POST en POSTMAN. Al ingresar el link reconoce parámetros de la API en la pestaña **Params.** 

[![50](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/50.png?raw=true "50")](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/50.png?raw=true "50")

[![10](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/10.png?raw=true "10")](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/10.png?raw=true "10")

Luego, en la pestaña **Headers** se debe agregar la Key "Authorization" y como valor "Basic dTB3anJrZzRjZTpfUHRZdDBkWWFSenZ6MDBnNU01d1p1WU13M2xMUFdhOHJMZzQwMEU1SXJ3" que se encuentra en el archivo JSON de salida por ejecutar el ABI en la REST API de Kaleido.

[![11](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/11.png?raw=true "11")](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/11.png?raw=true "11")

En la pestaña de **Body** se selecciona el tipo Raw ya que es texto plano y el archivo de tipo JSON, dentro de este se debe pegar el archivo ABI de la compilación del contrato. Una vez realizada esta configuración ya es posible dar clic en **Send** para enviar la solicitud a la API.

[![12](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/12.png?raw=true "12")](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/12.png?raw=true "12")

Se obtendra como output una respuesta del servidor similar a la de API REST de Kaliedo, donde se pueden ver parámetros de la transacción realizada, que le pueden ser de interés al usuario a lo largo de todo el flujo del proceso en la gestión de datos. Algunos de los términos relevantes en la respuesta incluyen:

- **contractAddress:** se refiere a la dirección del contrato inteligente utilizado en la transacción.

- **gasUsed y cumulativeGasUsed:** se refieren a la cantidad de gas utilizada en la transacción. El gas es una unidad de medida utilizada en Ethereum (la plataforma blockchain en la que se basa Kaleido) para calcular el costo de las transacciones.

- **transactionHash:** se refiere al hash único de la transacción en la cadena de bloques.

- **blockHash:** hace referencia al algoritmo que representa la dirección del bloque utilizado en la transacción.

- **blockNumber:** número del bloque utilizado en la transacción.

[![13](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/13.png?raw=true "13")](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/13.png?raw=true "13")

Respuesta de POSTMAN:

```javascript
{
    "headers": {
        "id": "66c2c2be-55a6-4ca0-6b72-d89286966c07",
        "type": "TransactionSuccess",
        "ctx": {
            "isRemoteRegistry": true
        },
        "timeReceived": "2023-03-10T20:18:55.471053182Z",
        "timeElapsed": 6.597767506,
        "requestOffset": "",
        "requestId": "ef09121a-d44c-41b3-5f45-94e7bc3b88e4",
        "requestABIId": "c01ffc26-65f1-4cbc-4893-170aa50cf4d6"
    },
    "blockHash": "0xafc33b7e6d0d194de477df06aa2c8a96c363f01f8f8b13de7af0fdf4aecb41e5",
    "blockNumber": "196838",
    "openapi": "https://u0cqnbwz76-u0r2m5xw62-connect.us0-aws.kaleido.io/instances/b565ffbf2c21420b439be27ddcac47ba4ecd3cd3?openapi",
    "apiexerciser": "https://u0cqnbwz76-u0r2m5xw62-connect.us0-aws.kaleido.io/instances/b565ffbf2c21420b439be27ddcac47ba4ecd3cd3?ui",
    "contractAddress": "0xb565ffbf2c21420b439be27ddcac47ba4ecd3cd3",
    "cumulativeGasUsed": "1110857",
    "from": "0x527e08ea398d607e898eb39176894f1815bfeb0b",
    "gasUsed": "1110857",
    "nonce": "0",
    "status": "1",
    "to": null,
    "transactionHash": "0x62496acc44e900b90c72f5f2ed05e5c9f2605d6d5d769ac0cc2104129e8d8c5b",
    "transactionIndex": "0"
}

```

En la parte derecha de POSTMAN en la pestaña **</>** se puede obtener el fragmento de código resultante, que servirá como documentación necesaria para la integración con el equipo de Frontend. Este fragmento es un archivo JSON con un formato de intercambio de datos utilizado para transmitir información en formato de texto entre diferentes aplicaciones. En este caso, el archivo JSON se está utilizando para realizar una solicitud POST usando la API de Kaleido. Esta solicitud se utiliza para crear una nueva puerta de enlace en la red Kaleido. El contenido del archivo JSON incluye información sobre los parámetros de la solicitud, tales como la dirección de la cuenta de la que se va a realizar la solicitud, la ABI de la aplicación, entre otros.

[![14](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/14.png?raw=true "14")](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/14.png?raw=true "14")

Código JSON para documentación:

```javascript
curl --location 'https://u0cqnbwz76-u0r2m5xw62-connect.us0-aws.kaleido.io/gateways/u0vl2r5dli/?kld-from=0x527e08ea398d607e898eb39176894f1815bfeb0b&kld-sync=true' \
--header 'Authorization: Basic dTB3anJrZzRjZTpfUHRZdDBkWWFSenZ6MDBnNU01d1p1WU13M2xMUFdhOHJMZzQwMEU1SXJ3' \
--header 'Content-Type: application/json' \
--data '{
  "input": {
    "abi": [
      {
        "anonymous": false,
        "inputs": [
          {
            "indexed": false,
            "internalType": "uint256",
            "name": "POID",
            "type": "uint256"
          },
          {
            "indexed": false,
            "internalType": "uint256",
            "name": "poType",
            "type": "uint256"
          },
          {
            "indexed": false,
            "internalType": "enum StatusValidator.Status",
            "name": "status",
            "type": "uint8"
          },
          {
            "indexed": false,
            "internalType": "string",
            "name": "metadata",
            "type": "string"
          },
          {
            "indexed": false,
            "internalType": "address",
            "name": "author",
            "type": "address"
          }
        ],
        "name": "RegisteredStep",
        "type": "event"
      },
      {
        "inputs": [
          {
            "internalType": "uint256",
            "name": "",
            "type": "uint256"
          },
          {
            "internalType": "uint256",
            "name": "",
            "type": "uint256"
          }
        ],
        "name": "POvalidator",
        "outputs": [
          {
            "internalType": "enum StatusValidator.Status",
            "name": "status",
            "type": "uint8"
          },
          {
            "internalType": "string",
            "name": "metadata",
            "type": "string"
          }
        ],
        "stateMutability": "view",
        "type": "function"
      },
      {
        "inputs": [
          {
            "internalType": "uint256",
            "name": "POID",
            "type": "uint256"
          }
        ],
        "name": "registerPO",
        "outputs": [
          {
            "internalType": "bool",
            "name": "success",
            "type": "bool"
          }
        ],
        "stateMutability": "nonpayable",
        "type": "function"
      },
      {
        "inputs": [
          {
            "internalType": "uint256",
            "name": "POID",
            "type": "uint256"
          },
          {
            "internalType": "string",
            "name": "metadata",
            "type": "string"
          },
          {
            "internalType": "uint256",
            "name": "poType",
            "type": "uint256"
          }
        ],
        "name": "registerStep",
        "outputs": [
          {
            "internalType": "bool",
            "name": "success",
            "type": "bool"
          }
        ],
        "stateMutability": "nonpayable",
        "type": "function"
      }
    ]
  }
}'

```

### Registrar una nueva Purchase Order con POST/{address}/registerPO

En la API REST de Kaleido en la sección **POST/{address}/registerPO** se puede registrar una nueva Purchase Order de PROMED para este ejercicio con un ID: "4220". Se requiere como parámetros la dirección del smart contract `<0xb565ffbf2c21420b439be27ddcac47ba4ecd3cd3>` y en el body el ID de la orden, después de esto se puede ejecutar.

[![15](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/15.png?raw=true "15")](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/15.png?raw=true "15")

En la respuesta del servidor muestra el Curl con el link y la key de autorización, también muestra la respuesta con toda la información de la transacción realizada para registrar la nueva order.

[![16](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/16.png?raw=true "16")](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/16.png?raw=true "16")

Respuesta del servidor:

```javascript
{
  "headers": {
    "id": "86cc38d7-31ee-4323-4f52-c943c87b5cea",
    "type": "TransactionSuccess",
    "timeReceived": "2023-03-10T21:00:56.504590978Z",
    "timeElapsed": 4.55330944,
    "requestOffset": "",
    "requestId": "8ac87fa1-3209-4566-50b1-53c3308dd821"
  },
  "blockHash": "0x95ea9e8d8423e0a00a612579c21d3487cb05b50aa65ff8b87a4844c7d577f65f",
  "blockNumber": "197342",
  "cumulativeGasUsed": "49553",
  "from": "0x527e08ea398d607e898eb39176894f1815bfeb0b",
  "gasUsed": "49553",
  "nonce": "0",
  "status": "1",
  "to": "0xb565ffbf2c21420b439be27ddcac47ba4ecd3cd3",
  "transactionHash": "0xd5fb035e2a29b49aa8f070bc7565784f87e18102fb2a9c6fbe92e9da3aab498a",
  "transactionIndex": "0"
}

```

Al intentar registrar la PO on el mismo ID directamente en POSTMAN, se logra comprobar mediante un error que la Purchase Order ya existe en la instancia del contrato. 

[![17](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/17.png?raw=true "17")](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/17.png?raw=true "17")

Código JSON para documentación:

```javascript
curl --location 'https://u0cqnbwz76-u0r2m5xw62-connect.us0-aws.kaleido.io/gateways/u0vl2r5dli/0xb565ffbf2c21420b439be27ddcac47ba4ecd3cd3/registerPO?kld-from=0x527e08ea398d607e898eb39176894f1815bfeb0b&kld-sync=true' \
--header 'Authorization: Basic dTB3anJrZzRjZTpfUHRZdDBkWWFSenZ6MDBnNU01d1p1WU13M2xMUFdhOHJMZzQwMEU1SXJ3' \
--header 'Content-Type: application/json' \
--data '{
  "POID": "4220"
}'
```

### Validar estado de Purchase Order con POST/{address}/POvalidator

En la API REST de Kaleido en la sección **POST/{address}/POvalidator** con el ID de la Purchase Order y el número "0" que equivale al estado "TO_BE_CONFIRMED", se puede hacer un llamado al smart contract y muestra como salida el status actual de la PO y un string que puede contener metadata del proceso hasta el momento. 

[![18](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/18.png?raw=true "18")](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/18.png?raw=true "18")

[![19](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/19.png?raw=true "19")](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/19.png?raw=true "19")

Se obtiene una respuesta correcta indicando que la Purchase Order esta en el estado CONFIRMED_PO = status 0. Se realiza la respectiva configuración en POSTMAN del método y se obtiene el mismo resultado.

[![20](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/20.png?raw=true "20")](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/20.png?raw=true "20")

Respuesta de POSTMAN:

```javascript
{
    "metadata": "",
    "status": "0"
}

```

Código JSON para documentación:

```javascript
curl --location 'https://u0cqnbwz76-u0r2m5xw62-connect.us0-aws.kaleido.io/gateways/u0vl2r5dli/0xb565ffbf2c21420b439be27ddcac47ba4ecd3cd3/POvalidator?kld-from=0x527e08ea398d607e898eb39176894f1815bfeb0b&kld-sync=true' \
--header 'Authorization: Basic dTB3anJrZzRjZTpfUHRZdDBkWWFSenZ6MDBnNU01d1p1WU13M2xMUFdhOHJMZzQwMEU1SXJ3' \
--header 'Content-Type: application/json' \
--data '{
  "input": "4220",
  "input1": "0"
}'

```

### Registrar paso a siguiente estado de la Purchase Order con POST/{address}/registerStep

En la API REST de Kaleido en la sección **POST/{address}/registerStep** con los datos del ID de la Purchase Order, una metadata en json y el tipo de orden, se registra un nuevo paso en el estado de la orden en el contrato y se hace la ejecución de la transacción.

> **Consideraciones:**
> De acuerdo al flujo de ordenes de compra se puede despachar el producto por completo, en dos parciales, tres parciales ó n parciales. Por esto se dividieron en los siguientes tipos de ordenes con sus estados correspondientes, condicionando el contrato a que de acuerdo al tipo x de orden solo tendrá n estados posibles:
>
> - **poType = 1**
>   Estados: To be confirmed(0), Approved(1) y Booking Request(2).
>
> - **poType = 2**
>   Estados: To be confirmed(0), Approved P1(1), To be confirmed P2(2), Approved P2(3) y Booking Request(4).
>
> - **poType = 3**
>   Estados: To be confirmed(0), Approved P1(1), To be confirmed P2(2), Approved P2(3), To be confirmed P3(4), Approved P3(5) y Booking Request(6).
>
> - **poType = x**
>   Estados: To be confirmed(0), Approved P1(1), To be confirmed P2(2), Approved P2(3), To be confirmed P3(4), Approved P3(5), To be confirmed Px(n), Approved Px(n) y Booking Request(n).
>

[![21](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/21.png?raw=true "21")](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/21.png?raw=true "21")

De manera efectiva se obtiene la información de la transacción en la red blockchain donde ahora el status de la order "4220" de tipo 1 es APPROVED = status 1.

[![22](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/22.png?raw=true "22")](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/22.png?raw=true "22")

Respuesta del servidor:

```javascript
{
  "headers": {
    "id": "9aed9363-2fc3-4b3b-7201-c3fd02460edd",
    "type": "TransactionSuccess",
    "timeReceived": "2023-03-10T21:57:40.919966828Z",
    "timeElapsed": 5.646317151,
    "requestOffset": "",
    "requestId": "a20539a6-9d16-459f-48be-73ed8dcb25c5"
  },
  "blockHash": "0x7f6fb5053a734b486e87efa565494e081c7266ea102a64401e1cb6b03cefb0f0",
  "blockNumber": "198023",
  "cumulativeGasUsed": "358238",
  "from": "0x527e08ea398d607e898eb39176894f1815bfeb0b",
  "gasUsed": "358238",
  "nonce": "0",
  "status": "1",
  "to": "0xb565ffbf2c21420b439be27ddcac47ba4ecd3cd3",
  "transactionHash": "0xb4775253fa5a0df26ec866e018bf589d3f0fefbfdbd662381bbe0a7589c95f06",
  "transactionIndex": "0"
}
```
Usando el POSTMAN de POvalidator se puede verificar que la PO "4220" si se encuentra en status 1.

[![23](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/23.png?raw=true "23")](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/23.png?raw=true "23")

Código JSON para documentación:

```javascript
curl --location 'https://u0cqnbwz76-u0r2m5xw62-connect.us0-aws.kaleido.io/gateways/u0vl2r5dli/0xb565ffbf2c21420b439be27ddcac47ba4ecd3cd3/POvalidator?kld-from=0x527e08ea398d607e898eb39176894f1815bfeb0b&kld-sync=true' \
--header 'Authorization: Basic dTB3anJrZzRjZTpfUHRZdDBkWWFSenZ6MDBnNU01d1p1WU13M2xMUFdhOHJMZzQwMEU1SXJ3' \
--header 'Content-Type: application/json' \
--data '{
  "input": "4220",
  "input1": "1"
}'
```

El código permite registrar y validar los siguientes estados hasta que la orden "4220" de tipo se encuentre en estado BOOKING REQUEST.

- BOOKING_REQUEST = status 2

[![24](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/24.png?raw=true "24")](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/24.png?raw=true "24")

Respuesta de POSTMAN:

```javascript
{
    "headers": {
        "id": "e70fc1de-3b15-47b2-78dd-125e30362034",
        "type": "TransactionSuccess",
        "timeReceived": "2023-03-10T22:10:14.354714927Z",
        "timeElapsed": 3.265814907,
        "requestOffset": "",
        "requestId": "8e2e284c-7fdf-417f-4e5c-96c771ed8398"
    },
    "blockHash": "0xfab147f02b7f4bea1a285c0cf4584fa0eed4fb123260b48c1e34035c6a28c3c6",
    "blockNumber": "198173",
    "cumulativeGasUsed": "389104",
    "from": "0x527e08ea398d607e898eb39176894f1815bfeb0b",
    "gasUsed": "389104",
    "nonce": "0",
    "status": "1",
    "to": "0xb565ffbf2c21420b439be27ddcac47ba4ecd3cd3",
    "transactionHash": "0xb364e45aa9b8c4b9966ed4ee374dac96ca82d4a2f401f64f6bf44583bc2ab9fb",
    "transactionIndex": "0"
}

```

Código JSON para documentación:

```javascript
curl --location 'https://u0cqnbwz76-u0r2m5xw62-connect.us0-aws.kaleido.io/gateways/u0vl2r5dli/0xb565ffbf2c21420b439be27ddcac47ba4ecd3cd3/registerStep?kld-from=0x527e08ea398d607e898eb39176894f1815bfeb0b&kld-sync=true' \
--header 'Authorization: Basic dTB3anJrZzRjZTpfUHRZdDBkWWFSenZ6MDBnNU01d1p1WU13M2xMUFdhOHJMZzQwMEU1SXJ3' \
--header 'Content-Type: application/json' \
--data '{
  "POID": "4220",
  "metadata": "VENCIMIENTO SEGUN RS, DMARIN, 14 de noviembre de 2022, 11:23:20 UTC-5, USD, USD, USD, Pedido Enviado, IMP, 1,  false, CPT, PANAMA, 14 de noviembre de 2022, 11:56:42 UTC-5, 15 de noviembre de 2022, 05:53:27 UTC-5, Credito, Credito, ECF-4220-NC1-NC1, 24 de febrero de 2023, 11:30:13 UTC-5, 15 de marzo de 2023, 19:00:00 UTC-5, EF12, EF12, DMARIN, APPROVED,  true, 33104, 33104, 0",
  "poType": "1"  
}'

```

Al intentar registrar otro paso para la PO tipo 1 que ya paso a estado BOOKING REQUEST, se genera el error: "The Purchase Order has no more steps", evitando que se incurra en pasos extra.

[![29](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/29.png?raw=true "29")](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/29.png?raw=true "29")

Respuesta de POSTMAN:

```javascript
{
    "error": "Call failed: execution reverted: The maximum status for poType 1 is 2",
    "code": "FFEC100148"
}

```

Otra función que permite el smart contract es validar estados anteriores junto con la metadata que quedo atada a este step, para esto se utiliza el POSTMAN de POvalidator que puede verificar los status de la PO "4220".

[![30](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/30.png?raw=true "30")](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/30.png?raw=true "30")

Respuesta de POSTMAN:

```javascript
{
    "metadata": "VENCIMIENTO SEGUN RS, DMARIN, 14 de noviembre de 2022, 11:23:20 UTC-5, USD, USD, USD, Pedido Enviado, IMP, 1,  false, CPT, PANAMA, 14 de noviembre de 2022, 11:56:42 UTC-5, 15 de noviembre de 2022, 05:53:27 UTC-5, Credito, Credito, ECF-4220-NC1-NC1, 24 de febrero de 2023, 11:30:13 UTC-5, 15 de marzo de 2023, 19:00:00 UTC-5, EF12, EF12, DMARIN, APPROVED,  true, 33104, 33104, 0",
    "status": "2"
}

```

Código JSON para documentación:

```javascript
curl --location 'https://u0cqnbwz76-u0r2m5xw62-connect.us0-aws.kaleido.io/gateways/u0vl2r5dli/0xb565ffbf2c21420b439be27ddcac47ba4ecd3cd3/POvalidator?kld-from=0x527e08ea398d607e898eb39176894f1815bfeb0b&kld-sync=true' \
--header 'Authorization: Basic dTB3anJrZzRjZTpfUHRZdDBkWWFSenZ6MDBnNU01d1p1WU13M2xMUFdhOHJMZzQwMEU1SXJ3' \
--header 'Content-Type: application/json' \
--data '{
  "input": "4220",
  "input1": "2"
}'

```

[![31](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/31.png?raw=true "31")](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/31.png?raw=true "31")

Respuesta de POSTMAN:

```javascript
{
    "metadata": "",
    "status": "0"
}

```

Código JSON para documentación:

```javascript
curl --location 'https://u0cqnbwz76-u0r2m5xw62-connect.us0-aws.kaleido.io/gateways/u0vl2r5dli/0xb565ffbf2c21420b439be27ddcac47ba4ecd3cd3/POvalidator?kld-from=0x527e08ea398d607e898eb39176894f1815bfeb0b&kld-sync=true' \
--header 'Authorization: Basic dTB3anJrZzRjZTpfUHRZdDBkWWFSenZ6MDBnNU01d1p1WU13M2xMUFdhOHJMZzQwMEU1SXJ3' \
--header 'Content-Type: application/json' \
--data '{
  "input": "4220",
  "input1": "0"
}'

```

También facilita que al momento de crear otros Purchase Orders con otros ID, tipo de orden y otros status diferentes, se pueda validar datos de un PO anterior o diferente al que se está trabajando en la misma instancia.

Crear una nueva PO:

[![32](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/32.png?raw=true "32")](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/32.png?raw=true "32")

Respuesta de POSTMAN:

```javascript
{
    "headers": {
        "id": "33d62d31-4c6a-4fc0-7af5-43376298ef74",
        "type": "TransactionSuccess",
        "timeReceived": "2023-03-10T22:53:02.980258269Z",
        "timeElapsed": 3.291637686,
        "requestOffset": "",
        "requestId": "7fb1a677-5666-482d-4149-0d3a1e6b4b62"
    },
    "blockHash": "0x10e67e253fc754945ca384b67a1eaee35e83e4cb95d107f3f71ccea666fc59cd",
    "blockNumber": "198687",
    "cumulativeGasUsed": "49553",
    "from": "0x527e08ea398d607e898eb39176894f1815bfeb0b",
    "gasUsed": "49553",
    "nonce": "0",
    "status": "1",
    "to": "0xb565ffbf2c21420b439be27ddcac47ba4ecd3cd3",
    "transactionHash": "0x4670488318edbc4fab75f3dc575ba3346f0528f11419ed088f869dfa6a5f83eb",
    "transactionIndex": "0"
}

```

Código JSON para documentación:

```javascript
curl --location 'https://u0cqnbwz76-u0r2m5xw62-connect.us0-aws.kaleido.io/gateways/u0vl2r5dli/0xb565ffbf2c21420b439be27ddcac47ba4ecd3cd3/registerPO?kld-from=0x527e08ea398d607e898eb39176894f1815bfeb0b&kld-sync=true' \
--header 'Authorization: Basic dTB3anJrZzRjZTpfUHRZdDBkWWFSenZ6MDBnNU01d1p1WU13M2xMUFdhOHJMZzQwMEU1SXJ3' \
--header 'Content-Type: application/json' \
--data '{
  "POID": "6343"
}'

```

Registrar un siguiente step del status Approved(1) de la PO 6343 tipo 2:

[![33](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/33.png?raw=true "33")](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/33.png?raw=true "33")

Respuesta de POSTMAN:

```javascript
{
    "headers": {
        "id": "e45eafc1-8ede-4e17-77a4-ab31b1665ed7",
        "type": "TransactionSuccess",
        "timeReceived": "2023-03-10T22:54:33.888006418Z",
        "timeElapsed": 3.286305872,
        "requestOffset": "",
        "requestId": "1e653d6c-9e43-4417-698a-7ea4e0311fa4"
    },
    "blockHash": "0x8ab0898b00140d887d260cb32f69b3882301178f1edb5c0e46a11e97e83f44c6",
    "blockNumber": "198705",
    "cumulativeGasUsed": "489350",
    "from": "0x527e08ea398d607e898eb39176894f1815bfeb0b",
    "gasUsed": "489350",
    "nonce": "0",
    "status": "1",
    "to": "0xb565ffbf2c21420b439be27ddcac47ba4ecd3cd3",
    "transactionHash": "0x59d064a08c51813f31307115b501669576210c37bff42dc702f73e3be6a6a588",
    "transactionIndex": "0"
}

```

Código JSON para documentación:

```javascript
curl --location 'https://u0cqnbwz76-u0r2m5xw62-connect.us0-aws.kaleido.io/gateways/u0vl2r5dli/0xb565ffbf2c21420b439be27ddcac47ba4ecd3cd3/registerStep?kld-from=0x527e08ea398d607e898eb39176894f1815bfeb0b&kld-sync=true' \
--header 'Authorization: Basic dTB3anJrZzRjZTpfUHRZdDBkWWFSenZ6MDBnNU01d1p1WU13M2xMUFdhOHJMZzQwMEU1SXJ3' \
--header 'Content-Type: application/json' \
--data '{
  "POID": "6343",
  "metadata": "OTROS CARGOS DEL PROVEEDOR, 428,  PROMED MEDICAL CARE  S.A. PROFORMA E11961,  9 de diciembre de 2022,  06:26:44 UTC-5,  USD,  USD,  USD,  Pedido Enviado,  IMP, 28,  false,  EXWORKS,  ZF INTEXZONA BD 85,  9 de diciembre de 2022,  08:15:04 UTC-5,  9 de diciembre de 2022,  14:05:25 UTC-5,  Credit 120 days,  Credit 120 days,  ER-6342-ER5-ER5,  29 de diciembre de 2022, 17:53:33 UTC-5,  10 de diciembre de 2022,  19:00:00 UTC-5,  EF05,  EF05,  APPROVED P1,  true,  18403.2,  18403.2, 0",
  "poType": "2"  
}'

```

Registrar un siguiente step del status To be confirmed P2(2) de la PO 6343 tipo 2:

[![51](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/51.png?raw=true "51")](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/51.png?raw=true "51")

Respuesta de POSTMAN:

```javascript
{
    "headers": {
        "id": "8ab84708-b971-4292-5319-b89328a8a634",
        "type": "TransactionSuccess",
        "timeReceived": "2023-03-10T22:57:22.29262218Z",
        "timeElapsed": 3.895877963,
        "requestOffset": "",
        "requestId": "8e0311ac-82e7-4e0f-73a3-b366c60d752d"
    },
    "blockHash": "0xc093f9bf7d44bfb5dda29ab61e5166c0a21a833e37552867c90c7bc003c1008f",
    "blockNumber": "198739",
    "cumulativeGasUsed": "528941",
    "from": "0x527e08ea398d607e898eb39176894f1815bfeb0b",
    "gasUsed": "528941",
    "nonce": "0",
    "status": "1",
    "to": "0xb565ffbf2c21420b439be27ddcac47ba4ecd3cd3",
    "transactionHash": "0xa49dc3b71d2109fd9cf4fd48645d8d080519a52f69bda8a81cecb3eb20dcfc6d",
    "transactionIndex": "0"
}

```

Código JSON para documentación:

```javascript
curl --location 'https://u0cqnbwz76-u0r2m5xw62-connect.us0-aws.kaleido.io/gateways/u0vl2r5dli/0xb565ffbf2c21420b439be27ddcac47ba4ecd3cd3/registerStep?kld-from=0x527e08ea398d607e898eb39176894f1815bfeb0b&kld-sync=true' \
--header 'Authorization: Basic dTB3anJrZzRjZTpfUHRZdDBkWWFSenZ6MDBnNU01d1p1WU13M2xMUFdhOHJMZzQwMEU1SXJ3' \
--header 'Content-Type: application/json' \
--data '{
  "POID": "6343",
  "metadata": "OTROS CARGOS DEL PROVEEDOR, 428,  PROMED MEDICAL CARE  S.A. PROFORMA E11961,  9 de diciembre de 2022,  06:26:44 UTC-5,  USD,  USD,  USD,  Pedido Enviado,  IMP, 28,  false,  EXWORKS,  ZF INTEXZONA BD 85,  9 de diciembre de 2022,  08:15:04 UTC-5,  9 de diciembre de 2022,  14:05:25 UTC-5,  Credit 120 days,  Credit 120 days,  ER-6342-ER5-ER5,  29 de diciembre de 2022, 17:53:33 UTC-5,  10 de diciembre de 2022,  19:00:00 UTC-5,  EF05,  EF05,  TO BE CONFIRMED P2,  true,  18403.2,  18403.2, 0",
  "poType": "2"  
}'

```

Registrar un siguiente step del status Approved P2(3) de la PO 6343 tipo 2:

[![52](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/52.png?raw=true "52")](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/52.png?raw=true "52")

Respuesta de POSTMAN:

```javascript
{
    "headers": {
        "id": "c238e496-9ed5-4787-4ebe-d8364692c936",
        "type": "TransactionSuccess",
        "timeReceived": "2023-03-10T22:59:51.782301424Z",
        "timeElapsed": 4.712909029,
        "requestOffset": "",
        "requestId": "68d8f060-68ac-421b-50fc-e099b84633fa"
    },
    "blockHash": "0xe1a0a9dcc011095b51af3366b53ae2d22014c54c2e9cd13686f6963a03dd6fce",
    "blockNumber": "198769",
    "cumulativeGasUsed": "568365",
    "from": "0x527e08ea398d607e898eb39176894f1815bfeb0b",
    "gasUsed": "568365",
    "nonce": "0",
    "status": "1",
    "to": "0xb565ffbf2c21420b439be27ddcac47ba4ecd3cd3",
    "transactionHash": "0x5f671f537193fa17679a56071992a9c19c05a04378f7499e95155c77271c5628",
    "transactionIndex": "0"
}

```

Código JSON para documentación:

```javascript
curl --location 'https://u0cqnbwz76-u0r2m5xw62-connect.us0-aws.kaleido.io/gateways/u0vl2r5dli/0xb565ffbf2c21420b439be27ddcac47ba4ecd3cd3/registerStep?kld-from=0x527e08ea398d607e898eb39176894f1815bfeb0b&kld-sync=true' \
--header 'Authorization: Basic dTB3anJrZzRjZTpfUHRZdDBkWWFSenZ6MDBnNU01d1p1WU13M2xMUFdhOHJMZzQwMEU1SXJ3' \
--header 'Content-Type: application/json' \
--data '{
  "POID": "6343",
  "metadata": "OTROS CARGOS DEL PROVEEDOR, 428,  PROMED MEDICAL CARE  S.A. PROFORMA E11961,  9 de diciembre de 2022,  06:26:44 UTC-5,  USD,  USD,  USD,  Pedido Enviado,  IMP, 28,  false,  EXWORKS,  ZF INTEXZONA BD 85,  9 de diciembre de 2022,  08:15:04 UTC-5,  9 de diciembre de 2022,  14:05:25 UTC-5,  Credit 120 days,  Credit 120 days,  ER-6342-ER5-ER5,  29 de diciembre de 2022, 17:53:33 UTC-5,  10 de diciembre de 2022,  19:00:00 UTC-5,  EF05,  EF05,  APPROVED P2,  true,  18403.2,  18403.2, 0",
  "poType": "2"  
}'

```

Validar status To be confirmed(2) de la PO 6343 tipo 2:

[![34](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/34.png?raw=true "34")](https://bitbucket.org/luis-carlos-charris-molina/blch-11v1/src/main/img/34.png?raw=true "34")

Respuesta de POSTMAN:

```javascript
{
    "metadata": "OTROS CARGOS DEL PROVEEDOR, 428,  PROMED MEDICAL CARE  S.A. PROFORMA E11961,  9 de diciembre de 2022,  06:26:44 UTC-5,  USD,  USD,  USD,  Pedido Enviado,  IMP, 28,  false,  EXWORKS,  ZF INTEXZONA BD 85,  9 de diciembre de 2022,  08:15:04 UTC-5,  9 de diciembre de 2022,  14:05:25 UTC-5,  Credit 120 days,  Credit 120 days,  ER-6342-ER5-ER5,  29 de diciembre de 2022, 17:53:33 UTC-5,  10 de diciembre de 2022,  19:00:00 UTC-5,  EF05,  EF05,  APPROVED P1,  true,  18403.2,  18403.2, 0",
    "status": "2"
}

```

Código JSON para documentación:

```javascript
curl --location 'https://u0cqnbwz76-u0r2m5xw62-connect.us0-aws.kaleido.io/gateways/u0vl2r5dli/0xb565ffbf2c21420b439be27ddcac47ba4ecd3cd3/POvalidator?kld-from=0x527e08ea398d607e898eb39176894f1815bfeb0b&kld-sync=true' \
--header 'Authorization: Basic dTB3anJrZzRjZTpfUHRZdDBkWWFSenZ6MDBnNU01d1p1WU13M2xMUFdhOHJMZzQwMEU1SXJ3' \
--header 'Content-Type: application/json' \
--data '{
  "input": "6343",
  "input1": "2"
}'

```