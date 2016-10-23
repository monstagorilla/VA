## VA

VA is the acronym for Virtual Acoustics, the generation of sound based on computation, simulation and reproduction.
The main goal is to support those who want to create and design audio-visual Virtual Reality.
VA provides acoustics applications with real-time capability that can be controlled by a network interface.

This project includes *C++ libraries*, *bindings* to other programming and scripting languages as well as *applications* for real-time auralization.


### License

**Copyright 2015-2016 Institute of Technical Acoustics, RWTH Aachen University.**

In general, the VA interface and all binding libraries, wrappers and marshallers are licensed under the [Apache License, Version 2.0](http://www.apache.org/licenses/LICENSE-2.0).
**The core functionality (VACore), on the other hand, is copyright protected. Any usage and distribution is prohibited, unless explicitly granted by the authors. 
Also, every add-on, plugin, renderer or reproduction that include third-party libraries may have different licenses.**

> For more information, read the LICENSE.md file in the respective submodule folders and take into consideration the third-party libraries used.



### Quick build guide

VA is a collective of repositories. Clone VA using the --recursive flag, since it mostly consists of submodules.
Use CMake and the top-level CMakeLists.txt of the VA project. It will generate a poject file with alle sub-projects included.
VA makes heavy use of [ViSTA, the Virtual Reality Toolkit](https://sourceforge.net/projects/vistavrtoolkit/files/) developed by the [Virtual Reality Group of the IT Center, RWTH Aachen University](http://www.itc.rwth-aachen.de/cms/IT-Center/Forschung-Projekte/~eubl/Virtuelle-Realitaet/).
Hence, the build environment requires the VistaCMakeCommon extension for CMake in order to define and resolve all required dependencies.
For more information, see the [Wiki pages of ITACoreLibs](https://git.rwth-aachen.de/ita/ITACoreLibs/wikis/home).


#### Updating

To update to HEAD use the command `git submodule update --remote` in order to also update the submodules. Make sure that your local changes are not overwritten.

