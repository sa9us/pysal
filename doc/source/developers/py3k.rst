=================
PySAL and Python3
=================
.. contents::

Background
----------

PySAL Enhancement Proposal #9 was approved February 2, 2011. It called for
adapting the code base to support both Python 2.x and 3.x releases. 

Setting up for development
--------------------------

First install `Python3 <http://python.org/download/releases/3.2.2/>`_.
Once Python3 is installed, you have the choice of downloading the following
files as pure source code from PyPi and running "python3 setup.py install" for
each, or follow the instructions below to setup useful helpers: easy_install and
pip.

To get setuptools and pip, first get distribute from PyPi::
 
 curl -O http://python-distribute.org/distribute_setup.py
 python3 distribute_setup.py
 # Now you have easy_install
 # It may be useful to setup an alias to this version of easy_install in your shell profile
 alias easy_install3='/Library/Frameworks/Python.framework/Versions/3.2/bin/easy_install'

After distribute is installed, get pip::

  curl -O https://raw.github.com/pypa/pip/master/contrib/get-pip.py
  python3 get-pip.py
  # It may be useful to setup an alias to this version of pip in your shell profile
  alias pip3='/Library/Frameworks/Python.framework/Versions/3.2/bin/pip'


NumPy and SciPy require extensive refactoring on installation. We recommend
`downloading the source code <http://new.scipy.org/download.html>`_, unzipping,
and running::

  cd numpy<dir>
  python3 setup.py install 
  # If all looks good, cd outside of the source directory, and verify import 
  cd
  python3 -c 'import numpy'

Be sure to install NumPy first since SciPy depends on it. Now install SciPy in
the same manner::

  cd scipy<dir>
  python3 setup.py install
  # After extensive building, if all looks good, cd outside of the source directory, and verify import 
  cd
  python3 -c 'import scipy'

Post any installation-related issues to the pysal-dev mailing list. 
If python complains about not finding gcc-4.2, and you're sure it is installed,
(run "gcc --version" to verify), you may create an alias to satisfy this::

  cd /usr/bin/ 
  sudo ln -s gcc  gcc-4.2


Now for PySAL. Get the bleeding edge repository version of PySAL and pass in
this call::
 
 cd pysal/trunk
 python3 setup.py install

You'll be able to watch the dynamic refactoring taking place. If all goes well,
PySAL will be installed into your Python3 site-packages directory. Confirm
success with:: 

  cd
  python3 -c 'import pysal; pysal.open.check()'


Optional Installations
-----------------------

Now that you have pip, get iPython::

 # Use pip from the Python3 distribution on your system, or with the alias above
 pip3 install iPython

The first time you launch iPython3, you may receive a warning about the Python
library readline. The warning makes it clear that pip does not work to install
readline, so use easy_install, which was installed with distribute above::

 /Library/Frameworks/Python.framework/Versions/3.2/bin/easy_install readline

If when launching iPython3 you receive another warning about kernmagic, note
that iPython 0.12 and newer use an alternate config file from previous versions.
Since I had not extensively customized my iPython profile, I just deleted the
~/.iPython directory and relaunched iPython3.
   
Now let's get our testing and documentation suites::

  pip3 install nose nose-exclude sphinx numpydoc

Now that nose is installed, let's run the test suite. Since the refactored code
only exists in the Python3 site-packages directory, cd into it and run nose.
First, however, copy our nose config files to the installed pysal so that nose
finds them::
 
 cp <path to local pysal svn>/nose-exclude.txt /Library/Frameworks/Python.frameworks/Versions/3.2/lib/python3.2/site-packages/pysal/
 cp <path to local pysal svn>/setup.cfg /Library/Frameworks/Python.frameworks/Versions/3.2/lib/python3.2/site-packages/pysal/
 cd /Library/Frameworks/Python.frameworks/Versions/3.2/lib/python3.2/site-packages
 /Library/Frameworks/Python.framework/Versions/3.2/bin/nosetests pysal > ~/Desktop/nose-output.txt 2>&1


