.. _Sign-in Utlities:

==================
Shared utililities
==================

As indicated by the :ref:`Sign-up or sign-in` workflow description, the sign-up and sign-in processes share 3 utility functions to toggle on/off a remember user function and recover lost credentials.  

You can mock out the imports for these modules in your ``conf.py`` with the following snippet::

    import sys
    from unittest.mock import MagicMock

    class Mock(MagicMock):
        @classmethod
        def __getattr__(cls, name):
                return Mock()

    MOCK_MODULES = ['pygtk', 'gtk', 'gobject', 'argparse', 'numpy', 'pandas']
    sys.modules.update((mod_name, Mock()) for mod_name in MOCK_MODULES)

Of course, replacing `MOCK_MODULES` with the modules that you want to mock out.

.. Note:: The library ``unittest.mock`` was introduced on python 3.3. On earlier versions install the ``mock`` library
    from PyPI with (ie ``pip install mock``) and replace the above import::

        from mock import Mock as MagicMock

Workflows for these functions are described below:

.. _Remember me toggle

Remember me toggle function
***************************

.. image:: https://s3.amazonaws.com/peer-downloads/images/TechDocs/Remember+Me.png
    :width: 82%
    :alt: Remember Me Toggle Workflow
|

.. _Recover lost credentials

Recover lost credentials
************************

.. image:: https://s3.amazonaws.com/peer-downloads/images/TechDocs/Remember+Me.png
    :width: 91%
    :alt: Recover Lost Credentials Workflow
|

.. _Reset password

Reset password function
***********************

.. image:: https://s3.amazonaws.com/peer-downloads/images/TechDocs/Reset+Password.png
    :width: 96%
    :alt: Reset Password Workflow
|

