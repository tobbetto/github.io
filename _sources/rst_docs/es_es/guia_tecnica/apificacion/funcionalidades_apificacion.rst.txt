|image0|

=========================
FUNCIONALIDAD APIFICACIÓN
=========================

`1. INTRODUCCIÓN 5 <#introduccion>`__

`2. FLUJO DE CREACIÓN DE PEDIDO BÁSICO <#flujo-de-creacion-de-pedido-basico>`__

`3. FLUJO DE CREACIÓN DE UN PEDIDO DE KIOSCO <#flujo-de-creacion-de-un-pedido-de-kiosco>`__

`4. INICIO DE PEDIDO 6 <#inicio-de-pedido>`__

`5. TIENDAS POR LOCALIZACIÓN 8 <#tiendas-por-localizacion>`__

`6. HORAS DISPONIBLES 9 <#horas-disponibles>`__

`7. OBTENER CATÁLOGO 10 <#obtener-catalogo>`__

`7.1 Obtener los ingredientes elegibles para un producto <#obtener-los-ingredientes-elegibles-para-un-producto>`__

`8. CREACIÓN DEL PEDIDO 21 <#creacion-del-pedido>`__

`9. AÑADIR UN PRODUCTO 22 <#añadir-un-producto>`__

`10. ESTABLECER UNA DIRECCIÓN DE ENTREGA <#establecer-una-direccion-de-entrega>`__

`11. OBTENER MEDIOS DE PAGO 29 <#obtener-medios-de-pago>`__

`12. FINALIZAR PEDIDO 31 <#finalizar-pedido>`__

   |image1|

   La información que contiene la presente propuesta es propiedad
   exclusiva de Vector ITC Group No está permitida la reproducción de la
   misma, salvo con autorización expresa.

   No está permitida la distribución de la misma a personas, entidades o
   empresas no mencionadas explícitamente en el panel de información de
   control y versiones, salvo con autorización expresa de Vector, SL.

   |image2|

1. INTRODUCCIÓN 
================

Este documento representa la propuesta funcional para dar respuesta a la
necesidad de Telepizza de crear un producto integrable por consumidores
externos.

Desde Telepizza se quiere dar la posibilidad a un usuario, que realice
un pedido a Telepizza a través de un consumidor externo.

2. FLUJO DE CREACIÓN DE PEDIDO BÁSICO 
======================================

Para poder crear un pedido sencillo mediante la Apificación, será
necesario realizar las llamadas a los servicios en el siguiente orden
concreto:

-  Token e inicio de pedido

POST /connect/token

-  Obtener tiendas por localización (a recoger)

GET /shop/location/{lat}/{lng}

-  Obtener horas disponibles

..

   GET /shop/availablehours

-  Obtener catálogo

..

   GET /catalogue/shop/{id}

-  Creación del pedido

..

   POST /order/create

-  Añadir un producto

..

   POST /order/product

-  Establecer los datos de reparto

..

   GET /order/getaddress

   POST /order/delivery

-  Obtener los medios de pago para la tienda GET /shop/{id}/payments

-  Finalizar el pedido

..

   POST /order/save

3. FLUJO DE CREACIÓN DE UN PEDIDO DE KIOSCO 
============================================

Para poder crear un pedido de kiosko mediante la Apificación, será
necesario realizar las llamadas a los servicios en el siguiente orden
concreto:

-  Token e inicio de pedido

..

   POST /connect/token

-  Obtener catálogo

..

   GET /catalogue/shop/{ShopId}/{DeliveryType}

-  Creación del pedido

..

   POST /order/createlocal

-  Añadir un producto

..

   POST /order/product

-  Finalizar el pedido

..

   POST /order/savelocal

4. INICIO DE PEDIDO 
====================

   POST /connect/token

Esta llamada se encarga de obtener un token de seguridad para autorizar
las invocaciones a las llamadas y será obligatorio usarlo en todas las
llamadas.

La invocación a esta llamada será la primera que se realice antes de
poder realizar cualquier otra dentro del servicio.

Los parámetros de entrada son:

============== ====================================================================================================== =============================
Parámetro         Descripción                                                                                            Ejemplo
============== ====================================================================================================== =============================
**x-consumer**    Token del consumidor codificado en Base64                                                              Base64(“prueba”) =
                                                                                                                     
                                                                                                                         “cHJ1ZWJhOkFwaWZpY2F0aW9u”
**grant_type**    Tipo de permiso                                                                                        client_credentials
**scope**         Para pedir acceso a los recursos con un determinado rol de lectura, escritura, sólo de acceso, etc.    “ALL”
**Device**        Tipo de dispositivo que interactúa con el servicio. (tablet, ipad, smartphone o mobile)                “tablet”
**Culture**       Cultura. (es_es, es_en, es_ca, …)                                                                      “es_es”
**Language**      Idioma en el que se mostrará la información                                                            “es”
============== ====================================================================================================== =============================

La respuesta contendrá el token que se usará en las llamadas posteriores
y se corresponderá con el campo **x-auth-back** en los parámetros de
entrada del resto de llamadas. Este token será válido durante un periodo
de tiempo y dentro del ámbito del pedido en curso.

   Ejemplo de respuesta:

.. code-block:: json

   {

   "access_token":

   "eyJhbGciOiJodHRwOi8vd3d3LnczLm9yZy8yMDAxLzA0L3htbGRzaWctbW9yZSNyc2Etc2hhMjU2IiwidHlwIjoiSldUIn
   0.eyJuYmYiOjE1NTYxMDE0ODQsImV4cCI6MTU1NjEwNTA4NCwiaXNzIjoiaHR0cDovL2FwaS1zZXJ2aWNlcy5kZXYuYXdzLnRlbGVwaXp6YS5jb20iLCJhdWQiOlsiaHR0cDovL2FwaS1zZXJ2aWNlcy5kZXYuYXdzLnRlbGVwaXp6YS5jb20vcmVzb3VyY2VzIiwiQUxMIl0sImNsaWVudF9pZCI6InRweiIsImp0aSI6IjUiLCJzY29wZSI6WyJBTEwiXX0.Q9bsxA6syMb1h3eTzRd
   oG-kJlnFLP3V3P7o0r5Xlvp3FBQY7mSX832sD-TSW288aTWYERHL50drl-QFD1VNVOFt0NG5drQuNFK4j8hnIUfu8NyMTF6fVPc_voi6SlEpZ5hfwdky1TWBbQYSL0rZgc1-Gz3sDuyU7XPo7x1_ISj8DXikYDSp7v6LcFTNR-Iz8NKCsLMvLjHs8WpkOFgFw9SlFOTYPJC7ns6O03ZZovaG2rEFLGAkZ2FAtEkMpekqiKd9TVCiKODdGFc2YRC9hdjKvb0q2
   s0Qrd4sYRMp7pJVNw51ZIGR0WQ6Osz92sUo1EG69DyJGBNerUSQuhunJVw",

   "expires_in": 3600,

   "token_type": "Bearer"

   }

5. TIENDAS POR LOCALIZACIÓN 
============================

   GET /shop/location/{lat}/{lng}

Obtiene los códigos de las tiendas y el coste de reparto por
localización más cercana a una latitud y longitud concreta. Esta llamada
es sólo para pedidos a recoger en tienda.

   Los parámetros de entrada son:

================== ============================================================================= ==========================
   Parámetro          Descripción                                                                Ejemplo
================== ============================================================================= ==========================
   **x-auth-back**    Autorización para el servicio con el token obtenido en la llamada al “POST “Bearer
                                                                                                
                      /connect/token”:                                                           eyJhbGciOiJodHRwOi8vd3d3…”
                                                                                                
                      token_type + “ ” + access_token                                           
   **lat**            Latitud de la posición que se quiere consultar                             “41.67246”
   **lng**            Longitud de la posición que se quiere consultar                            “-0.890901”
================== ============================================================================= ==========================

Esta llamada se puede ejecutar de forma independiente al resto de
existentes.

La información que devuelve consta del identificador de la tienda que se
utilizará en consultas posteriores, junto con la dirección de la tienda
(calle, ciudad y provincia).

   Ejemplo de respuesta:

.. code-block:: json

   [

   {

   "shopId": "00145",

   "addresss": "Gertrudis Gómez de Avellaneda",

   "city": "ZARAGOZA",

   "province": "ZARAGOZA"

   },

   {

   "shopId": "00306",

   "addresss": "Sobrarbe 43",

   "city": "ZARAGOZA",

   "province": "ZARAGOZA"

   },

   {

   "shopId": "00859",

   "addresss": "Plaza del Pilar 14",

   "city": "ZARAGOZA",

   "province": "ZARAGOZA"

   },

   {

   "shopId": "00374",

   "addresss": "Paseo María Agustín 9",

   "city": "ZARAGOZA",

   "province": "ZARAGOZA"

   },

   {

   "shopId": "00834",

   "addresss": "Avenida de Madrid 198",

   "city": "ZARAGOZA",

   "province": "ZARAGOZA"

   }

   ]

6. HORAS DISPONIBLES 
=====================

   POST /shop/availablehours

En el caso de que se informen los campos de **lat** y **lng**, se
encarga de consultar y devolver las horas disponibles de reparto a
domicilio. Si se informa el campo **shopId**, devolverá las horas en las
que se podrá recoger el pedido en la tienda indicada.

   Los parámetros de entrada son:

=================== ============================================================================= ==========================
   Parámetro           Descripción                                                                Ejemplo
=================== ============================================================================= ==========================
   **x-auth-back**     Autorización para el servicio con el token obtenido en la llamada al “POST “Bearer
                                                                                                 
                       /connect/token”:                                                           eyJhbGciOiJodHRwOi8vd3d3…”
                                                                                                 
                       token_type + “ ” + access_token                                           
   **lat**             Latitud de la posición que se quiere consultar                             “41.67246”
   **lng**             Longitud de la posición que se quiere consultar                            “-0.890901”
   **shopId**          Identificador de la tienda                                                 “00145”
   **deliveryType**    Tipo de reparto                                                            1. – Local
                                                                                                 
                                                                                                  2. – Domicilio
                                                                                                 
                                                                                                  3. – Recoger en Tienda
=================== ============================================================================= ==========================

Esta llamada se puede ejecutar de forma independiente al resto de
existentes.

La respuesta devolverá el listado de horas que la tienda tiene
disponibles para recoger pedidos o para envío a domicilio, junto con el
tiempo de espera entre hora y hora, configurado para la tienda asignada
a esa localización, o para la tienda indicada.

   Ejemplo de respuesta:

.. code-block:: json

   {

   "availableHours": [

   "2019-05-27T19:05:00Z",

   "2019-05-27T19:20:00Z",

   "2019-05-27T19:35:00Z",

   "2019-05-27T19:50:00Z",

   "2019-05-27T20:05:00Z",

   "2019-05-27T20:20:00Z",

   "2019-05-27T20:35:00Z",

   "2019-05-27T20:50:00Z",

   "2019-05-27T21:05:00Z",

   "2019-05-27T21:20:00Z",

   "2019-05-27T21:35:00Z",

   "2019-05-27T21:50:00Z",

   "2019-05-27T22:05:00Z",

   "2019-05-27T22:20:00Z",

   "2019-05-27T22:35:00Z",

   "2019-05-27T22:50:00Z",

   "2019-05-27T23:05:00Z",

   "2019-05-27T23:20:00Z",

   "2019-05-27T23:35:00Z"

   ],

   "waitTime": 0

   }

7. OBTENER CATÁLOGO 
====================

   GET /catalogue/shop/{id}

Esta llamada se encarga de devolver todos los productos disponibles para
una tienda.

   Los parámetros de entrada son:

================== ============================================================================= ==========================
   Parámetro          Descripción                                                                Ejemplo
================== ============================================================================= ==========================
   **x-auth-back**    Autorización para el servicio con el token obtenido en la llamada al “POST “Bearer
                                                                                                
                      /connect/token”: token_type + “ ” + access_token                           eyJhbGciOiJodHRwOi8vd3d3…”
   **Id**             Código identificador de la tienda.                                         “00145”
================== ============================================================================= ==========================

Dentro de la respuesta se encuentra el listado de productos agrupados
por categorías y subcategorías. Y a su vez, dentro de cada producto
podrán haber definidos diferentes tamaños del producto elegibles del
listado, ingredientes por defecto de cada producto (podrán ser añadidos)
y diferentes tamaños de masa también elegibles, entre otros campos.

======================== ================================================================================================================================================== ==================================================================================================
\                        **PRODUCTO**                                                                                                                                      
======================== ================================================================================================================================================== ==================================================================================================
   Parámetro                Descripción                                                                                                                                     Ejemplo
   **productId**            Número Identificador del producto                                                                                                               “999990000006710”
   **Name**                 Nombre del producto                                                                                                                             Pizza Barbacoa
   **description**          Descripción del contenido del producto                                                                                                          Masa fresca, bacon, pollo, topping a base de mozzarella, salsa barbacoa y doble de carne de vacuno
   **Image**                Ruta de la imagen asociada al producto                                                                                                          http://triton.tel epizza.es/nvol/es /content/producto s/pbbq_d.png
**portionsAllowed**         Campo que indica si el producto permite división en porciones o mitades.                                                                        “true”
**defaultSizeId**           Número identificador del tamaño por defecto. Este código pertenecerá a uno de los tamaños existentes dentro de listado del campo “sizes[]”      “20” → Mediana
**maxNumIngredients**       Número máximo de ingredientes adicionales que está permitido añadir a este producto                                                             1
**sizes[]**                 Listado de tamaños elegibles del producto. (“Individual”, “Mediana”, “Familiar”, “Strómboli”)                                                  
**defaultIngredients[]**    Listado de ingredientes por defecto que componen el producto. Estos ingredientes podrán ser modificados.                                       
**productBaseSizes[]**      Listado de tipos de bases o formatos de base del producto. En el caso de pizzas, son los tipos de masas que se pueden escoger para el producto.
                                                                                                                                                                           
                            (“Clásica”, “3 Pisos”, “Fina”,                                                                                                                 
                                                                                                                                                                           
                            “Integral” o “QuadRoller”)                                                                                                                     
======================== ================================================================================================================================================== ==================================================================================================

..

   Ejemplo de respuesta:

.. code-block:: json

   {

   "categories": [

   {

   "categoryId": "999990004923100",

   "name": "Pizzas",

   "description": "",

   "subcategories": [

   {

   "subcategoryId": "999990004922538",

   "name": "Las Clásicas",

   "products": [

   {

   "productId": "999990000006710",

   "name": "Pizza Barbacoa",

   "description": "Masa fresca, bacon, pollo, topping a base de
   mozzarella, salsa barbacoa y doble de carne de vacuno.",

   "image":
   "http://triton.telepizza.es/nvol/es/content/productos/pbbq_d.png",

   "portionsAllowed": true,

   "defaultSizeId": "20",

   "maxNumIngredients": 1,

   "sizes": [

   {

   "sizeId": "16",

   "name": "Pequeña",

   "price": 14.95

   },

   {

   "sizeId": "20",

   "name": "Mediana",

   "price": 20.95

   },

   {

   "sizeId": "21",

   "name": "Familiar",

   "price": 27.95

   },

   {

   "sizeId": "36",

   "name": "Strómboli",

   "price": 20.95

   }

   ],

   "defaultIngredients": [

   {

   "ingredientId": "999990005361675",

   "name": "SALSA BARBACOA",

   "image":

   "http://triton.telepizza.es/app/5.0/es/images/ingredients/{density}/sbpr.jpg",

   "quantity": 1,

   "groupId": "1",

   "groupDescription": "Group 1"

   },

   {

   "ingredientId": "999990000005700", "name": "BASE CLÁSICA",

   "image":

   "http://triton.telepizza.es/app/5.0/es/images/ingredients/{density}/base.jpg",

   "quantity": 1,

   "groupId": "2",

   "groupDescription": "Group 2"

   },

   {

   "ingredientId": "999990005369717", "name": "Con Topping",

   "image":

   "http://triton.telepizza.es/app/5.0/es/images/ingredients/{density}/moze.jpg",

   "quantity": 1,

   "groupId": "3",

   "groupDescription": "Group 3"

   },

   {

   "ingredientId": "999990000004466", "name": "Carne de vacuno",

   "image":

   "http://triton.telepizza.es/app/5.0/es/images/ingredients/{density}/ca.jpg",

   "quantity": 1,

   "groupId": "3",

   "groupDescription": "Group 3"

   },

   {

   "ingredientId": "999990005436200", "name": "Bacon",

   "image":

   "http://triton.telepizza.es/app/5.0/es/images/ingredients/{density}/ca.jpg",

   "quantity": 1,

   "groupId": "3",

   "groupDescription": "Group 3"

   },

   {

   "ingredientId": "999990000004543", "name": "Pollo marinado",

   "image":

   "http://triton.telepizza.es/app/5.0/es/images/ingredients/{density}/ca.jpg",

   "quantity": 1,

   "groupId": "3",

   "groupDescription": "Group 3"

   }

   ],

   "productBaseSizes": [

   {

   "productId": "999990000006710",

   "allowedSizes": [

   "36",

   "20"

   ]

   }

   ]

   },

   {

   "productId": "999990000013106",

   "name": "Pizza Carbonara",

   "description": null,

   "image": null,

   "portionsAllowed": false,

   "defaultSizeId": null,

   "maxNumIngredients": 0,

   "sizes": null,

   "defaultIngredients": null,

   "productBaseSizes": null

   }

   ]

   },

   {

   "subcategoryId": "999990004922500",

   "name": "Las Destacadas",

   "products": [

   {

   "productId": "999990000006814",

   "name": "A tu gusto",

   "description": null,

   "image": null,

   "portionsAllowed": false,

   "defaultSizeId": null,

   "maxNumIngredients": 0,

   "sizes": null,

   "defaultIngredients": null,

   "productBaseSizes": null

   },

   {

   "productId": "999990010533500",

   "name": "Telepizza Sweet",

   "description": null,

   "image": null,

   "portionsAllowed": false,

   "defaultSizeId": null,

   "maxNumIngredients": 0,

   "sizes": null,

   "defaultIngredients": null,

   "productBaseSizes": null

   }

   ]

   }

   ]

   },

   {

   "categoryId": "999990004923100",

   "name": "Bebidas",

   "description": "",

   "subcategories": [

   {

   "subcategoryId": "999990004922538",

   "name": "Refrescos 500 ml",

   "products": [

   {

   "productId": "999990001261600",

   "name": "Botella Coca-Cola (500ml)",

   "description": null,

   "image": null,

   "portionsAllowed": false,

   "defaultSizeId": null,

   "maxNumIngredients": 0,

   "sizes": [

   {

   "sizeId": "35",

   "name": "50cl",

   "price": 1.95

   }

   ],

   "defaultIngredients": null,

   "productBaseSizes": null

   }

   ]

   }

   ]

   },

   {

   "categoryId": "999990004923110",

   "name": "Hamburguesas",

   "description": "El bocado perfecto",

   "subcategories": [

   {

   "subcategoryId": "999990004923634",

   "name": "Hamburguesas",

   "products": [

   {

   "productId": "999990006381900",

   "name": "Nueva Top Burguer Vacuno",

   "description": null,

   "image": null,

   "portionsAllowed": false,

   "defaultSizeId": null,

   "maxNumIngredients": 0,

   "sizes": [

   {

   "sizeId": "4883062663",

   "name": "Individual",

   "price": 4.95000029

   }

   ],

   "defaultIngredients": null,

   "productBaseSizes": null

   }

   ]

   }

   ]

   }

   ]

   }

   GET /catalogue/shop/{ShopId}/{DeliveryType}

Esta llamada se encarga de comenzar un pedido de un kiosco, obteniendo
en su respuesta el catálogo correspondiente,

   Los parámetros de entrada son:

=================== ============================================================================= ==========================
   Parámetro           Descripción                                                                Ejemplo
=================== ============================================================================= ==========================
   **x-auth-back**     Autorización para el servicio con el token obtenido en la llamada al “POST “Bearer
                                                                                                 
                       /connect/token”:                                                           eyJhbGciOiJodHRwOi8vd3d3…”
                                                                                                 
                       token_type + “ ” + access_token                                           
   **ShopId**          Código identificador de la tienda.                                         “00145”
   **DeliveryType**    Código de tipo de reparto. Puede ser 1 ó 3. [1 = local, 3 = recoger]       1
=================== ============================================================================= ==========================

La respuesta sigue la misma estructura que el punto anterior.

Además del catálogo para una tienda concreta, existen varias llamadas
que se engloban dentro del servicio del catálogo que se encargan de
obtener un producto con todos sus ingredientes a partir de su código, o
también se pueden obtener los ingredientes completos existentes en una
tienda.

-  Obtiene todos los ingredientes elegibles para una tienda GET
   /catalogue/choices/shop/{id}

-  Obtiene los ingredientes elegibles para un producto en una tienda

GET /catalogue/shop/{shopId}/product/{productId}/choices

7.1 Obtener los ingredientes elegibles para un producto 
--------------------------------------------------------

GET /catalogue/shop/{shopId}/product/{productId}/choices

Permite obtener todos los ingredientes que se pueden añadir o elegir
para componer un producto compuesto a partir del identificador del
producto.

   Los parámetros de entrada son:

================== ============================================================================= ==========================
   Parámetro          Descripción                                                                Ejemplo
================== ============================================================================= ==========================
   **x-auth-back**    Autorización para el servicio con el token obtenido en la llamada al “POST “Bearer
                                                                                                
                      /connect/token”:                                                           eyJhbGciOiJodHRwOi8vd3d3…”
                                                                                                
                      token_type + “ ” + access_token                                           
   **shopId**         Identificador de la tienda                                                 “00145”
   **productId**      Identificador del producto                                                 “999990000006710”
================== ============================================================================= ==========================

El listado devolverá tantas repeticiones del mismo código de producto
como agrupaciones de ingredientes en las que esté incluido el producto
seleccionado. Dentro de cada agrupación estarán incluidos el listado de
ingredientes seleccionables. Estas agrupaciones son una clasificación
por tipo de ingrediente que permite saber la cantidad mínima y máxima de
ingredientes que pueden ser añadidos o no, al producto.

Esta agrupación se podrá utilizar para mostrar en pantalla la
información de estos ingredientes en listados seleccionables u otros
contenedores:

|image3|

Por ejemplo, si el campo mínimo de ingredientes viene informado con un 0
y el de máximo de ingredientes con valor 1, se correspondería con un
ingrediente opcional que puede o no ir incluido. En la imagen anterior
el ejemplo se corresponde con el de: “¿La quieres gratinar?”.

Sin embargo, si el campo de mínimo viene con valor 1 y el de máximo
viene con valor 1, quiere decir que será un elemento obligatorio y a su
vez, llevará un listado de ingredientes para poder elegir uno. En la
imagen anterior, se correspondería con el campo de: “Topping a base de
Mozzarella”.

Otro caso diferente, sería si el campo mínimo viniese con valor 0 y el
máximo con valor 8, implica que son ingredientes opcionales, y como
máximo se podrán añadir 8 ingredientes en total, 8 del mismo tipo u 8 en
total de todos ellos. Por ejemplo, el listado de ingredientes siguiente:

|image4|

|image5|

   Ejemplo de respuesta:

.. code-block:: json

   [

   {

   "productId": "999990000006710",

   "groupId": "5147621549",

   "sizeId": "16",

   "name": "SALSAS",

   "description": "SALSAS",

   "groupMinQuantity": 1,

   "groupMaxQuantity": 1,

   "minPerIngredient": 1,

   "maxPerIngredient": 1,

   "ingredients": [

   {

   "ingredientId": "999990005362717",

   "description": "SALSA BBQ CREME DOBLE",

   "image":
   "http://triton.telepizza.es/app/5.0/es/images/ingredients/{density}/2sbc.jpg"
   },

   {

   "ingredientId": "999990005363000",

   "description": "SALSA BARBACOA CRÉME",

   "image":
   "http://triton.telepizza.es/app/5.0/es/images/ingredients/{density}/sbcr.jpg"
   },

   {

   "ingredientId": "999990005361909",

   "description": "SALSA BARBACOA DOBLE",

   "image":
   "http://triton.telepizza.es/app/5.0/es/images/ingredients/{density}/2sba.jpg"
   },

   {

   "ingredientId": "999990005361675",

   "description": "SALSA BARBACOA",

   "image":
   "http://triton.telepizza.es/app/5.0/es/images/ingredients/{density}/sbpr.jpg"
   },

   {

   "ingredientId": "999990005362799",

   "description": "SALSA BURGER DOBLE",

   "image":
   "http://triton.telepizza.es/app/5.0/es/images/ingredients/{density}/2sbg.jpg"

   },

   {

   "ingredientId": "999990005363136",

   "description": "SALSA BURGER",

   "image":
   "http://triton.telepizza.es/app/5.0/es/images/ingredients/{density}/sbrg.jpg"
   },

   {

   "ingredientId": "999990005363775",

   "description": "SALSA CARBONARA DOBLE",

   "image":
   "http://triton.telepizza.es/app/5.0/es/images/ingredients/{density}/2sca.jpg"
   },

   {

   "ingredientId": "999990005363361",

   "description": "SALSA CARBONARA",

   "image":
   "http://triton.telepizza.es/app/5.0/es/images/ingredients/{density}/scae.jpg"
   },

   {

   "ingredientId": "999990005363943",

   "description": "SALSA JALISCO DOBLE",

   "image":
   "http://triton.telepizza.es/app/5.0/es/images/ingredients/{density}/2sja.jpg"
   },

   {

   "ingredientId": "999990005364413",

   "description": "SALSA JALISCO",

   "image":
   "http://triton.telepizza.es/app/5.0/es/images/ingredients/{density}/saje.jpg"
   },

   {

   "ingredientId": "999990005367124",

   "description": "SALSA TOMATE Y ORÉGANO DOBLE",

   "image":
   "http://triton.telepizza.es/app/5.0/es/images/ingredients/{density}/2sto.jpg"
   },

   {

   "ingredientId": "999990005365052",

   "description": "SALSA TOMATE Y ORÉGANO",

   "image":
   "http://triton.telepizza.es/app/5.0/es/images/ingredients/{density}/tome.jpg"
   },

   {

   "ingredientId": "999990002148797",

   "description": "SIN SALSA",

   "image":
   "http://triton.telepizza.es/app/5.0/es/images/ingredients/{density}/0sal.jpg"
   }

   ]

   },

   {

   "productId": "999990000006710",

   "groupId": "4940394233", "sizeId": "16",

   "name": "¿ALGÚN EXTRA?",

   "description": "¿ALGÚN EXTRA?",

   "groupMinQuantity": 1,

   "groupMaxQuantity": 1,

   "minPerIngredient": 1,

   "maxPerIngredient": 1,

   "ingredients": [

   {

   "ingredientId": "999990005630501",

   "description": "--",

   "image":
   "http://triton.telepizza.es/app/5.0/es/images/ingredients/{density}/0is.jpg"
   },

   {

   "ingredientId": "999990005360500",

   "description": "EXTRA BARBACOA",

   "image":
   "http://triton.telepizza.es/app/5.0/es/images/ingredients/{density}/exso.jpg"
   },

   {

   "ingredientId": "999990005363540",

   "description": "SALSA CÉSAR (Después de Horno)",

   "image":
   "http://triton.telepizza.es/app/5.0/es/images/ingredients/{density}/scep.jpg"
   },

   {

   "ingredientId": "999990005364597",

   "description": "SALSA STEAK & GRILL",

   "image":
   "http://triton.telepizza.es/app/5.0/es/images/ingredients/{density}/stg.jpg"
   },

   {

   "ingredientId": "999990005360849",

   "description": "EXTRA TOMATE CONFITADO",

   "image":
   "http://triton.telepizza.es/app/5.0/es/images/ingredients/{density}/extc.jpg"
   }

   ]

   },

   {

   "productId": "999990000006710",

   "groupId": "5228699519",

   "sizeId": "16",

   "name": "¿LA QUIERES GRATINAR?",

   "description": "¿LA QUIERES GRATINAR?",

   "groupMinQuantity": 0,

   "groupMaxQuantity": 1,

   "minPerIngredient": 1,

   "maxPerIngredient": 1,

   "ingredients": [

   {

   "ingredientId": "999990002554800",

   "description": "Gratinado (PVP 2 ingr.)",

   "image":
   "http://triton.telepizza.es/app/5.0/es/images/ingredients/{density}/grat.jpg"
   }

   ]

   }

]

8. CREACIÓN DEL PEDIDO 
=======================

   POST /order/create

Esta llamada realiza crea o inicializa el pedido vacío. Este paso es
previo para poder añadir productos, promociones y añadir un medio de
pago, y por lo tanto necesario para poder realizar cualquier pedido. Si
ya había añadidos productos, se inicializa el pedido sin productos ni
promociones.

   El parámetro de entrada es:

================== ============================================================================= ==========================
   Parámetro          Descripción                                                                Ejemplo
================== ============================================================================= ==========================
   **x-auth-back**    Autorización para el servicio con el token obtenido en la llamada al “POST “Bearer
                                                                                                
                      /connect/token”:                                                           eyJhbGciOiJodHRwOi8vd3d3…”
                                                                                                
                      token_type + “ ” + access_token                                           
   **shopId**         Identificador de la tienda                                                 “00145”
   **dateTime**       Fecha seleccionada para recogida o entrega del pedido.                     “2019-05-24T12:09:00.094Z”
================== ============================================================================= ==========================

La respuesta vendrá vacía si ha ido todo bien (con un código de
respuesta “204 – NoContent”).

   POST /order/createlocal

Esta llamada realiza crea o inicializa el pedido vacío. Este paso es
previo para poder añadir productos, promociones y añadir un medio de
pago, y por lo tanto necesario para poder realizar cualquier pedido. Si
ya había añadidos productos, se inicializa el pedido sin productos ni
promociones.

   El parámetro de entrada es:

================== ============================================================================= ==========================
   Parámetro          Descripción                                                                Ejemplo
================== ============================================================================= ==========================
   **x-auth-back**    Autorización para el servicio con el token obtenido en la llamada al “POST “Bearer
                                                                                                
                      /connect/token”:                                                           eyJhbGciOiJodHRwOi8vd3d3…”
                                                                                                
                      token_type + “ ” + access_token                                           
================== ============================================================================= ==========================

La respuesta vendrá vacía si ha ido todo bien (con un código de
respuesta “204 – NoContent”).

9. AÑADIR UN PRODUCTO 
======================

   POST /order/product

Esta llamada permite agregar un producto a un pedido ya existente, que
este pedido esté vacío o que contenga otros productos incluidos en él.

El producto de entrada puede ser un producto simple como una bebida que
no contiene ingredientes elegibles o seleccionables o un producto
compuesto (ejemplo: pizza) que contiene ingredientes por defecto y
también otros ingredientes que se pueden ir agregando según una lista.

   Los parámetros de entrada son:

================== ==================================================================== =============================
   Parámetro       Descripción                                                             Ejemplo
================== ==================================================================== =============================
   **x-auth-back** Autorización para el servicio con el token obtenido en la llamada al    “Bearer
                                                                                       
                   “POST /connect/token”:                                                  eyJhbGciOiJodHRwOi8vd3d3…”
                                                                                       
                   token_type + “ ” + access_token                                     
   **product[]**   Información del producto que se desea añadir al pedido.             
   **size**        Código identificador del tamaño                                         Mediana → “20”
   **units**       Cantidad de unidades del mismo producto                                 1
================== ==================================================================== =============================

================= =================================================================================
\                    **producto[]**                                                                
================= =================================================================================
   **products[]** Listado de productos con sus ingredientes y elecciones que se añadirán al pedido.
================= =================================================================================

======================= ================================================== ========================================================================================================
**products[]**                                                            
======================= ================================================== ========================================================================================================
   **partialProductId** Código identificador del producto                     “999990000006710”
   **name**             Nombre del producto                                   “Pizza Barbacoa”
   **description**      Descripción breve del producto                        “Masa fresca, bacon, pollo, topping a base de mozzarella, salsa barbacoa y doble de carne de vacuno.”
   **choices[]**        Listado de ingredientes elegibles para el producto
======================= ================================================== ========================================================================================================

=============== ================================================================= ===========================
\                  **choices[]**                                                 
=============== ================================================================= ===========================
   **choiceId** Identificador del ingrediente que se incluirá dentro del producto    “999990005365052”
   **name**     Nombre del ingrediente                                               “SALSA TOMATE Y ORÉGANO”
=============== ================================================================= ===========================

..

   Ejemplo de parámetros de entrada:

.. code-block:: json

   {

   "products": [

   {

   "name": "Bacon Crispy Gourmet",

   "description": "",

   "partialproductid": "999990010908732",

   "choices": [

   {

   "choiceid": "999990005263746",

   "name": "BASE FINA"

   },

   {

   "choiceid": "999990010517817",

   "name": "5 Quesos Gourmet"

   },

   {

   "choiceid": "999990006472065",

   "name": "Fina masa"

   },

   {

   "choiceid": "999990010429777",

   "name": "Salsa Barbacoa"

   },

   {

   "choiceid": "999990010902209",

   "name": "Topping a Base de Mozzarella"

   },

   {

   "choiceid": "999990010905260",

   "name": "Bacon"

   },

   {

   "choiceid": "999990010902269",

   "name": "Bacon Crispy"

   }

   ]

   }

   ],

   "size": 20,

   "units": 2

   }

La respuesta de esta llamada devolverá el pedido actual completo con
todos los productos que se han añadido hasta el momento.

   Ejemplo de respuesta:

.. code-block:: json

   {

   "customerEmail": null,

   "deliveryOrder": null,

   "cartDto": {

   "products": [

   {

   "products": [

   {

   "name": "Bacon Crispy Gourmet",

   "description": "Si eres fan del bacon, aquí tienes ración doble:
   ahumado y crispy.

   Una sabrosa mezcla acompañada por nuestra tradicional salsa barbacoa
   y la doble masa rellena de 5 quesos.",

   "partialProductId": "999990010908732",

   "choices": [

   {

   "choiceId": "999990005263746",

   "name": "BASE FINA BFP"

   },

   {

   "choiceId": "999990010517817",

   "name": "5 Quesos Gourmet"

   },

   {

   "choiceId": "999990006472065",

   "name": "Fina masa"

   },

   {

   "choiceId": "999990010429777",

   "name": "Salsa Barbacoa"

   },

   {

   "choiceId": "999990010902209",

   "name": "Topping a Base de Mozzarella"

   },

   {

   "choiceId": "999990010905260",

   "name": "Bacon"

   },

   {

   "choiceId": "999990010902269",

   "name": "Bacon Crispy"

   }

   ]

   }

   ],

   "size": 0,

   "units": 1,

   "price": 23,

   "productLineId": 1

   },

   {

   "products": [

   {

   "name": "Bacon Crispy Gourmet",

   "description": "Si eres fan del bacon, aquí tienes ración doble:
   ahumado y crispy.

   Una sabrosa mezcla acompañada por nuestra tradicional salsa barbacoa
   y la doble masa rellena de 5 quesos.",

   "partialProductId": "999990010908732",

   "choices": [

   {

   "choiceId": "999990005263746",

   "name": "BASE FINA BFP"

   },

   {

   "choiceId": "999990010517817",

   "name": "5 Quesos Gourmet"

   },

   {

   "choiceId": "999990006472065",

   "name": "Fina masa"

   },

   {

   "choiceId": "999990010429777",

   "name": "Salsa Barbacoa"

   },

   {

   "choiceId": "999990010902209",

   "name": "Topping a Base de Mozzarella"

   },

   {

   "choiceId": "999990010905260",

   "name": "Bacon"

   },

   {

   "choiceId": "999990010902269",

   "name": "Bacon Crispy"

   }

   ]

   }

   ],

   "size": 0,

   "units": 1,

   "price": 23,

   "productLineId": 2

   }

   ],

   "originalPrice": 45.9,

   "totalPrice": 45.9,

   "promotions": []

   },

   "creationDate": "0001-01-01T00:00:00"

}

10. ESTABLECER UNA DIRECCIÓN DE ENTREGA 
========================================

Para establecer una dirección de entrega, es necesario hacer dos
llamadas a los siguientes endpoints:

   GET /order/getaddress

Esta llamada se encarga de obtener la información necesaria para
establecer la dirección de entrega.

   Los parámetros de entrada son:

================== ==================================================================== =============================
   Parámetro       Descripción                                                             Ejemplo
================== ==================================================================== =============================
   **x-auth-back** Autorización para el servicio con el token obtenido en la llamada al    “Bearer
                                                                                       
                   “POST /connect/token”: token_type + “ ” + access_token                  eyJhbGciOiJodHRwOi8vd3d3…”
================== ==================================================================== =============================

..

   Los parámetros de salida son:

======================= =========================================================================================== =======
   Parámetro               Descripción                                                                              Ejemplo
======================= =========================================================================================== =======
   **primaryField[]**      Campos adicionales para identificar el domicilio del cliente (Portal, Piso, Letra, etc.)
   **secondaryField[]**    Campos adicionales para identificar el domicilio del cliente (Portal, Piso, Letra, etc.)
======================= =========================================================================================== =======

============= ==================================================================== ==========
\                **primaryField[], secondaryField[]**                             
============= ==================================================================== ==========
**key**          Identificador del campo                                              1
**label**        Nombre que identifica el campo que se va a informar en el “value”    “Letra”
**value**        Valor asociado al campo “label”                                      “A”
**maxLenght**    Longitud máxima de caracteres que tendrá el campo “value”.           3
**editable**     Si el campo value se puede editar o no (True o False)                True
============= ==================================================================== ==========

..

   Ejemplo de respuesta:

.. code-block:: json

   {

   "primaryField": [

   {

   "key": "county",

   "label": "Provincia",

   "value": "ZARAGOZA",

   "editable": false,

   "maxLength": -1

   },

   {

   "key": "city",

   "label": "Localidad",

   "value": "ZARAGOZA",

   "editable": false,

   "maxLength": -1

   },

   {

   "key": "street",

   "label": "Nombre de vía",

   "value": "CALLE JULIO CORTAZAR",

   "editable": true,

   "maxLength": -1

   },

   {

   "key": "house_number",

   "label": "Número",

   "value": "19",

   "editable": true,

   "maxLength": -1

   }

   ],

   "secondaryField": [

   {

   "key": null,

   "label": "Bloque",

   "value": null,

   "editable": true,

   "maxLength": 5

   },

   {

   "key": null,

   "label": "Escalera",

   "value": null,

   "editable": true,

   "maxLength": 3

   },

   {

   "key": null,

   "label": "Piso",

   "value": null,

   "editable": true,

   "maxLength": 3

   },

   {

   "key": null,

   "label": "Puerta",

   "value": null,

   "editable": true,

   "maxLength": 3

   }

   ] }

   POST /order/delivery

Esta llamada se encarga de establecer una dirección de entrega al pedido
en curso.

   Los parámetros de entrada son:

=================== ============================================================================================================================== ==========================
   Parámetro           Descripción                                                                                                                 Ejemplo
=================== ============================================================================================================================== ==========================
   **x-auth-back**     Autorización para el servicio con el token obtenido en la llamada al “POST /connect/token”: token_type + “ ” + access_token “Bearer
                                                                                                                                                  
                                                                                                                                                   eyJhbGciOiJodHRwOi8vd3d3…”
**deliveryOrder[]**    Información asociada al reparto del pedido                                                                                 
=================== ============================================================================================================================== ==========================

======================== ===================================================================
**deliveryInputOrder[]**                                                                    
======================== ===================================================================
**phone**                   Teléfono del cliente                                            
**deliveryObservations**    Notas que se tendrán en cuenta a la hora de realizar el reparto.
**address[]**               Dirección del cliente donde se repartirá el pedido              
======================== ===================================================================

==================== ===========================================================================================
\                       **address[]**                                                                           
==================== ===========================================================================================
**primaryField[]**      Campos adicionales para identificar el domicilio del cliente (Portal, Piso, Letra, etc.)
**secondaryField[]**    Campos adicionales para identificar el domicilio del cliente (Portal, Piso, Letra, etc.)
==================== ===========================================================================================

============= ==================================================================== ==========
\                **primaryField[], secondaryField[]**                             
============= ==================================================================== ==========
**key**          Identificador del campo                                              1
**label**        Nombre que identifica el campo que se va a informar en el “value”    “Letra”
**value**        Valor asociado al campo “label”                                      “A”
**maxLenght**    Longitud máxima de caracteres que tendrá el campo “value”.           3
**editable**     Si el campo value se puede editar o no (True o False)                True
============= ==================================================================== ==========

Ejemplo de parámetros de entrada:

.. code-block:: json

   {

   "Phone": "943546576",

   "deliveryObservations": "Sin observaciones",

   "address": {

   "primaryfield": [

   {

   "key": "county",

   "label": "Provincia",

   "value": "ZARAGOZA",

   "editable": false,

   "max_length": -1

   },

   {

   "key": "city",

   "label": "Localidad",

   "value": "ZARAGOZA",

   "editable": false,

   "max_length": -1

   },

   {

   "key": "street",

   "label": "Nombre de vía",

   "value": "CALLE EMILIA PARDO BAZAN", "editable": true,

   "max_length": -1

   },

   {

   "key": "house_number",

   "label": "Número",

   "value": "22",

   "editable": true,

   "max_length": -1

   }

   ],

   "secondaryfield": [

   {

   "key": null,

   "label": "Bloque",

   "value": "1",

   "editable": true,

   "max_length": 5

   },

   {

   "key": null,

   "label": "Escalera",

   "value": "3",

   "editable": true,

   "max_length": 3

   },

   {

   "key": null,

   "label": "Piso",

   "value": "5",

   "editable": true,

   "max_length": 3

   },

   {

   "key": null,

   "label": "Puerta",

   "value": "D",

   "editable": true,

   "max_length": 3

   }

   ]

   }

   }

11. OBTENER MEDIOS DE PAGO 
===========================

   GET /shop/{id}/payments

Esta llamada permite obtener un listado de los diferentes medios de pago
que permite una tienda en concreto por medio del identificador de la
tienda.

Los parámetros de entrada son:

================== ============================================================================= ==========================
   Parámetro          Descripción                                                                Ejemplo
================== ============================================================================= ==========================
   **x-auth-back**    Autorización para el servicio con el token obtenido en la llamada al “POST “Bearer
                                                                                                
                      /connect/token”:                                                           eyJhbGciOiJodHRwOi8vd3d3…”
                                                                                                
                      token_type + “ ” + access_token                                           
   **id**             Número que identifica la tienda en nuestro sistema.                        “00145”
================== ============================================================================= ==========================

Como resultado se obtendrá un listado con los medios de pago disponibles
para la tienda que se desea consultar.

   Los parámetros de salida son:

========================== ========================================================================================================================================================================================== ======================================================
   Parámetro                  Descripción                                                                                                                                                                             Ejemplo
========================== ========================================================================================================================================================================================== ======================================================
   **electronicPaymentId**    Identificador del medio de pago                                                                                                                                                         “1”
   **paymentTypeName**        Nombre del medio de pago elegido                                                                                                                                                        “Efectivo Euros”
   **changeEfective[]**       En caso de seleccionar el tipo de pago del pedido en Efectivo, devuelve un listado con el cambio, en monedas o billetes, que dispondrá el repartidor como máximo para afrontar el pago. “10”
                                                                                                                                                                                                                     
                                                                                                                                                                                                                      (El repartidor sólo llevará 10€ de cambio como máximo)
   **isExternalPayment**      Indica si el un medio de pago externo o medio de pago electrónico. (True o False)                                                                                                       false
   **tokenType**              0 -> Medio de pago no Tokenizable. 1 -> Tokenizable con restricciones por usuario.                                                                                                      0
                                                                                                                                                                                                                     
                              2 -> Tokenizable                                                                                                                                                                       
========================== ========================================================================================================================================================================================== ======================================================

**El campo tokenType**: este campo es relativo a la propiedad de
“billing_agreement” de la pasarela de pagos y vendrá definida por un
checkbox que el usuario pueda marcar cuando vaya a pagar, en la pantalla
de selección de medio de pago. Es importante definir los casos en los
que dicho checkbox estará disponible o no para ser marcado por el
usuario. Por esta razón se dispondrá de una propiedad numérica
denominada “\ **tokenType**\ ” al momento de pedir la información de los
medios de pago de la tienda.

Los posibles valores que puede tomar este campo, y lo que representa
cada uno, son:

-  **Valor numérico 0**: El medio de pago **NO es tokenizable**, por lo
   tanto el checkbox no debería poder usarse. Esto implica que al llamar
   a la pasarela el campo “billing_agreement” debería ser False.

-  **Valor numérico 1**: El medio de pago **SÍ es tokenizable PERO con
   una restricción**. Esta restricción consiste en que solamente se
   puede guardar un único token para este medio de pago por usuario.

..

   Esto significa que para saber si el checkbox debe estar disponible
   para el usuario hay que, primero, revisar si dicho usuario tiene
   tokens asociados a ese medio de pago (lo cual se conoce a través de
   la llamada a “\ *GET /customer/{id}/allpayments*\ ”) y, si no tiene
   ninguno, tendrá el checkbox disponible. Mientras que si tiene algún
   token asociado a dicho medio de pago el checkbox no estará disponible
   para el usuario. El campo “billing_agreement” que se utiliza al
   llamar a la pasarela deberá ser False para este segundo caso;
   mientras que, por otro lado, será dependiente de que el checkbox esté
   marcado o desmarcado para pasar un True o False en el primer caso.

-  **Valor numérico 2**: El medio de pago **SÍ es tokenizable** y sin
   restricciones, por lo tanto, el checkbox debería estar activo siempre
   que se selecciona un medio de pago con este tipo de token. El campo
   “billing_agreement” que se utiliza al llamar a la pasarela es
   entonces dependiente de que el checkbox esté marcado o desmarcado
   para pasar un True o False respectivamente.

..

   Ejemplo de respuesta:

.. code-block:: json

   [

   {

   "electronicPaymentId": "1",

   "paymentTypeName": "Efectivo Euros",

   "changeEfective": [

   10,

   20,

   30,

   40

   ],

   "isExternalPayment": false,

   "tokenType": 0

   },

   {

   "electronicPaymentId": "2",

   "paymentTypeName": "Tarjeta",

   "changeEfective": null,

   "isExternalPayment": false,

   "tokenType": 0

   } ]

12. FINALIZAR PEDIDO 
=====================

   POST /order/save

   POST /order/savelocal

Por medio de esta llamada se permite finalizar el pedido (básico o de
kiosco) realizando el pago mediante el medio de pago en concreto.

   Los parámetros de entrada son:

======================== ================================================================================================================================ ==========================
   Parámetro                Descripción                                                                                                                   Ejemplo
======================== ================================================================================================================================ ==========================
   **x-auth-back**          Autorización para el servicio con el token obtenido en la llamada al “POST /connect/token”: token_type + “ ” + access_token   “Bearer
                                                                                                                                                         
                                                                                                                                                          eyJhbGciOiJodHRwOi8vd3d3…”
   **paymentType**          Tipo de medio de pago con el que se va a realizar el pago del pedido.                                                            “1”
                                                                                                                                                         
                            Cash = 1,                                                                                                                    
                                                                                                                                                         
                            TicketRestaurant = 4,                                                                                                        
                                                                                                                                                         
                            Dataphone = 16, PayPal = 20,                                                                                                 
                                                                                                                                                         
                            ConexFlow = 21,                                                                                                              
                                                                                                                                                         
                            Kuapay = 25,                                                                                                                 
                                                                                                                                                         
                            WebPay = 26,                                                                                                                 
                                                                                                                                                         
                            Iuapay = 27, PayU = 28,                                                                                                      
                                                                                                                                                         
                            RedSys = 32,                                                                                                                 
                                                                                                                                                         
                            PayMe = 33,                                                                                                                  
                                                                                                                                                         
                            PayTPV = 43                                                                                                                  
   **digitCard**            Número de tarjeta de crédido, solo sí se selecciona el medio de pago Tarjeta de Crédito en el campo “paymentType”             vacío
   **Token**                Cadena identificativa del pago electrónico                                                                                    vacío
   **clientCash**           Cambio de dinero en efectivo que tendrá que disponer el repartidor para el pago del pedido en efectivo y entrega a domicilio. “50”
   **OrderObservations**    Notas de elaboración del pedido                                                                                              
======================== ================================================================================================================================ ==========================

..

   Ejemplo de parámetros de entrada:

.. code-block:: json

   {

   "paymentType": 1,

   "digitCard": null,

   "token": null,

   "clientcash": 50,

   "OrderObservations": "Sin observaciones"

   }

La respuesta al grabar el pedido contendrá la información de la tienda
que suministra los productos, los datos relevantes de la dirección de
entrega junto con el precio del pedido, el coste de reparto y la hora de
reparto.

   Ejemplo de respuesta:

.. code-block:: json

   {

   "orderId": "12",

   "deliveryNoteId": "501",

   "address": "",

   "shopAddress": "Virgen de Aranzazu 33",

   "orderObservations": "Sin observaciones",

   "totalPrice": 20.95,

   "deliveryCost": 12.3,

   "email": "marcelino@pan.vino",

   "deliveryTime": "2019-04-25T16:45:34.6696507+00:00",

   "shopPhone": "914544567"

   }

.. |image0| image:: media/image1.png
   :width: 3.07874in
   :height: 0.81102in
.. |image1| image:: media/image2.png
   :width: 6.19375in
.. |image2| image:: media/image2.png
   :width: 6.19375in
.. |image3| image:: media/image3.jpg
   :width: 5.04744in
   :height: 2.44044in
.. |image4| image:: media/image4.jpg
   :width: 5.06944in
   :height: 1.59028in
.. |image5| image:: media/image5.jpg
   :width: 6.29097in
   :height: 4.19722in
