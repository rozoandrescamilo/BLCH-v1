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
    - [Validar estado de Purchase Order con POST/{address}/ParameterValidator](#validar-estado-de-purchase-order-con-post/{address}/parametervalidator)
    - [Registrar paso a siguiente estado de la Purchase Order con POST/{address}/registerStep](#registrar-paso-a-siguiente-estado-de-la-purchase-order-con-post/{address}/registerstep)

# BLCH-v1

En este repositorio se registra la documentación necesaria para la integración con Frontend por medio de POSTMAN, del smart contract que permite consultar los diferentes estados estados de una Orden de Compra que se encuentra desplegado en la API de Kaleido. 

El funcionamiento del contrato se basa en el flujo de órdenes de compra representando en el siguiente diagrama, muestra el proceso de negocios en el que se describe la interacción entre diferentes actores en la cadena de suministro, desde cuando un cliente realiza una solicitud de compra, la cual es enviada al proveedor. El proveedor revisa la solicitud y envía una confirmación de despacho completo del pedido a Dawipo, si son entregas parciales se debe tener autorización del cliente. Finalmente, el proveedor entrega los productos y emite un Booking Request.

[![35](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/FlujoOC.png?raw=true "35")](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/FlujoOC.png?raw=true "35")

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

[![1](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/1.png "1")](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/1.png "1")

Luego, se deben ingresar los detalles del proyecto como nombre, versión y seleccionar el archivo del contranto.

Para este proceso se utiliza el contrato <POstatusValidator.sol>, un contrato inteligente que tiene como fin crear un método para registrar y consultar los diferentes estados posibles de una Purchase Order desde TO BE CONFIRMED hasta BOOKING REQUEST. Se recomienda ver el funcionamiento de este contrato para entender de mejor manera: [https://github.com/rozoandrescamilo/Smart-Contract-para-consultar-estados-de-una-Purchase-Order](https://github.com/rozoandrescamilo/Smart-Contract-para-consultar-estados-de-una-Purchase-Order "https://github.com/rozoandrescamilo/Smart-Contract-para-consultar-estados-de-una-Purchase-Order")

[![2](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/2.png "2")](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/2.png "2")

Creada la nueva instancia del contrato se adiciona la Gateway API que es una consola Swagger para la gestión del contrato y transacciones.

[![3](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/3.png "3")](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/3.png "3")

Una vez finalizado el ingreso de detalles se mostrará los parametros de la versión y el contrato debe pasar de status Compiling a Compiled para poder continuar.

[![4](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/4.png "4")](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/4.png "4")

Después se mostrará detalles del Smart Contract donde se podran realizar varias actividades con API GATEWAY:

- Crear una API de puerta de enlace: crea una consola swagger dinámica y una especificación descargable para la interacción RESTful con métodos de contrato inteligente.

- Implementar contratos: use la API de Gateway para implementar su contrato inteligente mediante programación o directamente a través de la consola.

- Enviar transacciones: Aproveche la API para interactuar con contratos inteligentes en cadena existentes al pasar la dirección del contrato como parámetro en la llamada.

[![5](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/5.png "5")](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/5.png "5")

### Gestión de smart contract con REST API Gateway

La interfaz de usuario Swagger incorporada se despliega dando clic en el boton VIEW GATEWAY API y se pueden visualizar los servicios web de acceso y gestión de datos de organizaciones y proyectos a través de REST APIs.

[![6](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/6.png "6")](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/6.png "6")

Los servicios POST y los parámetros ligados que se usarán:

| Servicio POST | Link API | Params (key:value) | Headers (key:value) | Body (tipo raw JSON) |
| ---------------------- | ---------------------- | ---------------------- | ---------------------- | ---------------------- |
| **POST/constructor()**      | https://u0cqnbwz76-u0r2m5xw62-connect.us0-aws.kaleido.io/gateways/u0t3eu9oq8/?kld-from=0x527e08ea398d607e898eb39176894f1815bfeb0b&kld-sync=true      | kId-from: 0x527e08ea398d607e898eb39176894f1815bfeb0b, kld-sync: true      | Authorization: Basic dTB3anJrZzRjZTpfUHRZdDBkWWFSenZ6MDBnNU01d1p1WU13M2xMUFdhOHJMZzQwMEU1SXJ3      | ABI generado al compilar smart contract      |
| **POST/{address}/registerPO**      | https://u0cqnbwz76-u0r2m5xw62-connect.us0-aws.kaleido.io/gateways/u0t3eu9oq8/0xc050386500b1b2c8706627bebc622c4bcd6cebff/RegisterPO?kld-from=0x527e08ea398d607e898eb39176894f1815bfeb0b&kld-sync=true      | kId-from: 0x527e08ea398d607e898eb39176894f1815bfeb0b, kld-sync: true      | CAuthorization: Basic dTB3anJrZzRjZTpfUHRZdDBkWWFSenZ6MDBnNU01d1p1WU13M2xMUFdhOHJMZzQwMEU1SXJ3      | "POID": "1234", "userWallet": "0x527e08ea398d607e898eb39176894f1815bfeb0b"      |
| **POST/{address}/ParameterValidator**      | https://u0cqnbwz76-u0r2m5xw62-connect.us0-aws.kaleido.io/gateways/u0t3eu9oq8/0xc050386500b1b2c8706627bebc622c4bcd6cebff/ParameterValidator?kld-from=0x527e08ea398d607e898eb39176894f1815bfeb0b&kld-sync=true      | kId-from: 0x527e08ea398d607e898eb39176894f1815bfeb0b, kld-sync: true      | Authorization: Basic dTB3anJrZzRjZTpfUHRZdDBkWWFSenZ6MDBnNU01d1p1WU13M2xMUFdhOHJMZzQwMEU1SXJ3      | "input": "1234", "input1": "0"      |
| **POST/{address}/registerStep**      | https://u0cqnbwz76-u0r2m5xw62-connect.us0-aws.kaleido.io/gateways/u0t3eu9oq8/0xc050386500b1b2c8706627bebc622c4bcd6cebff/RegisterStep?kld-from=0x527e08ea398d607e898eb39176894f1815bfeb0b&kld-sync=true      | kId-from: 0x527e08ea398d607e898eb39176894f1815bfeb0b, kld-sync: true      | Authorization: Basic dTB3anJrZzRjZTpfUHRZdDBkWWFSenZ6MDBnNU01d1p1WU13M2xMUFdhOHJMZzQwMEU1SXJ3      | "POID": "1234", "metadata": "Data Encode", "poType": "1", "userWallet": "0x527e08ea398d607e898eb39176894f1815bfeb0b"      |

En el **POST/constructor()** se debe agregar el archivo ABI generado en la compilación del contrato y hacer clic en EJECUTAR.

[![7](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/7.png "7")](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/7.png "7")

ABI del contrato compilado:

```javascript
{
  "input": {
    "abi": [
      {
        "inputs": [],
        "stateMutability": "nonpayable",
        "type": "constructor"
      },
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
        "name": "ParameterValidator",
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
            "internalType": "address",
            "name": "userWallet",
            "type": "address"
          },
          {
            "internalType": "uint256",
            "name": "POID",
            "type": "uint256"
          }
        ],
        "name": "RegisterPO",
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
            "internalType": "address",
            "name": "userWallet",
            "type": "address"
          },
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
        "name": "RegisterStep",
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

[![8](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/8.png "8")](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/8.png "8")

## Gestión del flujo del contrato y documentación de REST API con POSTMAN

Para generar la documentación necesaria para la integración con Frontend, se utiliza la aplicación de escritorio POSTMAN que es una herramienta para el desarrollo de APIs que facilita el diseño, el testing y el monitoreo de servicios REST. Sirve para probar endpoints de aplicaciones web basadas en APIs o REST. Con POSTMAN, los desarrolladores pueden configurar, enviar y guardar solicitudes HTTP y ver sus respuestas. Esto les permite hacer pruebas más rápidas y los ayuda a comprender mejor sus APIs y cómo interactúan con otros sistemas. Postman también ayuda a los desarrolladores a documentar mejor sus APIs, lo que facilita su uso para otros desarrolladores.

Se selecciona el boton SEND A REQUEST para comenzar la interacción.

[![9](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/9.png "9")](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/9.png "9")

### Crear smart crontract con el POST/constructor()

Teniendo en cuenta el archivo JSON de respuesta por ejecutar el ABI en la REST API de Kaleido, se debe extraer la URL del Curl: "https://u0cqnbwz76-u0r2m5xw62-connect.us0-aws.kaleido.io/gateways/u0t3eu9oq8/?kld-from=0x527e08ea398d607e898eb39176894f1815bfeb0b&kld-sync=true" y agregarla como un llamado POST en POSTMAN. Al ingresar el link reconoce parámetros de la API en la pestaña **Params.** 

[![50](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/50.png "50")](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/50.png "50")

[![10](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/10.png "10")](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/10.png "10")

Luego, en la pestaña **Headers** se debe agregar la Key "Authorization" y como valor "Basic dTB3anJrZzRjZTpfUHRZdDBkWWFSenZ6MDBnNU01d1p1WU13M2xMUFdhOHJMZzQwMEU1SXJ3" que se encuentra en el archivo JSON de salida por ejecutar el ABI en la REST API de Kaleido.

[![11](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/11.png "11")](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/11.png "11")

En la pestaña de **Body** se selecciona el tipo Raw ya que es texto plano y el archivo de tipo JSON, dentro de este se debe pegar el archivo ABI de la compilación del contrato. Una vez realizada esta configuración ya es posible dar clic en **Send** para enviar la solicitud a la API.

[![12](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/12.png "12")](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/12.png "12")

Se obtendra como output una respuesta del servidor similar a la de API REST de Kaliedo, donde se pueden ver parámetros de la transacción realizada, que le pueden ser de interés al usuario a lo largo de todo el flujo del proceso en la gestión de datos. Algunos de los términos relevantes en la respuesta incluyen:

- **contractAddress:** se refiere a la dirección del contrato inteligente utilizado en la transacción.

- **gasUsed y cumulativeGasUsed:** se refieren a la cantidad de gas utilizada en la transacción. El gas es una unidad de medida utilizada en Ethereum (la plataforma blockchain en la que se basa Kaleido) para calcular el costo de las transacciones.

- **transactionHash:** se refiere al hash único de la transacción en la cadena de bloques.

- **blockHash:** hace referencia al algoritmo que representa la dirección del bloque utilizado en la transacción.

- **blockNumber:** número del bloque utilizado en la transacción.

[![13](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/13.png "13")](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/13.png "13")

Respuesta de POSTMAN:

```javascript
{
    "headers": {
        "id": "5989196f-01c2-4152-5623-17a2cf2c48b2",
        "type": "TransactionSuccess",
        "ctx": {
            "isRemoteRegistry": true
        },
        "timeReceived": "2023-03-15T20:57:48.163827051Z",
        "timeElapsed": 8.343312325,
        "requestOffset": "",
        "requestId": "0210845c-68a4-4b3b-7c99-f000548b3a32",
        "requestABIId": "4cc3b807-3146-44a7-5c50-f0cd621af88d"
    },
    "blockHash": "0x0dc16d4971928ab43920e8b55171055ebf8f25387329784586a7631085bc64fb",
    "blockNumber": "252186",
    "openapi": "https://u0cqnbwz76-u0r2m5xw62-connect.us0-aws.kaleido.io/instances/c050386500b1b2c8706627bebc622c4bcd6cebff?openapi",
    "apiexerciser": "https://u0cqnbwz76-u0r2m5xw62-connect.us0-aws.kaleido.io/instances/c050386500b1b2c8706627bebc622c4bcd6cebff?ui",
    "contractAddress": "0xc050386500b1b2c8706627bebc622c4bcd6cebff",
    "cumulativeGasUsed": "1306867",
    "from": "0x527e08ea398d607e898eb39176894f1815bfeb0b",
    "gasUsed": "1306867",
    "nonce": "0",
    "status": "1",
    "to": null,
    "transactionHash": "0x7f957f9b1e0a5688c6f51a6d443504485c235c6a678a9fd303f90c5471ba7fd3",
    "transactionIndex": "0"
}

```

En la parte derecha de POSTMAN en la pestaña **</>** se puede obtener el fragmento de código resultante, que servirá como documentación necesaria para la integración con el equipo de Frontend. Este fragmento es un archivo JSON con un formato de intercambio de datos utilizado para transmitir información en formato de texto entre diferentes aplicaciones. En este caso, el archivo JSON se está utilizando para realizar una solicitud POST usando la API de Kaleido. Esta solicitud se utiliza para crear una nueva puerta de enlace en la red Kaleido. El contenido del archivo JSON incluye información sobre los parámetros de la solicitud, tales como la dirección de la cuenta de la que se va a realizar la solicitud, la ABI de la aplicación, entre otros.

[![14](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/14.png "14")](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/14.png "14")

Código JSON para documentación:

```javascript
curl --location 'https://u0cqnbwz76-u0r2m5xw62-connect.us0-aws.kaleido.io/gateways/u0t3eu9oq8/?kld-from=0x527e08ea398d607e898eb39176894f1815bfeb0b&kld-sync=true' \
--header 'Authorization: Basic dTB3anJrZzRjZTpfUHRZdDBkWWFSenZ6MDBnNU01d1p1WU13M2xMUFdhOHJMZzQwMEU1SXJ3' \
--header 'Content-Type: application/json' \
--data '{
  "input": {
    "abi": [
      {
        "inputs": [],
        "stateMutability": "nonpayable",
        "type": "constructor"
      },
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
        "name": "ParameterValidator",
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
            "internalType": "address",
            "name": "userWallet",
            "type": "address"
          },
          {
            "internalType": "uint256",
            "name": "POID",
            "type": "uint256"
          }
        ],
        "name": "RegisterPO",
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
            "internalType": "address",
            "name": "userWallet",
            "type": "address"
          },
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
        "name": "RegisterStep",
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

En la API REST de Kaleido en la sección **POST/{address}/registerPO** se puede registrar una nueva Purchase Order de PROMED para este ejercicio con un ID: "1111". Se requiere como parámetros la dirección del smart contract `<0xc050386500b1b2c8706627bebc622c4bcd6cebff>` y en el body el ID de la orden la direcció de la wallet del usuario, después de esto se puede ejecutar. 

> **Consideraciones:**
> Para la función RegisterPO se agregó una validación que comprueba si la dirección de wallet del usuario que llama a la función es igual a la dirección de wallet permitida para ese usuario y si la dirección de wallet del usuario está en la lista de direcciones permitidas almacenadas en el mapping **allowedWallets.**
> Con esta implementación se logra garantizar que solo los usuarios autorizados (Participantes: Cliente, proveedor y Dawipo) puedan interactuar con el contrato inteligente, generando confirmaciones y que no haya manipulación maliciosa de los datos de la orden de compra.
>

[![15](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/15.png "15")](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/15.png "15")

En la respuesta del servidor muestra el Curl con el link para POSTMAN y la key de autorización, también muestra la respuesta con toda la información de la transacción realizada para registrar la nueva order.

[![16](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/16.png "16")](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/16.png "16")

Respuesta del servidor:

```javascript
{
  "headers": {
    "id": "ee2ed1e5-cd64-4abe-7609-874441cca7e6",
    "type": "TransactionSuccess",
    "timeReceived": "2023-03-15T21:06:30.651494617Z",
    "timeElapsed": 6.946242881,
    "requestOffset": "",
    "requestId": "3378b0a5-473e-49b3-72da-885dee86b7d4"
  },
  "blockHash": "0x3d293f533aebe0e322ec4cde66a68cea6ad45fa9c8651796339611721bb9d67f",
  "blockNumber": "252290",
  "cumulativeGasUsed": "50177",
  "from": "0x527e08ea398d607e898eb39176894f1815bfeb0b",
  "gasUsed": "50177",
  "nonce": "0",
  "status": "1",
  "to": "0xc050386500b1b2c8706627bebc622c4bcd6cebff",
  "transactionHash": "0x4c5988e709332cbd4844e840ee4b81fe25976b23bd6312b5f59469f81450057b",
  "transactionIndex": "0"
}

```

Al intentar registrar la PO on el mismo ID directamente en POSTMAN, se logra comprobar mediante un error que la Purchase Order ya existe en la instancia del contrato. 

[![17](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/17.png "17")](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/17.png "17")

Respuesta del servidor:

```javascript
{
    "error": "Call failed: execution reverted: This product already exists",
    "code": "FFEC100148"
}

```

Código JSON para documentación:

```javascript
curl --location 'https://u0cqnbwz76-u0r2m5xw62-connect.us0-aws.kaleido.io/gateways/u0t3eu9oq8/0xc050386500b1b2c8706627bebc622c4bcd6cebff/RegisterPO?kld-from=0x527e08ea398d607e898eb39176894f1815bfeb0b&kld-sync=true' \
--header 'Authorization: Basic dTB3anJrZzRjZTpfUHRZdDBkWWFSenZ6MDBnNU01d1p1WU13M2xMUFdhOHJMZzQwMEU1SXJ3' \
--header 'Content-Type: application/json' \
--data '{
  "POID": "1111",
  "userWallet": "0x527e08ea398d607e898eb39176894f1815bfeb0b"
}'
```

### Validar estado de Purchase Order con POST/{address}/ParameterValidator

En la API REST de Kaleido en la sección **POST/{address}/ParameterValidator** con el ID de la Purchase Order y el número "0" que equivale al estado "TO_BE_CONFIRMED", se puede hacer un llamado al smart contract y muestra como salida el status actual de la PO y un string que puede contener metadata del proceso hasta el momento.

[![18](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/18.png "18")](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/18.png "18")

[![19](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/19.png "19")](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/19.png "19")

Se obtiene una respuesta correcta indicando que la Purchase Order esta en el estado TO_BE_CONFIRMED = status 0. Se realiza la respectiva configuración en POSTMAN del método y se obtiene el mismo resultado.

[![20](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/20.png "20")](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/20.png "20")

Respuesta de POSTMAN:

```javascript
{
    "metadata": "",
    "status": "0"
}

```

Código JSON para documentación:

```javascript
curl --location 'https://u0cqnbwz76-u0r2m5xw62-connect.us0-aws.kaleido.io/gateways/u0t3eu9oq8/0xc050386500b1b2c8706627bebc622c4bcd6cebff/ParameterValidator?kld-from=0x527e08ea398d607e898eb39176894f1815bfeb0b&kld-sync=true' \
--header 'Authorization: Basic dTB3anJrZzRjZTpfUHRZdDBkWWFSenZ6MDBnNU01d1p1WU13M2xMUFdhOHJMZzQwMEU1SXJ3' \
--header 'Content-Type: application/json' \
--data '{
  "input": "1111",
  "input1": "0"
}'

```

### Registrar paso a siguiente estado de la Purchase Order con POST/{address}/registerStep

En la API REST de Kaleido en la sección **POST/{address}/registerStep** con los datos del ID de la Purchase Order, una metadata en un json códificado, el tipo de orden y la wallet de usuario se registra un nuevo paso en el estado de la orden en el contrato y se hace la ejecución de la transacción.

> **Consideraciones:**
>
> 1. La variable metadata al ser tipo string solo admite texto plano, por lo tanto se hace necesario que la metadata de la orden de compra en cada step de los estados posibles se le agregue el archivo JSON codificado, debe enviarse al contrato como una cadena de caracteres codificada en base64.
>
>    Para codificar el archivo JSON, se utilizó una herramienta en línea llamada https://www.base64encode.org/. Después de copiar y pegar el contenido del archivo JSON en el campo de entrada de la herramienta, con un clic en el botón "Encode" se obtendra la versión codificada en base64 de la cadena que admite la variable metadata.
>
>    De igual manera al momento de leer la salida de los POST, se puede copiar la versión codificada en base64 de la cadena de metadata y pegarlo en la sección de "Decode" de la herramienta para obtener el JSON que se habia códificado.
> 
> 2. De acuerdo al flujo de ordenes de compra se puede despachar el producto por completo, en dos parciales, tres parciales ó n parciales. Por esto se dividieron en los siguientes tipos de ordenes con sus estados correspondientes, condicionando el contrato a que de acuerdo al tipo x de orden solo tendrá n estados posibles:
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
> 3. Para la función registerStep se agregó una validación que comprueba si la dirección de wallet del usuario que llama a la función es igual a la dirección de wallet permitida para ese usuario y si la dirección de wallet del usuario está en la lista de direcciones permitidas almacenadas en el mapping **allowedWallets.**
> Con esta implementación se logra garantizar que solo los usuarios autorizados (Participantes: Cliente, proveedor y Dawipo) puedan interactuar con el contrato inteligente, generando confirmaciones y que no haya manipulación maliciosa de los datos de la orden de compra.
>

JSON de prueba para codificar con 200 registros de campo y valor:

```javascript
{
    "purchaseOrder": "ECF-4222-NC1-NC1",
    "orderDate": "15 de noviembre de 2022, 11:56:42 UTC-5",
    "status": "APPROVED",
    "buyer": "DMARIN",	
    "seller": "EF12",	
    "incoterm":	"CPT",
    "incotermName":	"PANAMA",
    "documentType":	"IMP",
    "documentStatus": "Pedido Enviado",
    "totalCashExcludingTax": "33104",	
    "id": "657b1193-655b-4562-5236-8c0fb0302744",
    "type": "TransactionSuccess",
    "timeReceived": "2023-02-25T05:24:27.082544577Z",
    "timeElapsed": 6.370812683,
    "requestOffset": "",
    "requestId": "69615e10-4f7e-4fb7-60eb-fa702972d9ec",
    "blockHash": "0xc77f7340764b721d125d608939b00f68dde391beb67016ed0e798dcd49d79ed6",
    "blockNumber": "79220",
    "cumulativeGasUsed": "102828",
    "from": "0xf0afc19d9108b443096f0de4d461093b106b3bcd",
    "gasUsed": "102828",
    "nonce": "0",
    "to": "0x35aac50f63e614e158f2f33aae23c6c50ec75785",
    "transactionHash": "0x81d6bed2f7d6e81c526d34cf65aebca04e6986c6326212b1be30d4ee493bbe4e",

    // ... Resto de los 200 registros
}
```

[![53](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/53.png "53")](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/53.png "53")

Resultado de codificación:

```javascript
{
   ewogICAgInB1cmNoYXNlT3JkZXIiOiAiRUNGLTQyMjItTkMxLU5DMSIsCiAgICAib3JkZXJEYXRlIjogIjE1IGRlIG5vdmllbWJyZSBkZSAyMDIyLCAxMTo1Njo0MiBVVEMtNSIsCiAgICAic3RhdHVzIjogIkFQUFJPVkVEIiwKICAgICJidXllciI6ICJETUFSSU4iLAkKICAgICJzZWxsZXIiOiAiRUYxMiIsCQogICAgImluY290ZXJtIjoJIkNQVCIsCiAgICAiaW5jb3Rlcm1OYW1lIjoJIlBBTkFNQSIsCiAgICAiZG9jdW1lbnRUeXBlIjoJIklNUCIsCiAgICAiZG9jdW1lbnRTdGF0dXMiOiAiUGVkaWRvIEVudmlhZG8iLAogICAgInRvdGFsQ2FzaEV4Y2x1ZGluZ1RheCI6ICIzMzEwNCIsCQoKICAgICJpZCI6ICI2NTdiMTE5My02NTViLTQ1NjItNTIzNi04YzBmYjAzMDI3NDQiLAogICAgInR5cGUiOiAiVHJhbnNhY3Rpb25TdWNjZXNzIiwKICAgICJ0aW1lUmVjZWl2ZWQiOiAiMjAyMy0wMi0yNVQwNToyNDoyNy4wODI1NDQ1NzdaIiwKICAgICJ0aW1lRWxhcHNlZCI6IDYuMzcwODEyNjgzLAogICAgInJlcXVlc3RPZmZzZXQiOiAiIiwKICAgICJyZXF1ZXN0SWQiOiAiNjk2MTVlMTAtNGY3ZS00ZmI3LTYwZWItZmE3MDI5NzJkOWVjIiwKICAgICJibG9ja0hhc2giOiAiMHhjNzdmNzM0MDc2NGI3MjFkMTI1ZDYwODkzOWIwMGY2OGRkZTM5MWJlYjY3MDE2ZWQwZTc5OGRjZDQ5ZDc5ZWQ2IiwKICAgICJibG9ja051bWJlciI6ICI3OTIyMCIsCiAgICAiY3VtdWxhdGl2ZUdhc1VzZWQiOiAiMTAyODI4IiwKICAgICJmcm9tIjogIjB4ZjBhZmMxOWQ5MTA4YjQ0MzA5NmYwZGU0ZDQ2MTA5M2IxMDZiM2JjZCIsCiAgICAiZ2FzVXNlZCI6ICIxMDI4MjgiLAogICAgIm5vbmNlIjogIjAiLAogICAgInRvIjogIjB4MzVhYWM1MGY2M2U2MTRlMTU4ZjJmMzNhYWUyM2M2YzUwZWM3NTc4NSIsCiAgICAidHJhbnNhY3Rpb25IYXNoIjogIjB4ODFkNmJlZDJmN2Q2ZTgxYzUyNmQzNGNmNjVhZWJjYTA0ZTY5ODZjNjMyNjIxMmIxYmUzMGQ0ZWU0OTNiYmU0ZSIsCiAgICAidHJhbnNhY3Rpb25JbmRleCI6ICIwIiwKCiAgICAicHVyY2hhc2VPcmRlciI6ICJFQ0YtNDIyMi1OQzEtTkMxIiwKICAgICJvcmRlckRhdGUiOiAiMTUgZGUgbm92aWVtYnJlIGRlIDIwMjIsIDExOjU2OjQyIFVUQy01IiwKICAgICJzdGF0dXMiOiAiQVBQUk9WRUQiLAogICAgImJ1eWVyIjogIkRNQVJJTiIsCQogICAgInNlbGxlciI6ICJFRjEyIiwJ

   // ... Resto de JSON codificado
}
```

[![21](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/21.png "21")](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/21.png "21")

De manera efectiva se obtiene la información de la transacción en la red blockchain donde ahora el status de la order "1111" de tipo 1 es APPROVED = status 1.

[![22](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/22.png "22")](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/22.png "22")

Respuesta del servidor:

```javascript
{
    "headers": {
        "id": "8fdf2264-8318-4312-48a7-4d6073a89905",
        "type": "TransactionSuccess",
        "timeReceived": "2023-03-15T22:31:47.525285977Z",
        "timeElapsed": 6.7580914960000005,
        "requestOffset": "",
        "requestId": "a1ed844c-3b13-4520-5bd4-85f73c1d2b2c"
    },
    "blockHash": "0xa5e6c7eacea6ff412578e565e0e98088c5a9de8e6a4c774545ceb516542efa21",
    "blockNumber": "253314",
    "cumulativeGasUsed": "8610617",
    "from": "0x527e08ea398d607e898eb39176894f1815bfeb0b",
    "gasUsed": "8610617",
    "nonce": "0",
    "status": "1",
    "to": "0xc050386500b1b2c8706627bebc622c4bcd6cebff",
    "transactionHash": "0xa2d4b6a6082b7f65266dc4ee6f45df0585682965f8e513d265a5780d0f64dde3",
    "transactionIndex": "0"
}
```

```javascript
curl --location 'https://u0cqnbwz76-u0r2m5xw62-connect.us0-aws.kaleido.io/gateways/u0t3eu9oq8/0xc050386500b1b2c8706627bebc622c4bcd6cebff/RegisterStep?kld-from=0x527e08ea398d607e898eb39176894f1815bfeb0b&kld-sync=true' \
--header 'Authorization: Basic dTB3anJrZzRjZTpfUHRZdDBkWWFSenZ6MDBnNU01d1p1WU13M2xMUFdhOHJMZzQwMEU1SXJ3' \
--header 'Content-Type: application/json' \
--data '{
  "POID": "1111",
  "metadata": "ewogICAgInB1cmNoYXNlT3JkZXIiOiAiRUNGLTQyMjItTkMxLU5DMSIsCiAgICAib3JkZXJEYXRlIjogIjE1IGRlIG5vdmllbWJyZSBkZSAyMDIyLCAxMTo1Njo0MiBVVEMtNSIsCiAgICAic3RhdHVzIjogIkFQUFJPVkVEIiwKICAgICJidXllciI6ICJETUFSSU4iLAkKICAgICJzZWxsZXIiOiAiRUYxMiIsCQogICAgImluY290ZXJtIjoJIkNQVCIsCiAgICAiaW5jb3Rlcm1OYW1lIjoJIlBBTkFNQSIsCiAgICAiZG9jdW1lbnRUeXBlIjoJIklNUCIsCiAgICAiZG9jdW1lbnRTdGF0dXMiOiAiUGVkaWRvIEVudmlhZG8iLAogICAgInRvdGFsQ2FzaEV4Y2x1ZGluZ1RheCI6ICIzMzEwNCIsCQoKICAgICJpZCI6ICI2NTdiMTE5My02NTViLTQ1NjItNTIzNi04YzBmYjAzMDI3NDQiLAogICAgInR5cGUiOiAiVHJhbnNhY3Rpb25TdWNjZXNzIiwKICAgICJ0aW1lUmVjZWl2ZWQiOiAiMjAyMy0wMi0yNVQwNToyNDoyNy4wODI1NDQ1NzdaIiwKICAgICJ0aW1lRWxhcHNlZCI6IDYuMzcwODEyNjgzLAogICAgInJlcXVlc3RPZmZzZXQiOiAiIiwKICAgICJyZXF1ZXN0SWQiOiAiNjk2MTVlMTAtNGY3ZS00ZmI3LTYwZWItZmE3MDI5NzJkOWVjIiwKICAgICJibG9ja0hhc2giOiAiMHhjNzdmNzM0MDc2NGI3MjFkMTI1ZDYwODkzOWIwMGY2OGRkZTM5MWJlYjY3MDE2ZWQwZTc5OGRjZDQ5ZDc5ZWQ2IiwKICAgICJibG9ja051bWJlciI6ICI3OTIyMCIsCiAgICAiY3VtdWxhdGl2ZUdhc1VzZWQiOiAiMTAyODI4IiwKICAgICJmcm9tIjogIjB4ZjBhZmMxOWQ5MTA4YjQ0MzA5NmYwZGU0ZDQ2MTA5M2IxMDZiM2JjZCIsCiAgICAiZ2FzVXNlZCI6ICIxMDI4MjgiLAogICAgIm5vbmNlIjogIjAiLAogICAgInRvIjogIjB4MzVhYWM1MGY2M2U2MTRlMTU4ZjJmMzNhYWUyM2M2YzUwZWM3NTc4NSIsCiAgICAidHJhbnNhY3Rpb25IYXNoIjogIjB4ODFkNmJlZDJmN2Q2ZTgxYzUyNmQzNGNmNjVhZWJjYTA0ZTY5ODZjNjMyNjIxMmIxYmUzMGQ0ZWU0OTNiYmU0ZSIsCiAgICAidHJhbnNhY3Rpb25JbmRleCI6ICIwIiwKCiAgICAicHVyY2hhc2VPcmRlciI6ICJFQ0YtNDIyMi1OQzEtTkMxIiwKICAgICJvcmRlckRhdGUiOiAiMTUgZGUgbm92aWVtYnJlIGRlIDIwMjIsIDExOjU2OjQyIFVUQy01IiwKICAgICJzdGF0dXMiOiAiQVBQUk9WRUQiLAogICAgImJ1eWVyIjogIkRNQVJJTiIsCQogICAgInNlbGxlciI6ICJFRjEyIiwJ",
  "poType": "1",
  "userWallet": "0x527e08ea398d607e898eb39176894f1815bfeb0b"
}'
```
Usando el POSTMAN de ParameterValidator se puede verificar que la PO "1111" si se encuentra en status 1.

[![23](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/23.png "23")](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/23.png "23")

Código JSON para documentación:

```javascript
curl --location 'https://u0cqnbwz76-u0r2m5xw62-connect.us0-aws.kaleido.io/gateways/u0t3eu9oq8/0xc050386500b1b2c8706627bebc622c4bcd6cebff/ParameterValidator?kld-from=0x527e08ea398d607e898eb39176894f1815bfeb0b&kld-sync=true' \
--header 'Authorization: Basic dTB3anJrZzRjZTpfUHRZdDBkWWFSenZ6MDBnNU01d1p1WU13M2xMUFdhOHJMZzQwMEU1SXJ3' \
--header 'Content-Type: application/json' \
--data '{
  "input": "1111",
  "input1": "1"
}'
```

El código permite registrar y validar los siguientes estados hasta que la orden "1111" de tipo se encuentre en estado BOOKING REQUEST.

- BOOKING_REQUEST = status 2

[![24](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/24.png "24")](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/24.png "24")

Respuesta de POSTMAN:

```javascript
{
    "headers": {
        "id": "8fdf2264-8318-4312-48a7-4d6073a89905",
        "type": "TransactionSuccess",
        "timeReceived": "2023-03-15T22:31:47.525285977Z",
        "timeElapsed": 6.7580914960000005,
        "requestOffset": "",
        "requestId": "a1ed844c-3b13-4520-5bd4-85f73c1d2b2c"
    },
    "blockHash": "0xa5e6c7eacea6ff412578e565e0e98088c5a9de8e6a4c774545ceb516542efa21",
    "blockNumber": "253314",
    "cumulativeGasUsed": "8610617",
    "from": "0x527e08ea398d607e898eb39176894f1815bfeb0b",
    "gasUsed": "8610617",
    "nonce": "0",
    "status": "2",
    "to": "0xc050386500b1b2c8706627bebc622c4bcd6cebff",
    "transactionHash": "0xa2d4b6a6082b7f65266dc4ee6f45df0585682965f8e513d265a5780d0f64dde3",
    "transactionIndex": "0"
}

```

Código JSON para documentación:

```javascript
curl --location 'https://u0cqnbwz76-u0r2m5xw62-connect.us0-aws.kaleido.io/gateways/u0t3eu9oq8/0xc050386500b1b2c8706627bebc622c4bcd6cebff/RegisterStep?kld-from=0x527e08ea398d607e898eb39176894f1815bfeb0b&kld-sync=true' \
--header 'Authorization: Basic dTB3anJrZzRjZTpfUHRZdDBkWWFSenZ6MDBnNU01d1p1WU13M2xMUFdhOHJMZzQwMEU1SXJ3' \
--header 'Content-Type: application/json' \
--data '{
  "POID": "1111",
  "metadata": "ewogICAgInB1cmNoYXNlT3JkZXIiOiAiRUNGLTQyMjItTkMxLU5DMSIsCiAgICAib3JkZXJEYXRlIjogIjE1IGRlIG5vdmllbWJyZSBkZSAyMDIyLCAxMTo1Njo0MiBVVEMtNSIsCiAgICAic3RhdHVzIjogIkJPT0tJTkdfUkVRVUVTVCIsCiAgICAiYnV5ZXIiOiAiRE1BUklOIiwJCiAgICAic2VsbGVyIjogIkVGMTIiLAkKICAgICJpbmNvdGVybSI6CSJDUFQiLAogICAgImluY290ZXJtTmFtZSI6CSJQQU5BTUEiLAogICAgImRvY3VtZW50VHlwZSI6CSJJTVAiLAogICAgImRvY3VtZW50U3RhdHVzIjogIlBlZGlkbyBFbnZpYWRvIiwKICAgICJ0b3RhbENhc2hFeGNsdWRpbmdUYXgiOiAiMzMxMDQiLAkKCiAgICAiaWQiOiAiNjU3YjExOTMtNjU1Yi00NTYyLTUyMzYtOGMwZmIwMzAyNzQ0IiwKICAgICJ0eXBlIjogIlRyYW5zYWN0aW9uU3VjY2VzcyIsCiAgICAidGltZVJlY2VpdmVkIjogIjIwMjMtMDItMjVUMDU6MjQ6MjcuMDgyNTQ0NTc3WiIsCiAgICAidGltZUVsYXBzZWQiOiA2LjM3MDgxMjY4MywKICAgICJyZXF1ZXN0T2Zmc2V0IjogIiIsCiAgICAicmVxdWVzdElkIjogIjY5NjE1ZTEwLTRmN2UtNGZiNy02MGViLWZhNzAyOTcyZDllYyIsCiAgICAiYmxvY2tIYXNoIjogIjB4Yzc3ZjczNDA3NjRiNzIxZDEyNWQ2MDg5MzliMDBmNjhkZGUzOTFiZWI2NzAxNmVkMGU3OThkY2Q0OWQ3OWVkNiIsCiAgICAiYmxvY2tOdW1iZXIiOiAiNzkyMjAiLAogICAgImN1bXVsYXRpdmVHYXNVc2VkIjogIjEwMjgyOCIsCiAgICAiZnJvbSI6ICIweGYwYWZjMTlkOTEwOGI0NDMwOTZmMGRlNGQ0NjEwOTNiMTA2YjNiY2QiLAogICAgImdhc1VzZWQiOiAiMTAyODI4IiwKICAgICJub25jZSI6ICIwIiwKICAgICJ0byI6ICIweDM1YWFjNTBmNjNlNjE0ZTE1OGYyZjMzYWFlMjNjNmM1MGVjNzU3ODUiLAogICAgInRyYW5zYWN0aW9uSGFzaCI6ICIweDgxZDZiZWQyZjdkNmU4MWM1MjZkMzRjZjY1YWViY2EwNGU2OTg2YzYzMjYyMTJiMWJlMzBkNGVlNDkzYmJlNGUiLAogICAgInRyYW5zYWN0aW9uSW5kZXgiOiAiMCIsCgogICAgInB1cmNoYXNlT3JkZXIiOiAiRUNGLTQyMjItTkMxLU5DMSIsCiAgICAib3JkZXJEYXRlIjogIjE1IGRlIG5vdmllbWJyZSBkZSAyMDIyLCAxMTo1Njo0MiBVVEMtNSIsCiAgICAic3RhdHVzIjogIkFQUFJPVkVEIiwKICAgICJidXllciI6ICJETUFSSU4iLAkKICAgICJzZWxsZXIiOiAiRUYxMiIsCQogICAgImluY290ZXJtIjoJIkNQVCIsCiAgICAiaW5jb3Rlcm1OYW1",
  "poType": "1",
  "userWallet": "0x527e08ea398d607e898eb39176894f1815bfeb0b"
}'

```

Al intentar registrar otro paso para la PO tipo 1 que ya paso a estado BOOKING REQUEST, se genera el error: "The Purchase Order has no more steps", evitando que se incurra en pasos extra.

[![29](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/29.png "29")](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/29.png "29")

Respuesta de POSTMAN:

```javascript
{
    "error": "Call failed: execution reverted: The maximum status for poType 1 is 2 BOOKING REQUEST",
    "code": "FFEC100148"
}
```

Otra función que permite el smart contract es validar estados anteriores junto con la metadata que quedo atada a este step, para esto se utiliza el POSTMAN de ParameterValidator que puede verificar los status de la PO "1111".

[![30](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/30.png "30")](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/30.png "30")

Respuesta de POSTMAN:

```javascript
{
    "metadata": "ewogICAgInB1cmNoYXNlT3JkZXIiOiAiRUNGLTQyMjItTkMxLU5DMSIsCiAgICAib3JkZXJEYXRlIjogIjE1IGRlIG5vdmllbWJyZSBkZSAyMDIyLCAxMTo1Njo0MiBVVEMtNSIsCiAgICAic3RhdHVzIjogIkJPT0tJTkdfUkVRVUVTVCIsCiAgICAiYnV5ZXIiOiAiRE1BUklOIiwJCiAgICAic2VsbGVyIjogIkVGMTIiLAkKICAgICJpbmNvdGVybSI6CSJDUFQiLAogICAgImluY290ZXJtTmFtZSI6CSJQQU5BTUEiLAogICAgImRvY3VtZW50VHlwZSI6CSJJTVAiLAogICAgImRvY3VtZW50U3RhdHVzIjogIlBlZGlkbyBFbnZpYWRvIiwKICAgICJ0b3RhbENhc2hFeGNsdWRpbmdUYXgiOiAiMzMxMDQiLAkKCiAgICAiaWQiOiAiNjU3YjExOTMtNjU1Yi00NTYyLTUyMzYtOGMwZmIwMzAyNzQ0IiwKICAgICJ0eXBlIjogIlRyYW5zYWN0aW9uU3VjY2VzcyIsCiAgICAidGltZVJlY2VpdmVkIjogIjIwMjMtMDItMjVUMDU6MjQ6MjcuMDgyNTQ0NTc3WiIsCiAgICAidGltZUVsYXBzZWQiOiA2LjM3MDgxMjY4MywKICAgICJyZXF1ZXN0T2Zmc2V0IjogIiIsCiAgICAicmVxdWVzdElkIjogIjY5NjE1ZTEwLTRmN2UtNGZiNy02MGViLWZhNzAyOTcyZDllYyIsCiAgICAiYmxvY2tIYXNoIjogIjB4Yzc3ZjczNDA3NjRiNzIxZDEyNWQ2MDg5MzliMDBmNjhkZGUzOTFiZWI2NzAxNmVkMGU3OThkY2Q0OWQ3OWVkNiIsCiAgICAiYmxvY2tOdW1iZXIiOiAiNzkyMjAiLAogICAgImN1bXVsYXRpdmVHYXNVc2VkIjogIjEwMjgyOCIsCiAgICAiZnJvbSI6ICIweGYwYWZjMTlkOTEwOGI0NDMwOTZmMGRlNGQ0NjEwOTNiMTA2YjNiY2QiLAogICAgImdhc1VzZWQiOiAiMTAyODI4IiwKICAgICJub25jZSI6ICIwIiwKICAgICJ0byI6ICIweDM1YWFjNTBmNjNlNjE0ZTE1OGYyZjMzYWFlMjNjNmM1MGVjNzU3ODUiLAogICAgInRyYW5zYWN0aW9uSGFzaCI6ICIweDgxZDZiZWQyZjdkNmU4MWM1MjZkMzRjZjY1YWViY2EwNGU2OTg2YzYzMjYyMTJiMWJlMzBkNGVlNDkzYmJlNGUiLAogICAgInRyYW5zYWN0aW9uSW5kZXgiOiAiMCIsCgogICAgInB1cmNoYXNlT3JkZXIiOiAiRUNGLTQyMjItTkMxLU5DMSIsCiAgICAib3JkZXJEYXRlIjogIjE1IGRlIG5vdmllbWJyZSBkZSAyMDIyLCAxMTo1Njo0MiBVVEMtNSIsCiAgICAic3RhdHVzIjogIkFQUFJPVkVEIiwKICAgICJidXllciI6ICJETUFSSU4iLAkKICAgICJzZWxsZXIiOiAiRUYxMiIsCQogICAgImluY290ZXJtIjoJIkNQVCIsCiAgICAiaW5jb3Rlcm1OYW1",
    "status": "2"
}

```

Código JSON para documentación:

```javascript
curl --location 'https://u0cqnbwz76-u0r2m5xw62-connect.us0-aws.kaleido.io/gateways/u0t3eu9oq8/0xc050386500b1b2c8706627bebc622c4bcd6cebff/ParameterValidator?kld-from=0x527e08ea398d607e898eb39176894f1815bfeb0b&kld-sync=true' \
--header 'Authorization: Basic dTB3anJrZzRjZTpfUHRZdDBkWWFSenZ6MDBnNU01d1p1WU13M2xMUFdhOHJMZzQwMEU1SXJ3' \
--header 'Content-Type: application/json' \
--data '{
  "input": "1111",
  "input1": "2"
}'

```

[![31](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/31.png "31")](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/31.png "31")

Respuesta de POSTMAN:

```javascript
{
    "metadata": "ewogICAgInB1cmNoYXNlT3JkZXIiOiAiRUNGLTQyMjItTkMxLU5DMSIsCiAgICAib3JkZXJEYXRlIjogIjE1IGRlIG5vdmllbWJyZSBkZSAyMDIyLCAxMTo1Njo0MiBVVEMtNSIsCiAgICAic3RhdHVzIjogIkFQUFJPVkVEIiwKICAgICJidXllciI6ICJETUFSSU4iLAkKICAgICJzZWxsZXIiOiAiRUYxMiIsCQogICAgImluY290ZXJtIjoJIkNQVCIsCiAgICAiaW5jb3Rlcm1OYW1lIjoJIlBBTkFNQSIsCiAgICAiZG9jdW1lbnRUeXBlIjoJIklNUCIsCiAgICAiZG9jdW1lbnRTdGF0dXMiOiAiUGVkaWRvIEVudmlhZG8iLAogICAgInRvdGFsQ2FzaEV4Y2x1ZGluZ1RheCI6ICIzMzEwNCIsCQoKICAgICJpZCI6ICI2NTdiMTE5My02NTViLTQ1NjItNTIzNi04YzBmYjAzMDI3NDQiLAogICAgInR5cGUiOiAiVHJhbnNhY3Rpb25TdWNjZXNzIiwKICAgICJ0aW1lUmVjZWl2ZWQiOiAiMjAyMy0wMi0yNVQwNToyNDoyNy4wODI1NDQ1NzdaIiwKICAgICJ0aW1lRWxhcHNlZCI6IDYuMzcwODEyNjgzLAogICAgInJlcXVlc3RPZmZzZXQiOiAiIiwKICAgICJyZXF1ZXN0SWQiOiAiNjk2MTVlMTAtNGY3ZS00ZmI3LTYwZWItZmE3MDI5NzJkOWVjIiwKICAgICJibG9ja0hhc2giOiAiMHhjNzdmNzM0MDc2NGI3MjFkMTI1ZDYwODkzOWIwMGY2OGRkZTM5MWJlYjY3MDE2ZWQwZTc5OGRjZDQ5ZDc5ZWQ2IiwKICAgICJibG9ja051bWJlciI6ICI3OTIyMCIsCiAgICAiY3VtdWxhdGl2ZUdhc1VzZWQiOiAiMTAyODI4IiwKICAgICJmcm9tIjogIjB4ZjBhZmMxOWQ5MTA4YjQ0MzA5NmYwZGU0ZDQ2MTA5M2IxMDZiM2JjZCIsCiAgICAiZ2FzVXNlZCI6ICIxMDI4MjgiLAogICAgIm5vbmNlIjogIjAiLAogICAgInRvIjogIjB4MzVhYWM1MGY2M2U2MTRlMTU4ZjJmMzNhYWUyM2M2YzUwZWM3NTc4NSIsCiAgICAidHJhbnNhY3Rpb25IYXNoIjogIjB4ODFkNmJlZDJmN2Q2ZTgxYzUyNmQzNGNmNjVhZWJjYTA0ZTY5ODZjNjMyNjIxMmIxYmUzMGQ0ZWU0OTNiYmU0ZSIsCiAgICAidHJhbnNhY3Rpb25JbmRleCI6ICIwIiwKCiAgICAicHVyY2hhc2VPcmRlciI6ICJFQ0YtNDIyMi1OQzEtTkMxIiwKICAgICJvcmRlckRhdGUiOiAiMTUgZGUgbm92aWVtYnJlIGRlIDIwMjIsIDExOjU2OjQyIFVUQy01IiwKICAgICJzdGF0dXMiOiAiQVBQUk9WRUQiLAogICAgImJ1eWVyIjogIkRNQVJJTiIsCQogICAgInNlbGxlciI6ICJFRjEyIiwJ",
    "status": "1"
}

```

Código JSON para documentación:

```javascript
curl --location 'https://u0cqnbwz76-u0r2m5xw62-connect.us0-aws.kaleido.io/gateways/u0t3eu9oq8/0xc050386500b1b2c8706627bebc622c4bcd6cebff/ParameterValidator?kld-from=0x527e08ea398d607e898eb39176894f1815bfeb0b&kld-sync=true' \
--header 'Authorization: Basic dTB3anJrZzRjZTpfUHRZdDBkWWFSenZ6MDBnNU01d1p1WU13M2xMUFdhOHJMZzQwMEU1SXJ3' \
--header 'Content-Type: application/json' \
--data '{
  "input": "1111",
  "input1": "1"
}'

```

También facilita que al momento de crear otros Purchase Orders con otros ID, tipo de orden y otros status diferentes, se pueda validar datos de un PO anterior o diferente al que se está trabajando en la misma instancia.

- Crear una nueva PO de tipo 3, con tres entregas parciales:

[![32](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/32.png "32")](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/32.png "32")

Respuesta de POSTMAN:

```javascript
{
    "headers": {
        "id": "112c83e0-1e2f-47f3-7402-060e93019b4f",
        "type": "TransactionSuccess",
        "timeReceived": "2023-03-16T01:53:40.679581846Z",
        "timeElapsed": 6.531904995,
        "requestOffset": "",
        "requestId": "5a3a6b65-d0e3-4b24-7537-1318bbe5ee05"
    },
    "blockHash": "0x787f5d3199ee6d16b0576aa52e7bdc6496089a0c41445b28228baea201b08a35",
    "blockNumber": "255736",
    "cumulativeGasUsed": "50177",
    "from": "0x527e08ea398d607e898eb39176894f1815bfeb0b",
    "gasUsed": "50177",
    "nonce": "0",
    "status": "1",
    "to": "0xc050386500b1b2c8706627bebc622c4bcd6cebff",
    "transactionHash": "0x3dc96f9cf9b70745af6528a45efdfad034aef1a186b67225e31bde6313b7fda9",
    "transactionIndex": "0"
}

```

Código JSON para documentación:

```javascript
curl --location 'https://u0cqnbwz76-u0r2m5xw62-connect.us0-aws.kaleido.io/gateways/u0t3eu9oq8/0xc050386500b1b2c8706627bebc622c4bcd6cebff/RegisterPO?kld-from=0x527e08ea398d607e898eb39176894f1815bfeb0b&kld-sync=true' \
--header 'Authorization: Basic dTB3anJrZzRjZTpfUHRZdDBkWWFSenZ6MDBnNU01d1p1WU13M2xMUFdhOHJMZzQwMEU1SXJ3' \
--header 'Content-Type: application/json' \
--data '{
  "POID": "3333",
  "userWallet": "0x527e08ea398d607e898eb39176894f1815bfeb0b"
}'

```

- Registrar un siguiente step del status Approved(1) de la PO "3333" tipo 3:

[![33](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/33.png "33")](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/33.png "33")

Respuesta de POSTMAN:

```javascript
{
    "headers": {
        "id": "00f0f481-f774-4b82-53e2-96b73f393e15",
        "type": "TransactionSuccess",
        "timeReceived": "2023-03-16T01:58:57.191665375Z",
        "timeElapsed": 6.432358051,
        "requestOffset": "",
        "requestId": "dccea9ba-3f60-47db-5738-85bcb3985b9f"
    },
    "blockHash": "0xebca3146912c2edb28b8d5b5119ef982be40f8cb199bbf55513cae0fe15039a3",
    "blockNumber": "255800",
    "cumulativeGasUsed": "1257087",
    "from": "0x527e08ea398d607e898eb39176894f1815bfeb0b",
    "gasUsed": "1257087",
    "nonce": "0",
    "status": "1",
    "to": "0xc050386500b1b2c8706627bebc622c4bcd6cebff",
    "transactionHash": "0xe147390fb1bdb4f9f572268ce12176bcf9c7b8340c6ae4524bd26d4ebecb81ad",
    "transactionIndex": "0"
}

```

Código JSON para documentación:

```javascript
curl --location 'https://u0cqnbwz76-u0r2m5xw62-connect.us0-aws.kaleido.io/gateways/u0t3eu9oq8/0xc050386500b1b2c8706627bebc622c4bcd6cebff/RegisterStep?kld-from=0x527e08ea398d607e898eb39176894f1815bfeb0b&kld-sync=true' \
--header 'Authorization: Basic dTB3anJrZzRjZTpfUHRZdDBkWWFSenZ6MDBnNU01d1p1WU13M2xMUFdhOHJMZzQwMEU1SXJ3' \
--header 'Content-Type: application/json' \
--data '{
  "POID": "3333",
  "metadata": "ewogICAgInB1cmNoYXNlT3JkZXIiOiAiRUNGLTQyMjItTkMxLU5DMSIsCiAgICAib3JkZXJEYXRlIjogIjE1IGRlIG5vdmllbWJyZSBkZSAyMDIyLCAxMTo1Njo0MiBVVEMtNSIsCiAgICAic3RhdHVzIjogIkFQUFJPVkVEX1AxIiwKICAgICJidXllciI6ICJETUFSSU4iLAkKICAgICJzZWxsZXIiOiAiRUYxMiIsCQogICAgImluY290ZXJtIjoJIkNQVCIsCiAgICAiaW5jb3Rlcm1OYW1lIjoJIlBBTkFNQSIsCiAgICAiZG9jdW1lbnRUeXBlIjoJIklNUCIsCiAgICAiZG9jdW1lbnRTdGF0dXMiOiAiUGVkaWRvIEVudmlhZG8iLAogICAgInRvdGFsQ2FzaEV4Y2x1ZGluZ1RheCI6ICIzMzEwNCIsCQoKICAgICJpZCI6ICI2NTdiMTE5My02NTViLTQ1NjItNTIzNi04YzBmYjAzMDI3NDQiLAogICAgInR5cGUiOiAiVHJhbnNhY3Rpb25TdWNjZXNzIiwKICAgICJ0aW1lUmVjZWl2ZWQiOiAiMjAyMy0wMi0yNVQwNToyNDoyNy4wODI1NDQ1NzdaIiwKICAgICJ0aW1lRWxhcHNlZCI6IDYuMzcwODEyNjgzLAogICAgInJlcXVlc3RPZmZzZXQiOiAiIiwKICAgICJyZXF1ZXN0SWQiOiAiNjk2MTVlMTAtNGY3ZS00ZmI3LTYwZWItZmE3MDI5NzJkOWVjIiwKICAgICJibG9ja0hhc2giOiAiMHhjNzdmNzM0MDc2NGI3MjFkMTI1ZDYwODkzOWIwMGY2OGRkZTM5MWJlYjY3MDE2ZWQwZTc5OGRjZDQ5ZDc5ZWQ2IiwKICAgICJibG9ja051bWJlciI6ICI3OTIyMCIsCiAgICAiY3VtdWxhdGl2ZUdhc1VzZWQiOiAiMTAyODI4IiwKICAgICJmcm9tIjogIjB4ZjBhZmMxOWQ5MTA4YjQ0MzA5NmYwZGU0ZDQ2MTA5M2IxMDZiM2JjZCIsCiAgICAiZ2FzVXNlZCI6ICIxMDI4MjgiLAogICAgIm5vbmNlIjogIjAiLAogICAgInRvIjogIjB4MzVhYWM1MGY2M2U2MTRlMTU4ZjJmMzNhYWUyM2M2YzUwZWM3NTc4NSIsCiAgICAidHJhbnNhY3Rpb25IYXNoIjogIjB4ODFkNmJlZDJmN2Q2ZTgxYzUyNmQzNGNmNjVhZWJjYTA0ZTY5ODZjNjMyNjIxMmIxYmUzMGQ0ZWU0OTNiYmU0ZSIsCiAgICAidHJhbnNhY3Rpb25JbmRleCI6ICIwIiwKCiAgICAicHVyY2hhc2VPcmRlciI6ICJFQ0YtNDIyMi1OQzEtTkMxIiwKICAgICJvcmRlckRhdGUiOiAiMTUgZGUgbm92aWVtYnJlIGRlIDIwMjIsIDExOjU2OjQyIFVUQy01IiwKICAgICJzdGF0dXMiOiAiQVBQUk9WRUQiLAogICAgImJ1eWVyIjogIkRNQVJJTiIsCQogICAgInNlbGxlciI6ICJFRjEyIiwJCiAgICAiaW5jb3Rlcm0iOgkiQ1BUIgp9",
  "poType": "3",
  "userWallet": "0x527e08ea398d607e898eb39176894f1815bfeb0b"
}'

```

- Registrar un siguiente step del status To be confirmed P2(2) de la PO "3333" tipo 3:

[![51](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/51.png "51")](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/51.png "51")

Respuesta de POSTMAN:

```javascript
{
    "headers": {
        "id": "2265ca73-d9e1-4ebf-4fbc-276bf5c6e6a7",
        "type": "TransactionSuccess",
        "timeReceived": "2023-03-16T02:05:43.837486638Z",
        "timeElapsed": 6.337811684,
        "requestOffset": "",
        "requestId": "2b598862-8154-4945-5829-0050df3cc502"
    },
    "blockHash": "0x23f1dc8341ca409b9d9cc8e0b4020447c196ea9b8bfac80253c4bf99ccb4e964",
    "blockNumber": "255881",
    "cumulativeGasUsed": "1394709",
    "from": "0x527e08ea398d607e898eb39176894f1815bfeb0b",
    "gasUsed": "1394709",
    "nonce": "0",
    "status": "1",
    "to": "0xc050386500b1b2c8706627bebc622c4bcd6cebff",
    "transactionHash": "0xc63f9f2eb524883d0f0a0fd4f460b63014c328e29b27677ca3b814dd2c03cb04",
    "transactionIndex": "0"
}

```

Código JSON para documentación:

```javascript
curl --location 'https://u0cqnbwz76-u0r2m5xw62-connect.us0-aws.kaleido.io/gateways/u0t3eu9oq8/0xc050386500b1b2c8706627bebc622c4bcd6cebff/RegisterStep?kld-from=0x527e08ea398d607e898eb39176894f1815bfeb0b&kld-sync=true' \
--header 'Authorization: Basic dTB3anJrZzRjZTpfUHRZdDBkWWFSenZ6MDBnNU01d1p1WU13M2xMUFdhOHJMZzQwMEU1SXJ3' \
--header 'Content-Type: application/json' \
--data '{
  "POID": "3333",
  "metadata": "ewogICAgInB1cmNoYXNlT3JkZXIiOiAiRUNGLTQyMjItTkMxLU5DMSIsCiAgICAib3JkZXJEYXRlIjogIjE1IGRlIG5vdmllbWJyZSBkZSAyMDIyLCAxMTo1Njo0MiBVVEMtNSIsCiAgICAic3RhdHVzIjogIlRPX0JFX0NPTkZJUk1FRF9QMiIsCiAgICAiYnV5ZXIiOiAiRE1BUklOIiwJCiAgICAic2VsbGVyIjogIkVGMTIiLAkKICAgICJpbmNvdGVybSI6CSJDUFQiLAogICAgImluY290ZXJtTmFtZSI6CSJQQU5BTUEiLAogICAgImRvY3VtZW50VHlwZSI6CSJJTVAiLAogICAgImRvY3VtZW50U3RhdHVzIjogIlBlZGlkbyBFbnZpYWRvIiwKICAgICJ0b3RhbENhc2hFeGNsdWRpbmdUYXgiOiAiMzMxMDQiLAkKCiAgICAiaWQiOiAiNjU3YjExOTMtNjU1Yi00NTYyLTUyMzYtOGMwZmIwMzAyNzQ0IiwKICAgICJ0eXBlIjogIlRyYW5zYWN0aW9uU3VjY2VzcyIsCiAgICAidGltZVJlY2VpdmVkIjogIjIwMjMtMDItMjVUMDU6MjQ6MjcuMDgyNTQ0NTc3WiIsCiAgICAidGltZUVsYXBzZWQiOiA2LjM3MDgxMjY4MywKICAgICJyZXF1ZXN0T2Zmc2V0IjogIiIsCiAgICAicmVxdWVzdElkIjogIjY5NjE1ZTEwLTRmN2UtNGZiNy02MGViLWZhNzAyOTcyZDllYyIsCiAgICAiYmxvY2tIYXNoIjogIjB4Yzc3ZjczNDA3NjRiNzIxZDEyNWQ2MDg5MzliMDBmNjhkZGUzOTFiZWI2NzAxNmVkMGU3OThkY2Q0OWQ3OWVkNiIsCiAgICAiYmxvY2tOdW1iZXIiOiAiNzkyMjAiLAogICAgImN1bXVsYXRpdmVHYXNVc2VkIjogIjEwMjgyOCIsCiAgICAiZnJvbSI6ICIweGYwYWZjMTlkOTEwOGI0NDMwOTZmMGRlNGQ0NjEwOTNiMTA2YjNiY2QiLAogICAgImdhc1VzZWQiOiAiMTAyODI4IiwKICAgICJub25jZSI6ICIwIiwKICAgICJ0byI6ICIweDM1YWFjNTBmNjNlNjE0ZTE1OGYyZjMzYWFlMjNjNmM1MGVjNzU3ODUiLAogICAgInRyYW5zYWN0aW9uSGFzaCI6ICIweDgxZDZiZWQyZjdkNmU4MWM1MjZkMzRjZjY1YWViY2EwNGU2OTg2YzYzMjYyMTJiMWJlMzBkNGVlNDkzYmJlNGUiLAogICAgInRyYW5zYWN0aW9uSW5kZXgiOiAiMCIsCgogICAgInB1cmNoYXNlT3JkZXIiOiAiRUNGLTQyMjItTkMxLU5DMSIsCiAgICAib3JkZXJEYXRlIjogIjE1IGRlIG5vdmllbWJyZSBkZSAyMDIyLCAxMTo1Njo0MiBVVEMtNSIsCiAgICAic3RhdHVzIjogIkFQUFJPVkVEIiwKICAgICJidXllciI6ICJETUFSSU4iLAkKICAgICJzZWxsZXIiOiAiRUYxMiIsCQogICAgImluY290ZXJtIjoJIkNQVCIKfQ==",
  "poType": "3",
  "userWallet": "0x527e08ea398d607e898eb39176894f1815bfeb0b"
}'
```

- Registrar un siguiente step del status Approved P2(3) de la PO "3333" tipo 3:

[![52](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/52.png "52")](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/52.png "52")

Respuesta de POSTMAN:

```javascript
{
    "headers": {
        "id": "20f2713d-9f2a-474d-7a29-b58deec005f6",
        "type": "TransactionSuccess",
        "timeReceived": "2023-03-16T02:08:49.846733188Z",
        "timeElapsed": 6.254659244,
        "requestOffset": "",
        "requestId": "de543d0e-9ca2-480c-6dfc-0b60105a99bc"
    },
    "blockHash": "0xc06a85b77ef1280cfe9130134698dc03f0258318169944eccd033fb8711c3562",
    "blockNumber": "255918",
    "cumulativeGasUsed": "1489027",
    "from": "0x527e08ea398d607e898eb39176894f1815bfeb0b",
    "gasUsed": "1489027",
    "nonce": "0",
    "status": "1",
    "to": "0xc050386500b1b2c8706627bebc622c4bcd6cebff",
    "transactionHash": "0xecad2a24e9bc3de593f0df03e304dd191af8dd8528476fedc1aa46863e7a35fc",
    "transactionIndex": "0"
}

```

Código JSON para documentación:

```javascript
curl --location 'https://u0cqnbwz76-u0r2m5xw62-connect.us0-aws.kaleido.io/gateways/u0t3eu9oq8/0xc050386500b1b2c8706627bebc622c4bcd6cebff/RegisterStep?kld-from=0x527e08ea398d607e898eb39176894f1815bfeb0b&kld-sync=true' \
--header 'Authorization: Basic dTB3anJrZzRjZTpfUHRZdDBkWWFSenZ6MDBnNU01d1p1WU13M2xMUFdhOHJMZzQwMEU1SXJ3' \
--header 'Content-Type: application/json' \
--data '{
  "POID": "3333",
  "metadata": "ewogICAgInB1cmNoYXNlT3JkZXIiOiAiRUNGLTQyMjItTkMxLU5DMSIsCiAgICAib3JkZXJEYXRlIjogIjE1IGRlIG5vdmllbWJyZSBkZSAyMDIyLCAxMTo1Njo0MiBVVEMtNSIsCiAgICAic3RhdHVzIjogIkFQUFJPVkVEX1AyIiwKICAgICJidXllciI6ICJETUFSSU4iLAkKICAgICJzZWxsZXIiOiAiRUYxMiIsCQogICAgImluY290ZXJtIjoJIkNQVCIsCiAgICAiaW5jb3Rlcm1OYW1lIjoJIlBBTkFNQSIsCiAgICAiZG9jdW1lbnRUeXBlIjoJIklNUCIsCiAgICAiZG9jdW1lbnRTdGF0dXMiOiAiUGVkaWRvIEVudmlhZG8iLAogICAgInRvdGFsQ2FzaEV4Y2x1ZGluZ1RheCI6ICIzMzEwNCIsCQoKICAgICJpZCI6ICI2NTdiMTE5My02NTViLTQ1NjItNTIzNi04YzBmYjAzMDI3NDQiLAogICAgInR5cGUiOiAiVHJhbnNhY3Rpb25TdWNjZXNzIiwKICAgICJ0aW1lUmVjZWl2ZWQiOiAiMjAyMy0wMi0yNVQwNToyNDoyNy4wODI1NDQ1NzdaIiwKICAgICJ0aW1lRWxhcHNlZCI6IDYuMzcwODEyNjgzLAogICAgInJlcXVlc3RPZmZzZXQiOiAiIiwKICAgICJyZXF1ZXN0SWQiOiAiNjk2MTVlMTAtNGY3ZS00ZmI3LTYwZWItZmE3MDI5NzJkOWVjIiwKICAgICJibG9ja0hhc2giOiAiMHhjNzdmNzM0MDc2NGI3MjFkMTI1ZDYwODkzOWIwMGY2OGRkZTM5MWJlYjY3MDE2ZWQwZTc5OGRjZDQ5ZDc5ZWQ2IiwKICAgICJibG9ja051bWJlciI6ICI3OTIyMCIsCiAgICAiY3VtdWxhdGl2ZUdhc1VzZWQiOiAiMTAyODI4IiwKICAgICJmcm9tIjogIjB4ZjBhZmMxOWQ5MTA4YjQ0MzA5NmYwZGU0ZDQ2MTA5M2IxMDZiM2JjZCIsCiAgICAiZ2FzVXNlZCI6ICIxMDI4MjgiLAogICAgIm5vbmNlIjogIjAiLAogICAgInRvIjogIjB4MzVhYWM1MGY2M2U2MTRlMTU4ZjJmMzNhYWUyM2M2YzUwZWM3NTc4NSIsCiAgICAidHJhbnNhY3Rpb25IYXNoIjogIjB4ODFkNmJlZDJmN2Q2ZTgxYzUyNmQzNGNmNjVhZWJjYTA0ZTY5ODZjNjMyNjIxMmIxYmUzMGQ0ZWU0OTNiYmU0ZSIsCiAgICAidHJhbnNhY3Rpb25JbmRleCI6ICIwIiwKCiAgICAicHVyY2hhc2VPcmRlciI6ICJFQ0YtNDIyMi1OQzEtTkMxIiwKICAgICJvcmRlckRhdGUiOiAiMTUgZGUgbm92aWVtYnJlIGRlIDIwMjIsIDExOjU2OjQyIFVUQy01IiwKICAgICJzdGF0dXMiOiAiQVBQUk9WRUQiLAogICAgImJ1eWVyIjogIkRNQVJJTiIsCQogICAgInNlbGxlciI6ICJFRjEyIiwJCiAgICAiaW5jb3Rlcm0iOgkiQ1BUIgp9",
  "poType": "3",
  "userWallet": "0x527e08ea398d607e898eb39176894f1815bfeb0b"
}'

```

- Registrar un siguiente step del status To be cofirmed P3(4) de la PO "3333" tipo 3:

[![54](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/54.png "54")](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/54.png "54")

Respuesta de POSTMAN:

```javascript
{
    "headers": {
        "id": "1b8c0731-7b93-4a05-584e-0974b15adebb",
        "type": "TransactionSuccess",
        "timeReceived": "2023-03-16T02:14:48.093856106Z",
        "timeElapsed": 6.178342535,
        "requestOffset": "",
        "requestId": "4fd38fd4-d197-4fb5-4ac3-2e31f4fb051b"
    },
    "blockHash": "0xe10d650299abfcf19766c3dad5ff16e99fb2947e38fa45c3cc46b15d037b88e3",
    "blockNumber": "255990",
    "cumulativeGasUsed": "1626674",
    "from": "0x527e08ea398d607e898eb39176894f1815bfeb0b",
    "gasUsed": "1626674",
    "nonce": "0",
    "status": "1",
    "to": "0xc050386500b1b2c8706627bebc622c4bcd6cebff",
    "transactionHash": "0xeba577f40694c71277d3b821f5ff55b335f2f0551fa7831ec6d1cb30f4860b4a",
    "transactionIndex": "0"
}

```

Código JSON para documentación:

```javascript
curl --location 'https://u0cqnbwz76-u0r2m5xw62-connect.us0-aws.kaleido.io/gateways/u0t3eu9oq8/0xc050386500b1b2c8706627bebc622c4bcd6cebff/RegisterStep?kld-from=0x527e08ea398d607e898eb39176894f1815bfeb0b&kld-sync=true' \
--header 'Authorization: Basic dTB3anJrZzRjZTpfUHRZdDBkWWFSenZ6MDBnNU01d1p1WU13M2xMUFdhOHJMZzQwMEU1SXJ3' \
--header 'Content-Type: application/json' \
--data '{
  "POID": "3333",
  "metadata": "ewogICAgInB1cmNoYXNlT3JkZXIiOiAiRUNGLTQyMjItTkMxLU5DMSIsCiAgICAib3JkZXJEYXRlIjogIjE1IGRlIG5vdmllbWJyZSBkZSAyMDIyLCAxMTo1Njo0MiBVVEMtNSIsCiAgICAic3RhdHVzIjogIlRPX0JFX0NPTkZJUk1FRF9QMyIsCiAgICAiYnV5ZXIiOiAiRE1BUklOIiwJCiAgICAic2VsbGVyIjogIkVGMTIiLAkKICAgICJpbmNvdGVybSI6CSJDUFQiLAogICAgImluY290ZXJtTmFtZSI6CSJQQU5BTUEiLAogICAgImRvY3VtZW50VHlwZSI6CSJJTVAiLAogICAgImRvY3VtZW50U3RhdHVzIjogIlBlZGlkbyBFbnZpYWRvIiwKICAgICJ0b3RhbENhc2hFeGNsdWRpbmdUYXgiOiAiMzMxMDQiLAkKCiAgICAiaWQiOiAiNjU3YjExOTMtNjU1Yi00NTYyLTUyMzYtOGMwZmIwMzAyNzQ0IiwKICAgICJ0eXBlIjogIlRyYW5zYWN0aW9uU3VjY2VzcyIsCiAgICAidGltZVJlY2VpdmVkIjogIjIwMjMtMDItMjVUMDU6MjQ6MjcuMDgyNTQ0NTc3WiIsCiAgICAidGltZUVsYXBzZWQiOiA2LjM3MDgxMjY4MywKICAgICJyZXF1ZXN0T2Zmc2V0IjogIiIsCiAgICAicmVxdWVzdElkIjogIjY5NjE1ZTEwLTRmN2UtNGZiNy02MGViLWZhNzAyOTcyZDllYyIsCiAgICAiYmxvY2tIYXNoIjogIjB4Yzc3ZjczNDA3NjRiNzIxZDEyNWQ2MDg5MzliMDBmNjhkZGUzOTFiZWI2NzAxNmVkMGU3OThkY2Q0OWQ3OWVkNiIsCiAgICAiYmxvY2tOdW1iZXIiOiAiNzkyMjAiLAogICAgImN1bXVsYXRpdmVHYXNVc2VkIjogIjEwMjgyOCIsCiAgICAiZnJvbSI6ICIweGYwYWZjMTlkOTEwOGI0NDMwOTZmMGRlNGQ0NjEwOTNiMTA2YjNiY2QiLAogICAgImdhc1VzZWQiOiAiMTAyODI4IiwKICAgICJub25jZSI6ICIwIiwKICAgICJ0byI6ICIweDM1YWFjNTBmNjNlNjE0ZTE1OGYyZjMzYWFlMjNjNmM1MGVjNzU3ODUiLAogICAgInRyYW5zYWN0aW9uSGFzaCI6ICIweDgxZDZiZWQyZjdkNmU4MWM1MjZkMzRjZjY1YWViY2EwNGU2OTg2YzYzMjYyMTJiMWJlMzBkNGVlNDkzYmJlNGUiLAogICAgInRyYW5zYWN0aW9uSW5kZXgiOiAiMCIsCgogICAgInB1cmNoYXNlT3JkZXIiOiAiRUNGLTQyMjItTkMxLU5DMSIsCiAgICAib3JkZXJEYXRlIjogIjE1IGRlIG5vdmllbWJyZSBkZSAyMDIyLCAxMTo1Njo0MiBVVEMtNSIsCiAgICAic3RhdHVzIjogIkFQUFJPVkVEIiwKICAgICJidXllciI6ICJETUFSSU4iLAkKICAgICJzZWxsZXIiOiAiRUYxMiIsCQogICAgImluY290ZXJtIjoJIkNQVCIKfQ==",
  "poType": "3",
  "userWallet": "0x527e08ea398d607e898eb39176894f1815bfeb0b"
}'

```

- Registrar un siguiente step del status Approved P3(5) de la PO "3333" tipo 3:

[![55](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/55.png "54")](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/55.png "55")

Respuesta de POSTMAN:

```javascript
{
    "headers": {
        "id": "790fd8ef-a042-4e57-5d8a-4aa7e1171fbe",
        "type": "TransactionSuccess",
        "timeReceived": "2023-03-16T02:18:33.536713458Z",
        "timeElapsed": 6.12331346,
        "requestOffset": "",
        "requestId": "bafbcf90-1c10-4e2b-52d2-8b82c7f5fdc1"
    },
    "blockHash": "0x635aac6e16106b1314f49a1f8445633ec16ff8c6fff6db6434e2d38ebbbbdf5c",
    "blockNumber": "256035",
    "cumulativeGasUsed": "1721014",
    "from": "0x527e08ea398d607e898eb39176894f1815bfeb0b",
    "gasUsed": "1721014",
    "nonce": "0",
    "status": "1",
    "to": "0xc050386500b1b2c8706627bebc622c4bcd6cebff",
    "transactionHash": "0x1e36e56007d1937b9ffd6b9c870fba2ce9387bc76979bb559dad733f7bbf2a08",
    "transactionIndex": "0"
}
```

Código JSON para documentación:

```javascript
curl --location 'https://u0cqnbwz76-u0r2m5xw62-connect.us0-aws.kaleido.io/gateways/u0t3eu9oq8/0xc050386500b1b2c8706627bebc622c4bcd6cebff/RegisterStep?kld-from=0x527e08ea398d607e898eb39176894f1815bfeb0b&kld-sync=true' \
--header 'Authorization: Basic dTB3anJrZzRjZTpfUHRZdDBkWWFSenZ6MDBnNU01d1p1WU13M2xMUFdhOHJMZzQwMEU1SXJ3' \
--header 'Content-Type: application/json' \
--data '{
  "POID": "3333",
  "metadata": "ewogICAgInB1cmNoYXNlT3JkZXIiOiAiRUNGLTQyMjItTkMxLU5DMSIsCiAgICAib3JkZXJEYXRlIjogIjE1IGRlIG5vdmllbWJyZSBkZSAyMDIyLCAxMTo1Njo0MiBVVEMtNSIsCiAgICAic3RhdHVzIjogIkFQUFJPVkVEX1AzIiwKICAgICJidXllciI6ICJETUFSSU4iLAkKICAgICJzZWxsZXIiOiAiRUYxMiIsCQogICAgImluY290ZXJtIjoJIkNQVCIsCiAgICAiaW5jb3Rlcm1OYW1lIjoJIlBBTkFNQSIsCiAgICAiZG9jdW1lbnRUeXBlIjoJIklNUCIsCiAgICAiZG9jdW1lbnRTdGF0dXMiOiAiUGVkaWRvIEVudmlhZG8iLAogICAgInRvdGFsQ2FzaEV4Y2x1ZGluZ1RheCI6ICIzMzEwNCIsCQoKICAgICJpZCI6ICI2NTdiMTE5My02NTViLTQ1NjItNTIzNi04YzBmYjAzMDI3NDQiLAogICAgInR5cGUiOiAiVHJhbnNhY3Rpb25TdWNjZXNzIiwKICAgICJ0aW1lUmVjZWl2ZWQiOiAiMjAyMy0wMi0yNVQwNToyNDoyNy4wODI1NDQ1NzdaIiwKICAgICJ0aW1lRWxhcHNlZCI6IDYuMzcwODEyNjgzLAogICAgInJlcXVlc3RPZmZzZXQiOiAiIiwKICAgICJyZXF1ZXN0SWQiOiAiNjk2MTVlMTAtNGY3ZS00ZmI3LTYwZWItZmE3MDI5NzJkOWVjIiwKICAgICJibG9ja0hhc2giOiAiMHhjNzdmNzM0MDc2NGI3MjFkMTI1ZDYwODkzOWIwMGY2OGRkZTM5MWJlYjY3MDE2ZWQwZTc5OGRjZDQ5ZDc5ZWQ2IiwKICAgICJibG9ja051bWJlciI6ICI3OTIyMCIsCiAgICAiY3VtdWxhdGl2ZUdhc1VzZWQiOiAiMTAyODI4IiwKICAgICJmcm9tIjogIjB4ZjBhZmMxOWQ5MTA4YjQ0MzA5NmYwZGU0ZDQ2MTA5M2IxMDZiM2JjZCIsCiAgICAiZ2FzVXNlZCI6ICIxMDI4MjgiLAogICAgIm5vbmNlIjogIjAiLAogICAgInRvIjogIjB4MzVhYWM1MGY2M2U2MTRlMTU4ZjJmMzNhYWUyM2M2YzUwZWM3NTc4NSIsCiAgICAidHJhbnNhY3Rpb25IYXNoIjogIjB4ODFkNmJlZDJmN2Q2ZTgxYzUyNmQzNGNmNjVhZWJjYTA0ZTY5ODZjNjMyNjIxMmIxYmUzMGQ0ZWU0OTNiYmU0ZSIsCiAgICAidHJhbnNhY3Rpb25JbmRleCI6ICIwIiwKCiAgICAicHVyY2hhc2VPcmRlciI6ICJFQ0YtNDIyMi1OQzEtTkMxIiwKICAgICJvcmRlckRhdGUiOiAiMTUgZGUgbm92aWVtYnJlIGRlIDIwMjIsIDExOjU2OjQyIFVUQy01IiwKICAgICJzdGF0dXMiOiAiQVBQUk9WRUQiLAogICAgImJ1eWVyIjogIkRNQVJJTiIsCQogICAgInNlbGxlciI6ICJFRjEyIiwJCiAgICAiaW5jb3Rlcm0iOgkiQ1BUIgp9",
  "poType": "3",
  "userWallet": "0x527e08ea398d607e898eb39176894f1815bfeb0b"
}'

```

- Registrar un siguiente step del status Booking Request(6) de la PO "3333" tipo 3:

[![56](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/56.png "56")](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/56.png "56")

Respuesta de POSTMAN:

```javascript
{
    "headers": {
        "id": "747a6af8-6f0d-465c-574c-c5562a8002a7",
        "type": "TransactionSuccess",
        "timeReceived": "2023-03-16T02:22:23.137932931Z",
        "timeElapsed": 6.040487098,
        "requestOffset": "",
        "requestId": "19892fa5-e0e4-4ec6-52ae-a09e0592700d"
    },
    "blockHash": "0xe1def165e19ff9ea01e4465e3922b2964249e7a7fb78221f1a48a0498badcc9d",
    "blockNumber": "256081",
    "cumulativeGasUsed": "1835986",
    "from": "0x527e08ea398d607e898eb39176894f1815bfeb0b",
    "gasUsed": "1835986",
    "nonce": "0",
    "status": "1",
    "to": "0xc050386500b1b2c8706627bebc622c4bcd6cebff",
    "transactionHash": "0x42e1ab441db045b0ba3155c8ac9c060d752b817f1cce5e74ac8346f5db1a24b8",
    "transactionIndex": "0"
}
```

Código JSON para documentación:

```javascript
curl --location 'https://u0cqnbwz76-u0r2m5xw62-connect.us0-aws.kaleido.io/gateways/u0t3eu9oq8/0xc050386500b1b2c8706627bebc622c4bcd6cebff/RegisterStep?kld-from=0x527e08ea398d607e898eb39176894f1815bfeb0b&kld-sync=true' \
--header 'Authorization: Basic dTB3anJrZzRjZTpfUHRZdDBkWWFSenZ6MDBnNU01d1p1WU13M2xMUFdhOHJMZzQwMEU1SXJ3' \
--header 'Content-Type: application/json' \
--data '{
  "POID": "3333",
  "metadata": "ewogICAgInB1cmNoYXNlT3JkZXIiOiAiRUNGLTQyMjItTkMxLU5DMSIsCiAgICAib3JkZXJEYXRlIjogIjE1IGRlIG5vdmllbWJyZSBkZSAyMDIyLCAxMTo1Njo0MiBVVEMtNSIsCiAgICAic3RhdHVzIjogIkJPT0tJTkdfUkVRVUVTVCIsCiAgICAiYnV5ZXIiOiAiRE1BUklOIiwJCiAgICAic2VsbGVyIjogIkVGMTIiLAkKICAgICJpbmNvdGVybSI6CSJDUFQiLAogICAgImluY290ZXJtTmFtZSI6CSJQQU5BTUEiLAogICAgImRvY3VtZW50VHlwZSI6CSJJTVAiLAogICAgImRvY3VtZW50U3RhdHVzIjogIlBlZGlkbyBFbnZpYWRvIiwKICAgICJ0b3RhbENhc2hFeGNsdWRpbmdUYXgiOiAiMzMxMDQiLAkKCiAgICAiaWQiOiAiNjU3YjExOTMtNjU1Yi00NTYyLTUyMzYtOGMwZmIwMzAyNzQ0IiwKICAgICJ0eXBlIjogIlRyYW5zYWN0aW9uU3VjY2VzcyIsCiAgICAidGltZVJlY2VpdmVkIjogIjIwMjMtMDItMjVUMDU6MjQ6MjcuMDgyNTQ0NTc3WiIsCiAgICAidGltZUVsYXBzZWQiOiA2LjM3MDgxMjY4MywKICAgICJyZXF1ZXN0T2Zmc2V0IjogIiIsCiAgICAicmVxdWVzdElkIjogIjY5NjE1ZTEwLTRmN2UtNGZiNy02MGViLWZhNzAyOTcyZDllYyIsCiAgICAiYmxvY2tIYXNoIjogIjB4Yzc3ZjczNDA3NjRiNzIxZDEyNWQ2MDg5MzliMDBmNjhkZGUzOTFiZWI2NzAxNmVkMGU3OThkY2Q0OWQ3OWVkNiIsCiAgICAiYmxvY2tOdW1iZXIiOiAiNzkyMjAiLAogICAgImN1bXVsYXRpdmVHYXNVc2VkIjogIjEwMjgyOCIsCiAgICAiZnJvbSI6ICIweGYwYWZjMTlkOTEwOGI0NDMwOTZmMGRlNGQ0NjEwOTNiMTA2YjNiY2QiLAogICAgImdhc1VzZWQiOiAiMTAyODI4IiwKICAgICJub25jZSI6ICIwIiwKICAgICJ0byI6ICIweDM1YWFjNTBmNjNlNjE0ZTE1OGYyZjMzYWFlMjNjNmM1MGVjNzU3ODUiLAogICAgInRyYW5zYWN0aW9uSGFzaCI6ICIweDgxZDZiZWQyZjdkNmU4MWM1MjZkMzRjZjY1YWViY2EwNGU2OTg2YzYzMjYyMTJiMWJlMzBkNGVlNDkzYmJlNGUiLAogICAgInRyYW5zYWN0aW9uSW5kZXgiOiAiMCIsCgogICAgInB1cmNoYXNlT3JkZXIiOiAiRUNGLTQyMjItTkMxLU5DMSIsCiAgICAib3JkZXJEYXRlIjogIjE1IGRlIG5vdmllbWJyZSBkZSAyMDIyLCAxMTo1Njo0MiBVVEMtNSIsCiAgICAic3RhdHVzIjogIkFQUFJPVkVEIiwKICAgICJidXllciI6ICJETUFSSU4iLAkKICAgICJzZWxsZXIiOiAiRUYxMiIsCQogICAgImluY290ZXJtIjoJIkNQVCIKfQ==",
  "poType": "3",
  "userWallet": "0x527e08ea398d607e898eb39176894f1815bfeb0b"
}'
```

- Al ser una orden de tipo 3 se entiende que el máximo estado al que debe llegar es Booking Request(6), al intentar agregar un step 7 se genera el error: The Purchase Order has no more steps.

[![57](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/57.png "57")](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/57.png "57")

- Consultar parámetros guardados en el contrato de acuerdo al status To be confirmed P2(2) de la PO "3333" tipo 3:

[![34](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/34.png "34")](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/34.png "34")

Respuesta de POSTMAN:

```javascript
{
    "metadata": "ewogICAgInB1cmNoYXNlT3JkZXIiOiAiRUNGLTQyMjItTkMxLU5DMSIsCiAgICAib3JkZXJEYXRlIjogIjE1IGRlIG5vdmllbWJyZSBkZSAyMDIyLCAxMTo1Njo0MiBVVEMtNSIsCiAgICAic3RhdHVzIjogIlRPX0JFX0NPTkZJUk1FRF9QMiIsCiAgICAiYnV5ZXIiOiAiRE1BUklOIiwJCiAgICAic2VsbGVyIjogIkVGMTIiLAkKICAgICJpbmNvdGVybSI6CSJDUFQiLAogICAgImluY290ZXJtTmFtZSI6CSJQQU5BTUEiLAogICAgImRvY3VtZW50VHlwZSI6CSJJTVAiLAogICAgImRvY3VtZW50U3RhdHVzIjogIlBlZGlkbyBFbnZpYWRvIiwKICAgICJ0b3RhbENhc2hFeGNsdWRpbmdUYXgiOiAiMzMxMDQiLAkKCiAgICAiaWQiOiAiNjU3YjExOTMtNjU1Yi00NTYyLTUyMzYtOGMwZmIwMzAyNzQ0IiwKICAgICJ0eXBlIjogIlRyYW5zYWN0aW9uU3VjY2VzcyIsCiAgICAidGltZVJlY2VpdmVkIjogIjIwMjMtMDItMjVUMDU6MjQ6MjcuMDgyNTQ0NTc3WiIsCiAgICAidGltZUVsYXBzZWQiOiA2LjM3MDgxMjY4MywKICAgICJyZXF1ZXN0T2Zmc2V0IjogIiIsCiAgICAicmVxdWVzdElkIjogIjY5NjE1ZTEwLTRmN2UtNGZiNy02MGViLWZhNzAyOTcyZDllYyIsCiAgICAiYmxvY2tIYXNoIjogIjB4Yzc3ZjczNDA3NjRiNzIxZDEyNWQ2MDg5MzliMDBmNjhkZGUzOTFiZWI2NzAxNmVkMGU3OThkY2Q0OWQ3OWVkNiIsCiAgICAiYmxvY2tOdW1iZXIiOiAiNzkyMjAiLAogICAgImN1bXVsYXRpdmVHYXNVc2VkIjogIjEwMjgyOCIsCiAgICAiZnJvbSI6ICIweGYwYWZjMTlkOTEwOGI0NDMwOTZmMGRlNGQ0NjEwOTNiMTA2YjNiY2QiLAogICAgImdhc1VzZWQiOiAiMTAyODI4IiwKICAgICJub25jZSI6ICIwIiwKICAgICJ0byI6ICIweDM1YWFjNTBmNjNlNjE0ZTE1OGYyZjMzYWFlMjNjNmM1MGVjNzU3ODUiLAogICAgInRyYW5zYWN0aW9uSGFzaCI6ICIweDgxZDZiZWQyZjdkNmU4MWM1MjZkMzRjZjY1YWViY2EwNGU2OTg2YzYzMjYyMTJiMWJlMzBkNGVlNDkzYmJlNGUiLAogICAgInRyYW5zYWN0aW9uSW5kZXgiOiAiMCIsCgogICAgInB1cmNoYXNlT3JkZXIiOiAiRUNGLTQyMjItTkMxLU5DMSIsCiAgICAib3JkZXJEYXRlIjogIjE1IGRlIG5vdmllbWJyZSBkZSAyMDIyLCAxMTo1Njo0MiBVVEMtNSIsCiAgICAic3RhdHVzIjogIkFQUFJPVkVEIiwKICAgICJidXllciI6ICJETUFSSU4iLAkKICAgICJzZWxsZXIiOiAiRUYxMiIsCQogICAgImluY290ZXJtIjoJIkNQVCIKfQ==",
    "status": "2"
}

```

Código JSON para documentación:

```javascript
curl --location 'https://u0cqnbwz76-u0r2m5xw62-connect.us0-aws.kaleido.io/gateways/u0t3eu9oq8/0xc050386500b1b2c8706627bebc622c4bcd6cebff/ParameterValidator?kld-from=0x527e08ea398d607e898eb39176894f1815bfeb0b&kld-sync=true' \
--header 'Authorization: Basic dTB3anJrZzRjZTpfUHRZdDBkWWFSenZ6MDBnNU01d1p1WU13M2xMUFdhOHJMZzQwMEU1SXJ3' \
--header 'Content-Type: application/json' \
--data '{
  "input": "3333",
  "input1": "2"
}'
```

Al decodificar la metadata que muestra registraba en el estado 2, se obtiene el archivo JSON correspondiente al estado To be confirmed(2):

[![58](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/58.png "58")](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/58.png "58")

Parámetros del contrato en formato JSON:

```javascript
{
    "purchaseOrder": "ECF-4222-NC1-NC1",
    "orderDate": "15 de noviembre de 2022, 11:56:42 UTC-5",
    "status": "TO_BE_CONFIRMED_P2",
    "buyer": "DMARIN",	
    "seller": "EF12",	
    "incoterm":	"CPT",
    "incotermName":	"PANAMA",
    "documentType":	"IMP",
    "documentStatus": "Pedido Enviado",
    "totalCashExcludingTax": "33104",	

    "id": "657b1193-655b-4562-5236-8c0fb0302744",
    "type": "TransactionSuccess",
    "timeReceived": "2023-02-25T05:24:27.082544577Z",
    "timeElapsed": 6.370812683,
    "requestOffset": "",
    "requestId": "69615e10-4f7e-4fb7-60eb-fa702972d9ec",
    "blockHash": "0xc77f7340764b721d125d608939b00f68dde391beb67016ed0e798dcd49d79ed6",
    "blockNumber": "79220",
    "cumulativeGasUsed": "102828",
    "from": "0xf0afc19d9108b443096f0de4d461093b106b3bcd",
    "gasUsed": "102828",
    "nonce": "0",
    "to": "0x35aac50f63e614e158f2f33aae23c6c50ec75785",
    "transactionHash": "0x81d6bed2f7d6e81c526d34cf65aebca04e6986c6326212b1be30d4ee493bbe4e",
    "transactionIndex": "0",

    "purchaseOrder": "ECF-4222-NC1-NC1",
    "orderDate": "15 de noviembre de 2022, 11:56:42 UTC-5",
    "status": "APPROVED",
    "buyer": "DMARIN",	
    "seller": "EF12",	
    "incoterm":	"CPT"
}

```

- Consultar parámetros guardados en el contrato de acuerdo al status Booking Request(6) de la PO "3333" tipo 3:

[![59](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/59.png "59")](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/59.png "59")

Respuesta de POSTMAN:

```javascript
{
    "metadata": "ewogICAgInB1cmNoYXNlT3JkZXIiOiAiRUNGLTQyMjItTkMxLU5DMSIsCiAgICAib3JkZXJEYXRlIjogIjE1IGRlIG5vdmllbWJyZSBkZSAyMDIyLCAxMTo1Njo0MiBVVEMtNSIsCiAgICAic3RhdHVzIjogIkJPT0tJTkdfUkVRVUVTVCIsCiAgICAiYnV5ZXIiOiAiRE1BUklOIiwJCiAgICAic2VsbGVyIjogIkVGMTIiLAkKICAgICJpbmNvdGVybSI6CSJDUFQiLAogICAgImluY290ZXJtTmFtZSI6CSJQQU5BTUEiLAogICAgImRvY3VtZW50VHlwZSI6CSJJTVAiLAogICAgImRvY3VtZW50U3RhdHVzIjogIlBlZGlkbyBFbnZpYWRvIiwKICAgICJ0b3RhbENhc2hFeGNsdWRpbmdUYXgiOiAiMzMxMDQiLAkKCiAgICAiaWQiOiAiNjU3YjExOTMtNjU1Yi00NTYyLTUyMzYtOGMwZmIwMzAyNzQ0IiwKICAgICJ0eXBlIjogIlRyYW5zYWN0aW9uU3VjY2VzcyIsCiAgICAidGltZVJlY2VpdmVkIjogIjIwMjMtMDItMjVUMDU6MjQ6MjcuMDgyNTQ0NTc3WiIsCiAgICAidGltZUVsYXBzZWQiOiA2LjM3MDgxMjY4MywKICAgICJyZXF1ZXN0T2Zmc2V0IjogIiIsCiAgICAicmVxdWVzdElkIjogIjY5NjE1ZTEwLTRmN2UtNGZiNy02MGViLWZhNzAyOTcyZDllYyIsCiAgICAiYmxvY2tIYXNoIjogIjB4Yzc3ZjczNDA3NjRiNzIxZDEyNWQ2MDg5MzliMDBmNjhkZGUzOTFiZWI2NzAxNmVkMGU3OThkY2Q0OWQ3OWVkNiIsCiAgICAiYmxvY2tOdW1iZXIiOiAiNzkyMjAiLAogICAgImN1bXVsYXRpdmVHYXNVc2VkIjogIjEwMjgyOCIsCiAgICAiZnJvbSI6ICIweGYwYWZjMTlkOTEwOGI0NDMwOTZmMGRlNGQ0NjEwOTNiMTA2YjNiY2QiLAogICAgImdhc1VzZWQiOiAiMTAyODI4IiwKICAgICJub25jZSI6ICIwIiwKICAgICJ0byI6ICIweDM1YWFjNTBmNjNlNjE0ZTE1OGYyZjMzYWFlMjNjNmM1MGVjNzU3ODUiLAogICAgInRyYW5zYWN0aW9uSGFzaCI6ICIweDgxZDZiZWQyZjdkNmU4MWM1MjZkMzRjZjY1YWViY2EwNGU2OTg2YzYzMjYyMTJiMWJlMzBkNGVlNDkzYmJlNGUiLAogICAgInRyYW5zYWN0aW9uSW5kZXgiOiAiMCIsCgogICAgInB1cmNoYXNlT3JkZXIiOiAiRUNGLTQyMjItTkMxLU5DMSIsCiAgICAib3JkZXJEYXRlIjogIjE1IGRlIG5vdmllbWJyZSBkZSAyMDIyLCAxMTo1Njo0MiBVVEMtNSIsCiAgICAic3RhdHVzIjogIkFQUFJPVkVEIiwKICAgICJidXllciI6ICJETUFSSU4iLAkKICAgICJzZWxsZXIiOiAiRUYxMiIsCQogICAgImluY290ZXJtIjoJIkNQVCIKfQ==",
    "status": "6"
}

```

Código JSON para documentación:

```javascript
curl --location 'https://u0cqnbwz76-u0r2m5xw62-connect.us0-aws.kaleido.io/gateways/u0t3eu9oq8/0xc050386500b1b2c8706627bebc622c4bcd6cebff/ParameterValidator?kld-from=0x527e08ea398d607e898eb39176894f1815bfeb0b&kld-sync=true' \
--header 'Authorization: Basic dTB3anJrZzRjZTpfUHRZdDBkWWFSenZ6MDBnNU01d1p1WU13M2xMUFdhOHJMZzQwMEU1SXJ3' \
--header 'Content-Type: application/json' \
--data '{
  "input": "3333",
  "input1": "6"
}'
```

Al decodificar la metadata que muestra registraba en el estado 6, se obtiene el archivo JSON correspondiente al estado Booking Request(6):

[![60](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/60.png "60")](https://github.com/rozoandrescamilo/BLCH-v1/blob/main/img/60.png?raw=true "60")

Parámetros del contrato en formato JSON:

```javascript
{
    "purchaseOrder": "ECF-4222-NC1-NC1",
    "orderDate": "15 de noviembre de 2022, 11:56:42 UTC-5",
    "status": "BOOKING_REQUEST",
    "buyer": "DMARIN",	
    "seller": "EF12",	
    "incoterm":	"CPT",
    "incotermName":	"PANAMA",
    "documentType":	"IMP",
    "documentStatus": "Pedido Enviado",
    "totalCashExcludingTax": "33104",	

    "id": "657b1193-655b-4562-5236-8c0fb0302744",
    "type": "TransactionSuccess",
    "timeReceived": "2023-02-25T05:24:27.082544577Z",
    "timeElapsed": 6.370812683,
    "requestOffset": "",
    "requestId": "69615e10-4f7e-4fb7-60eb-fa702972d9ec",
    "blockHash": "0xc77f7340764b721d125d608939b00f68dde391beb67016ed0e798dcd49d79ed6",
    "blockNumber": "79220",
    "cumulativeGasUsed": "102828",
    "from": "0xf0afc19d9108b443096f0de4d461093b106b3bcd",
    "gasUsed": "102828",
    "nonce": "0",
    "to": "0x35aac50f63e614e158f2f33aae23c6c50ec75785",
    "transactionHash": "0x81d6bed2f7d6e81c526d34cf65aebca04e6986c6326212b1be30d4ee493bbe4e",
    "transactionIndex": "0",

    "purchaseOrder": "ECF-4222-NC1-NC1",
    "orderDate": "15 de noviembre de 2022, 11:56:42 UTC-5",
    "status": "APPROVED",
    "buyer": "DMARIN",	
    "seller": "EF12",	
    "incoterm":	"CPT"
}

```
