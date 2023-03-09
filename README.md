# Python 3.9 for Windows 7 and Windows Vista

This repository contains Python 3.9 installers for Windows Vista and Windows 7, as well as the source code of the Python programming language which is adapted for Windows Vista and Windows 7 with instructions on how to build it.

# Overview

## Introduction

Python is a popular programming language that is widely used in almost all fields of software development. Python has been continuously evolving since its inception. However, one significant change which recently occurred in Python was the drop of support for Windows Vista and Windows 7. This decision has sparked a lot of debate among developers and users, with some supporting this change and others giving negative criticism about it.

## Theoretical background

Since version 3.9, Python requires ```api-ms-win-core-path-l1-1-0.dll``` which is absent in Windows 7 and Windows Vista. Prior to version 3.9, the library was dynamically imported;  starting from Python 3.9, [the dynamic load is replaced with a static import](https://github.com/izbyshev/cpython/commit/6a65eba44bfd82ccc8bed4b5c6dd6637549955d5) of ```api-ms-win-core-path-l1-1-0.dll``` which breaks the compatibility with Windows 7 and Windows Vista.

Since the static import of ```api-ms-win-core-path-l1-1-0.dll``` is the only issue which prevents Python 3.9 from running on Windows 7 and Windows Vista, let's build the interpreter in the following way: instead of asking for the file from Windows 7 and Windows Vista, we can directly provide the file ```api-ms-win-core-path-l1-1-0.dll``` to the programming language while building it.

I give credit to [Aohan Dang](https://www.linkedin.com/in/aohan-dang-536643a7/), a professional software developer and Python enthusiast, who has provided a way how to build the Python programming language with ```api-ms-win-core-path-l1-1-0.dll``` which is used here while backporting Python 3.9 to Windows 7. If you need more installers just for Windows 7, you can visit [Aohan's repository](https://github.com/adang1345/PythonWin7). This repository provides installers which are also compatible with Windows Vista.

## Building

1. Install Visual Studio IDE Community edition. Visual Studio is needed to perform a build.
2. Use Visual Studio Installer to install the following development tools:
* Windows Software Development Kit for Windows 11, version 22000. You do not have to install Windows 11, this SDK can be used on Windows 10.
* C++ build tools MSVC v140 for Visual Studio 2015
* C++ build tools MSVC v142 for Visual Studio 2019
* C++ build tools MSVC v143 for Visual Studio 2022
3. Install the [CPython interpreter](https://www.python.org/), version 3.6 or 3.7 or 3.8 on the host machine.
4. Install Sphinx 3.5.4 to this Python interpreter: ```pip install sphinx==3.5.4```. You are free to install Sphinx in a virtual Python environment.
5. Download the source code from this repository as ZIP. I do not recommend using Git. In case some issue happens, this repository may be force pushed. Unpack to a path that does not contain spaces. Otherwise, you may get an error 9009 when executing ```gendef```.
6. Go to Settings -> System -> About -> Advanced system settings -> Environment variables. You can work in the section with system variables as well as in the section with user variables.
* Add a new variable ```PATCHDIR``` and set its value to ```where_you_have_unpacked_zip\cpython-3.9-windows-vista-and-7\Python-3.9.13\api-ms-win-core-path-HACK```. This folder should contain ```x86\api-ms-win-core-path-l1-1-0.dll``` and ```x64\api-ms-win-core-path-l1-1-0.dll```.
* Add a new variable ```PYTHON``` and set its value to the location of the interpreter where Sphinx 3.5.4 is installed. For example, ```PYTHON = C:\Users\User\AppData\Local\Programs\Python\Python38\python.exe```. In order to check where your Python interpreter is located, type ```where python``` in the command line.
7. In the terminal, go to the folder with the Python 3.9 source code and run ```.\Tools\msi\buildrelease.bat -x86 -x64```. If you get an error during the documentation build saying that ```itircl.dll``` was not registered correctly, go to ```externals\windows-installer\htmlhelp``` and run ```regsvr32 itcc.dll```. Another solution to this problem is installing HTML Help Compiler system-wide, which registers this DLL anyway.
8. Obtain installers from ```.\PCbuild\amd64\en-us``` and ```.\PCbuild\amd64\en-us``` and enjoy problem solving with Python!

## Where compatibility problems may arise

**The core CPython 3.9 interpreter and the standard CPython 3.9 libraries run correctly on Windows 7 and Windows Vista.** Nothing is cut or modified in the Python programming language itself, and the interpreter in these installers reads the code in the same exact way as the interpreter in the official Python installers. However, in the future, you may experience compatibility issues with ```pip``` packages. The reason is that developers of these packages may drop support for Windows Vista and Windows 7 considering that officially Python 3.9 is not intended for these operating systems. I cannot provide any help with such kind of troubles because I have no control over thousands of Python software packages. In this case, I recommend contacting the developer of the specific package which causes the compatibility issue.
