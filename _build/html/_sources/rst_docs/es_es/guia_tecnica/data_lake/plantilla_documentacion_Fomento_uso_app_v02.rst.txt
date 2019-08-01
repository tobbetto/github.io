.. centered:: |image0|

=========================
JOURNEY - Fomento Uso App
=========================

========================== =======================================================================
Fecha: 24 de julio de 2019 Área responsable: Data Lake
========================== =======================================================================

`1. INTRODUCCIÓN 1 <#introduccion>`__

`2. DESARROLLO 2 <#desarrollo>`__

`2.1 Data Extension 2 <#data-extension>`__

`2.2 Automation 3 <#automation>`__

`2.3 Journey 7 <#journey>`__

1. INTRODUCCIÓN
===============

En el presente documento se describe el proceso de desarrollo seguido
para la generación del Journey “Fomento Uso App”.

Dicho proceso se ha estructurado en 3 partes:

-  Creación de Data Extension.

-  Creación de Automation.

-  Creación de Journey.

Además se ha dividido el público objetivo en 3 segmentos:

-  Segmento 1: Ticket Medio <= 10€

-  Segmento 2: 10€ < Ticket Medio <= 15€

-  Segmento 3: Ticket Medio > 15€

2. DESARROLLO
=============

.. _section-1:

2.1 Data Extension
==================

Data Extension son tablas de datos, en nuestro caso utilizadas para
segmentar a los clientes según funcionalidades.

Se han creado las siguientes Data Extension dentro de la ruta “Data
Extensions > Journeys > Fomento Uso App”:

-  Público Objetivo

-  Grupo de Control

-  Sin Grupo de Control

-  Histórico

-  Cupones (x3)

-  Cupones Sin Asignar (x3)

-  Cupones Verificación

2.2 Automation
==============
Automation Builder es la herramienta que ejecuta actividades
de gestión de datos y marketing de forma automática.

|image1| 

Se ha creado un Automation en la ruta “my automations > Journey Builder
Automations > Fomento Uso App”, con los siguientes pasos:

-  STEP 1.1.- **FomentoUsoApp**: Selecciona el público objetivo del
   Journey.

   -  Tipo de actividad: SQL Query

   -  Data Extension objetivo: FomentoUsoApp

   -  Acción sobre Data Extension: Overwrite

   -  Query:

..

   *select ct.\* from ClientesTelepizza ct*

   *where ct.CambiaSegmento = 0*

   *and ct.Fecha_de_ultimo_pedido__c = getdate()-1*

   *and ct.TP_Ticket_Medio__c > 0*

   *and ct.TP_Ticket_Medio__c is not null*

   *and ct.Fecha_Ultimo_Envio <= getdate()-3*

   *and ct.ID not in (select rh.ID from VT_CA_FomentoUsoApp_Historico
   rh*

   *where rh.Id=ct.Id and rh.Fecha_Proceso > getdate()-90)*

   *and ct.Fecha_de_ultimo_pedido__c != ct.Fecha_de_primer_pedido__c*

   *and ct.TP_Contactable__c = 1*

   *and ct.TP_Canal_Habitual__c = 410 or*

   *ct.TP_Canal_Habitual__c = 411 or*

   *ct.TP_Canal_Habitual__c = 420 or*

   *ct.TP_Canal_Habitual__c = 421 or*

   *ct.TP_Canal_Habitual__c = 430 or*

   *ct.TP_Canal_Habitual__c = 431 or*

   *ct.TP_Canal_Habitual__c = 440 or*

   *ct.TP_Canal_Habitual__c = 441 or*

   *ct.TP_Canal_Habitual__c = 434 or*

   *ct.TP_Canal_Habitual__c = 400 or*

   *ct.TP_Canal_Habitual__c = 445 or*

   *ct.TP_Canal_Habitual__c = 433*

-  STEP 2.1.- **FomentoUsoApp_GC_Seg1**: Genera un grupo de clientes
   pertenecientes al publico objetivo sobre los cuales no se aplicará el
   Journey. Este grupo será un 10% del total de clientes del segmento 1.

   -  Tipo de actividad: SQL Query

   -  Data Extension objetivo: FomentoUsoApp_GC_Seg1

   -  Acción sobre Data Extension: Overwrite

   -  Query:

..

   *select top 10 percent \**

   *from VT_CA_FomentoUsoApp*

   *where TP_Ticket_Medio__c <= 10*

   *order by newid()*

⊗Los pasos STEP 3.1 y STEP 4.1 se desarrollan de manera análoga al STEP
2.1, pero con sus respectivos segmentos.

-  STEP 5.1.- **FomentoUsoApp_Exclusion**: Genera el grupo de clientes
   pertenecientes al Grupo de Control, a los cuales se les cambian el
   campo GrupoControl para caracterizarlos.

   -  Tipo de actividad: SQL Query

   -  Data Extension objetivo: FomentoUsoApp

   -  Acción sobre Data Extension: Update

   -  Query:

..

   *select 1 as GrupoControl,fua.Id,fua.ContactKey*

   *from VT_CA_FomentoUsoApp fua*

   *join VT_CA_FomentoUsoApp_GC gc on fua.Id=gc.Id*

*
*

-  STEP 6.1.- **FomentoUsoApp_SinGC**: Genera el grupo de clientes que
   recibirán la comunicación. Para ellos se seleccionan aquellos que no
   hayan sido caracterizados como pertenecientes al Grupo de Control.

   -  Tipo de actividad: SQL Query

   -  Data Extension objetivo: FomentoUsoApp_SinGC

   -  Acción sobre Data Extension: Overwrite

   -  Query:

..

   *select \* from VT_CA_FomentoUsoApp fua*

   *where fua.GrupoControl = 0*

-  STEP 7.1.- **FomentoUsoApp_AsignarCupones_Seg1**: Añade la oferta
   correspondiente al grupo de clientes pertenecientes al público
   objetivo del segmento 1.

   -  Tipo de actividad: SQL Query

   -  Data Extension objetivo: FomentoUsoApp_SinGC

   -  Acción sobre Data Extension: Update

   -  Query:

..

   *Select c.cupon,*

   *cl.Id,*

   *cl.ContactKey*

   *From (*

   *SELECT Id,ContactKey, ROW_NUMBER() OVER (ORDER BY Id) as tp_rank*

   *from VT_CA_FomentoUsoApp fua*

   *where (fua.GrupoControl=0 and fua.TP_Ticket_Medio__c <= 10) ) cl*

   *left Join (*

   *SELECT cupon, ROW_NUMBER() OVER (ORDER BY cupon) as c_rank*

   *from VT_CA_FomentoUsoApp_Cupones_Seg1*

   *where Asignado = 0 ) c*

   *ON cl.tp_rank = c.c_rank*

⊗Los pasos STEP 8.1 y STEP 9.1 se desarrollan de manera análoga al STEP
7.1, pero con sus respectivos segmentos.

-  STEP 10.1.- **FomentoUsoApp_AsignarCupones_Seg1**: Añade la oferta
   correspondiente al grupo de clientes pertenecientes al público
   objetivo del segmento 1.

   -  Tipo de actividad: SQL Query

   -  Data Extension objetivo: FomentoUsoApp_Cupones_Seg1

   -  Acción sobre Data Extension: Update

   -  Query:

..

   *Select 1 as Asignado, GETDATE() as FechaAsignacion, fuasin.Cupon*

   *from VT_CA_FomentoUsoApp_SinGC fuasin*

   *join VT_CA_FomentoUsoApp_Cupones_Seg1 fuacup on
   fuasin.Cupon=fuacup.Cupon*

⊗ Los pasos STEP 10.2 y STEP 10.3 se desarrollan de manera análoga al
STEP 10.1, pero con sus respectivos segmentos.

-  STEP 11.1.- **FomentoUsoApp_CuponesVerificacion**: Genera el grupo de
   clientes pertenecientes al público objetivo que NO ha recibido una
   oferta.

   -  Tipo de actividad: SQL Query

   -  Data Extension objetivo: FomentoUsoApp_Cupones_Verificacion

   -  Acción sobre Data Extension: Overwrite

   -  Query:

..

   *select \**

   *from VT_CA_FomentoUsoApp_SinGC*

   *where cupon is null*

-  STEP 12.1.- **FomentoUsoApp_Cupones_Verificacion**: Realiza un conteo
   de los clientes que NO han sido vinculados con la asignación de una
   oferta. El resultado debe ser 0, si se obtiene un valor diferente se
   produce la parada del Journey y el envío de unos email de aviso a los
   correos correspondientes:

   -  Tipo de actividad: Verfication

-  STEP 13.1.- **FomentoUsoApp_Historico**: Genera el grupo de clientes
   a los cuales se les envia una comunicación, asignando al campo de
   “Fecha Proceso” la fecha en la cual se ha ejecutado el Journey.

   -  Tipo de actividad: Verfication

   -  Data Extension objetivo: FomentoUsoApp_Historico

   -  Acción sobre Data Extension: Update

   -  Query:

..

   *Select fua.*,getDate() as Fecha_Proceso*

   *from VT_CA_FomentoUsoApp fua*

-  STEP 14.1.- **FomentoUsoApp_CuponesSinAsignar_Seg1** Genera el grupo
   de cupones los cuales no han sido asignados a un cliente:

   -  Tipo de actividad: SQL Query.

   -  Data Extension objetivo: FomentoUsoApp_SinAsignar_Seg1

   -  Acción sobre Data Extension: Overwrite

   -  Query:

..

   *SELECT Cupon, Asignado, FechaAsignacion*

   *from VT_CA_FomentoUsoApp_Cupones_Seg1*

   *where Asignado = 0*

⊗Los pasos STEP 14.2 y STEP 14.3 se desarrollan de manera análoga al
STEP 14.1, pero con sus respectivos segmentos.

-  STEP 15.1.- **FomentoUsoApp_Cupones_SinAsignar_Seg1**: Realiza un
   conteo de los cupones que NO han sido asignados con ningún cliente.
   Se especifica un número mínimo de cupones sin asignar de
   aproximadamente la cantidad necesaria para la ejecución del Journey
   durante los próximos 3 días. Cuando el número es inferior al mínimo
   se produce el envío de email de aviso a los correos correspondientes:

   -  Tipo de actividad: Verification.

**⊗** Los pasos STEP 15.2 y STEP 15.3 se desarrollan de manera análoga
al STEP 15.1, pero con sus respectivos segmentos.

2.3 Journey
===========

Journey Builder es la herramienta de planificación de campañas que
permite guiar a los clientes por sus trayectorias con la marca. Una vez
configurado, Journey Builder ejecuta automáticamente campañas con
capacidad de respuesta.

El Journey se ha generado con el siguiente modelo:

|image2|

Se han aplicado las siguientes configuraciones:

-  Filter Criteria: CambiaSegmento = false

-  Exit Criteria: CambioSegmento = true

-  Data Extension: FomentoUsoApp_SinGC

-  Fichas:

   -  Decision Split

..

   |image3|

   -  Einstein STO

   -  Email: Creatividades “UsoApp0719 - TM 10”, ” UsoApp0719 … TM 10-15”,
   “UsoApp0719 - TM 15”

   -  Wait By Duration: 1days

.. |image0| image:: media/image1.png
   :width: 1.27778in
   :height: 1.63383in
.. |image1| image:: media/image2.png
   :width: 6.52778in
   :height: 5.16757in
.. |image2| image:: media/image3.png
   :width: 5.31489in
   :height: 5.50926in
.. |image3| image:: media/image4.png
   :width: 3.77569in
   :height: 2.49583in
