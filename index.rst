.. header:: This space for rent.

.. techDocTLPZ documentation master file, created by
   sphinx-quickstart on Tue Jun 25 10:03:19 2019.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Bienvenidos a la documentación del futuro!
==========================================

|guia_funcional| |guia_tecnica| |guia_configuracion| |gtu|

.. toctree::
   :maxdepth: 1
   :hidden:
   :caption: PRODUCTO:

   rst_docs/es_es/producto/intro_producto_billing
   
..   rst_docs/es_es/Welcome/acceso_plataforma_gtu
   

.. toctree::
   :maxdepth: 1
   :hidden:
   :caption: Guía Funcional:
   
   rst_docs/es_es/guia_funcional/index_func
 
.. toctree::
   :maxdepth: 1
   :hidden:
   :caption: Guía Técnica:
   
   rst_docs/es_es/guia_tecnica/indice_tecnico
   rst_docs/es_es/guia_tecnica/orderflow/orderflow_2
   rst_docs/es_es/guia_tecnica/apificacion/funcionalidades_apificacion
   rst_docs/es_es/guia_tecnica/data_lake/plantilla_documentacion_Fomento_uso_app_v02

.. toctree::
   :maxdepth: 1
   :hidden:   
   :caption: Guía de configuración:
   
   rst_docs/es_es/guia_configuracion/indice_configuracion

.. toctree::
   :maxdepth: 1
   :hidden:
   :caption: Anexo:
   
   rst_docs/plantillas/tobbi
   
   
.. toctree::
   :maxdepth: 1
   :hidden:
   :caption: Glosario:
   
   rst_docs/es_es/glosario/glosario_es



.. note::

   This function is not suitable for sending spam e-mails.
   
.. warning::

	Agitar antes de hablar
   
.. error::

	Se admite quejas

.. seealso::

   Module .
      Documentation of the :ref:`my-reference-label` standard module.

   `GUIA FUNCIONAL <rst_docs/es_es/guia_funcional/indice_funcional/>`_
      Documentation for tar archive files, including GNU tar extensions.


.. index:: 
   single: función; some_function()

.. code-block:: python
   :emphasize-lines: 3,5

   def some_function():
       interesting = False
       print 'This line is highlighted.'
       print 'This one is not...'
       print '...but this one is.'

   
.. glossary::

   environment
      A structure where information about all documents under the root is
      saved, and used for cross-referencing.  The environment is pickled
      after the parsing stage, so that successive runs only need to read
      and parse new and changed documents.

   source directory
      The directory which, including its subdirectories, contains all
      source files for one Sphinx project.

.. codeauthor:: Thorvaldur Konradsson <thorvaldurk@vectoritcgroup.com>


Indices y tablas
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`

.. |guia_funcional| image:: images/index/guia_funcional.png
   :target: rst_docs/es_es/guia_funcional/index_func.html
   :width: 24%
.. |guia_tecnica| image:: images/index/guia_tecnica.png
   :target: rst_docs/es_es/guia_tecnica/indice_tecnico.html
   :width: 24%
.. |guia_configuracion| image:: images/index/guia_configuracion.png
   :target: rst_docs/es_es/guia_configuracion/indice_configuracion.html
   :width: 24%
.. |gtu| image:: images/index/gtu.png
   :target: https://grupotelepizzauniversity.telepizza.com/
   :width: 24%

.. index::
   single: función; codeauthor
