.. _preparation:

General Preparation
===================

You can use multiple programming languages with Panda3D. The most
commonly used language is Python, followed by C++. Some manual pages offer
both C++ and Python information - use the toggle button at the top of the
manual page to toggle between C++ information and Python information.

.. only:: python

    For example, if you want this page to contain information about learning C++
    instead of learning Python, just hit the "C++" link at the top of this page.

.. only:: cpp

    For example, if you want this page to contain information about learning
    Python instead of learning C++, just hit the "Python" link at the top of this
    page.

.. only:: python

    Learning Python
    ---------------
    
    Since Panda3D is a library, and not a point-n-click game maker, it is needed
    to learn Python or C++ before you will be able to use it. Since this engine's
    main goal is to support Python, it would be a good idea to familiarize
    yourself with Python before continuing.
    
    Python is an interpreted, interactive, object-oriented language comparable to
    Java or Perl. It is available on several platforms, including UNIX, Windows,
    OS/2, and Mac. Python also has a large number of modules outside of the
    standard Python installation, and additional modules can be created in C or
    C++. Because it is late-binding and requires minimal memory management, it is
    an ideal language for rapid prototyping.
    
    The Copy of Python that comes with Panda
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    
    On Windows, it is not necessary to install Python, because the Windows
    installer for Panda3D includes a copy. This is a completely normal copy of
    Python, identical to what you would have if you installed Python using the
    standard Python installer. Panda's built-in copy of Python is automatically
    added to the PATH environment variable. This enables you to type "python" at
    the command prompt, and it will run the Python that comes with Panda.
    
    On macOS, Python 2.7 already comes pre-installed on your system, which will
    work with Panda3D. However, it is recommended to install a newer version of
    Python from https://www.python.org/downloads/mac-osx/ .
    
    What if I already have a copy of Python?
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    
    If you already have a copy of Python, and you wish to use that instead of the
    one provided with panda, it is easy to do so. Simply create a "panda.pth" file
    inside your copy of Python, containing the path of the panda directory and the
    bin directory within it on separate lines (for example C:\Program
    Files\Panda3D-1.2.3 and C:\Program Files\Panda3D-1.2.3\bin). This will enable
    your copy of Python to find the panda libraries.
    
    For this to work, the version of Python that you use must match the version of
    Python included with panda. The panda libraries are compiled for that
    particular version, and will not work with any other.
    
    Of course, if you do use your own copy of Python, you may wish to delete
    panda's copy of Python, or at least, remove it from the PATH environment
    variable. Otherwise, you will have two copies of Python, which can lead to
    confusion.
    
    Python Programming Resources
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    
    There are a lot of other resources available for programming in Python. Here
    is a list of some of the best:
    
    Links from the official Python website:
    
    -  `Official Website - http://www.python.org <http://www.python.org>`__
    -  `Current Python Documentation <http://docs.python.org/>`__
    -  `Python Documentation <http://www.python.org/doc>`__
    
    
    Here are some other good links for learning Python:
    
    -  `Byte of Python <http://www.swaroopch.com/notes/python/>`__
    -  `Dive Into Python <http://www.diveintopython.net>`__
    -  `Beginner's Guide from Python's wiki <http://wiki.python.org/moin/BeginnersGuide>`__
    -  `The Standard Python Library <http://www.effbot.org/zone/librarybook-index.htm>`__
    -  `Introduction to OOP with Python <http://www.voidspace.org.uk/python/articles/OOP.shtml>`__

.. only:: cpp

    Learning C++
    ------------
    
    It is possible to write Panda3D programs using C++. However, since most of the
    documentation uses Python, it may be better to learn Panda3D using Python
    first, and then switch to C++ later. If you do switch, the function calls are
    very similar.
    
    C++ is an object-oriented high-level multi-purpose language. It is actually a
    copy of the C programming language, but object-oriented, with more functions.
    Here are a few links to C++ tutorials that might be useful for you:
    
    -  http://www.cplusplus.com/doc/tutorial/
    -  `www.learncpp.com <http://www.learncpp.com/>`__
    
    The binaries of the last Windows release are built with Microsoft Visual C++
    2015. If you want to use the provided binaries you can use this version, but
    2017 will work as well.
    
    If you wish to use another version you will have to build Panda from source.
    Note that if you do that you will need all the dependencies (such us libjpeg,
    libpng, etc) built by the same compiler than you are using. You can do this
    yourself or look around for 3rd party binaries.
    
    On UNIX-like operating systems you can use the LLVM Clang or GNU G++ compiler.
    
    For information about compiling your C++ program, see
    :ref:`this page <how-to-compile-a-c++-panda3d-program>`.

