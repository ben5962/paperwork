# Paperwork development on Windows


## Build dependencies

It's hard to list all the dependencies here. Currently, the main ones are:

* [Python 3.4](https://www.python.org/downloads/windows/)
* Windows SDK7.1 : See [Python wiki](https://wiki.python.org/moin/WindowsCompilers#Microsoft_Visual_C.2B-.2B-_10.0_standalone:_Windows_SDK_7.1_.28x86.2C_x64.2C_ia64.29)
* [Windows DDK](https://www.microsoft.com/en-us/download/details.aspx?id=11800) (for Pyinsane ; required only if the no Python wheel is available)
* [Pillow](http://www.lfd.uci.edu/~gohlke/pythonlibs/#pillow)
* [Python-levenshtein](www.lfd.uci.edu/~gohlke/pythonlibs/#python-levenshtein)
* [GObject Introspection, Gtk, Gdk, Libpoppler, & friends](https://sourceforge.net/projects/pygobjectwin32/)
* etc

They must be installed *before* the rest of Paperwork. Once everything is installed:

* [Clone](https://git-for-windows.github.io/):
  * ```https://github.com/jflesch/paperwork.git```
  * ```https://github.com/jflesch/paperwork-backend.git```
* Backend can be installed like any Python library (```python setup.py install```). It should
  install a bunch of dependencies as well.
* On the frontend, you can run ```python ./setup.py install``` to fetch all the dependencies
  not listed here. However, it won't create any shortcut or anything. Paperwork startup script
  is installed, but isn't of much help.


## Running


Frontend can be started like on GNU/Linux. Go to where you checked out Paperwork frontend,
and run ```python src\launcher.py```. Tesseract must be in your PATH.


## Packaging

```
cd git\paperwork
pyinstaller pyinstaller\win64.spec
```

It should create a directory 'paperwork' with all the required files, except Tesseract.
This directory can be stored in a .zip file and deploy wherever you wish.


## Adding Tesseract

[PyOCR](https://github.com/jflesch/pyocr) has 2 ways to call Tesseract. Either
by running its executable (module ```pyocr.tesseract```), or using its library
(module ```pyocr.libtesseract```). Currently, for convenience reasons, the
packaged version of Paperwork uses only ```pyocr.tesseract```.

By default, this module looks for tesseract in the PATH only, and let Tesseract
look for its data files in the default location. However, when packaged with
Pyinstaller, PyOCR will also look for tesseract in the subdirectory ```tesseract```
of the current directory (```os.path.join(os.getcwd(), 'tesseract')```). It will
also set an environment variable so Tesseract looks for its data files in
the subdirectory ```data\tessdata```.

So in the end, you can put Paperwork in a directory with the following hierarchy:

```
C:\Program Files (x86)\OpenPaper\ (for example)
|-- Paperwork\ (for example)
    |
    |-- Paperwork.exe
    |-- (...).dll
    |
    |-- Tesseract\
    |   |-- Tesseract.exe
    |   |-- (...).dll
    |
    |-- Data
        |
        |-- paperwork.svg
        |-- (...)
        |
        |-- Tessdata\
            |-- eng.traineddata
            |-- fra.traineddata
```

Note that it will only work if packaged with Pyinstaller.


## Disabling the cruel and unusual DRM

<del>
Paperwork for Windows includes a trial period and an activation key mechanism.
When working on Paperwork, they can be annoying. However, they can be disabled
easily:

* Either patch src/paperwork/frontend/activation/__init__.py:is_activated(). Just add
  ```return True```.
* Or set the environment variable ```PAPERWORK_ACTIVATED``` to ```"true"```
</del>

update: DRM is disabled for now.
