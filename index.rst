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
   :caption: Introducción:

   rst_docs/es_es/Welcome/acceso_plataforma_gtu
 
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


.. code-block:: c

	/* Or say "highlight::" once to set the language for all of the
	code blocks that follow it. Options include ":linenos:",
	":linenothreshold:", and ":emphasize-lines: 1,2,3". */
	
	char s[] = "You can also say 'python', 'ruby', ..., or 'guess'!";

.. literalinclude:: example.py
   :lines: 10-20
   :linenos:
   
.. literalinclude:: index.rst
   :lines: 79-85
   :emphasize-lines: 3,5
   :linenos:
   
.. index::
   single: funcion; johnny()
 
.. literalinclude:: johnny.json
   :lines: 1-211
   :emphasize-lines: 3,5
   :linenos:

Examples:

.. epigraph::

	No matter where you go, there you are.
	
	-- Buckaroo Banzai
		
		
.. compound::

   The 'rm' command is very dangerous.  If you are logged
   in as root and enter ::

       cd /
       rm -rf *

   you will erase the entire contents of your file system.
   
Link:

see :ref:`my-reference-label`

.. note::

   This function is not suitable for sending spam e-mails.
   
.. warning::

	Agitar antes de hablar
   
.. error::

	Se admite quejas

.. seealso::

   Module :ref:`my-reference-label`
      Documentation of the :ref:`my-reference-label` standard module.

   `GUIA FUNCIONAL <rst_docs/es_es/guia_funcional/indice_funcional/>`_
      Documentation for tar archive files, including GNU tar extensions.

.. versionadded:: 2.5
   The *spam* parameter.
	  
.. centered:: **LICENSE AGREEMENT**

.. hlist::
   :columns: 3

   * A list of
   * short items
   * that should be
   * displayed
   * horizontally

.. index:: 
   single: función; some_function()

.. code-block:: python
   :emphasize-lines: 3,5

   def some_function():
       interesting = False
       print 'This line is highlighted.'
       print 'This one is not...'
       print '...but this one is.'
	   
.. code-block:: python
   :caption: this.py
   :name: this-py

   print 'Explicit is better than implicit.'
   
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


.. list-table:: Frozen Delights!
   :widths: 15 10 30
   :header-rows: 1

   * - Treat
     - Quantity
     - Description
   * - Albatross
     - 2.99
     - On a stick!
   * - Crunchy Frog
     - 1.49
     - If we took the bones out, it wouldn't be
       crunchy, now would it?
   * - Gannet Ripple
     - 1.99
     - On a stick!
	 

.. csv-table:: Frozen Delights!
   :header: "Treat", "Quantity", "Description"
   :widths: 15, 10, 30

   "Albatross", 2.99, "On a stick!"
   "Crunchy Frog", 1.49, "If we took the bones out, it wouldn't be
   crunchy, now would it?"
   "Gannet Ripple", 1.99, "On a stick!"

Indices and tables
==================

* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`

.. |guia_funcional| image:: images/index/guia_funcional.png
   :target: rst_docs/es_es/guia_funcional/index_func.html
   :width: 130px
   :height: 86px
.. |guia_tecnica| image:: images/index/guia_tecnica.png
   :target: rst_docs/es_es/guia_tecnica/indice_tecnico.html
   :width: 130px
   :height: 86px
.. |guia_configuracion| image:: images/index/guia_configuracion.png
   :target: rst_docs/es_es/guia_configuracion/indice_configuracion.html
   :width: 130px
   :height: 86px
.. |gtu| image:: images/index/gtu.png
   :target: https://grupotelepizzauniversity.telepizza.com/
   :width: 130px
   :height: 86px

.. index::
   single: función; codeauthor
