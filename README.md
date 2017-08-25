## VA (Virtual Acoustics)

VA is the acronym for Virtual Acoustics, the generation of sound based on computation, simulation and reproduction.
The main goal is to support those who want to create and design audio-visual Virtual Reality.
VA provides acoustics applications with real-time capability that can be controlled by a network interface.

This project includes *C++ libraries*, *bindings* to other programming and scripting languages as well as *applications* for real-time auralization.

### History

VA was invented and developed in a series of scientific projects at the Institute of Technical Acoustics (ITA), RWTH Aachen University. If was first introduced as a software application for loudspeaker-based real-time auralization by Tobias Lentz. It made progress with a joint project for an audio-visual application in the first CAVE-like system in close collaboration with the [Virtual Reality Group lead by Prof. Torsten Kuhlen at the RWTH Aachen University](http://www.vr.rwth-aachen.de).

After the decision to build a [new CAVE-like system](http://www.itc.rwth-aachen.de/cms/IT-Center/Forschung-Projekte/Virtuelle-Realitaet/Infrastruktur/~fgqa/aixCAVE/), Frank Wefers started to re-invent Virtual Acoustics under the acronym VA and added support for a sophisticated C++ interfacing that also allows for networked bindings to be implemented fairly easy.


### License

**Copyright 2015-2017 Institute of Technical Acoustics, RWTH Aachen University.**

Licensed under the Apache License, Version 2.0 (the "License");
you may not use files of this project except in compliance with the License.
You may obtain a copy of the License at

<http://www.apache.org/licenses/LICENSE-2.0>

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.


### Quick build guide

VA is a collective of repositories. Clone VA using the `--recursive` flag, since it mostly consists of submodules.
Use CMake and the top-level CMakeLists.txt of the VA project. It will generate a poject file with alle sub-projects included.
VA makes heavy use of [ViSTA, the Virtual Reality Toolkit](https://sourceforge.net/projects/vistavrtoolkit/files/) developed by the [Virtual Reality Group of the IT Center, RWTH Aachen University](http://www.itc.rwth-aachen.de/cms/IT-Center/Forschung-Projekte/~eubl/Virtuelle-Realitaet/).
Hence, the build environment requires the VistaCMakeCommon extension for CMake in order to define and resolve all required dependencies.
For more information, see the [Wiki pages of ITACoreLibs](https://git.rwth-aachen.de/ita/ITACoreLibs/wikis/home).


#### Working with submodules

If you do not want to make any changes but update to the latest HEAD revision, use the command `git submodule update --remote` in order to also update the submodules.
If you have already made changes, updating will fail.

If you want to make changes to the submodule project, browse into the directory and checkout a branch since a submodule is per default detached from HEAD, i.e. the master branch:

`git checkout master`

Now make your changes, add, commit and push from this location as usual.

For convenience, GIT also provides a batch command that performs the call for each submodule:
`git submodule foreach git checkout master`


#### Switching to a specific version

To use a spefic version of a VA submodule, say VACore, you have to `cd VACore` and `git checkout v2017.a`. From now on, take care when updating.


#### Cleaning up

Sometimes, submodules remain in a dirty state because files are created by the build environment - which are not under version control.
If you want to clean up your working directory, use `git submodule foreach git clean -f -n`, and remove the `-n` if your are sure to remove the listed files in the submodules.
Afterwards, a `git submodule status` should return a clean copy.
