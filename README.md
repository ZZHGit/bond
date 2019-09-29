[![Build Status](https://travis-ci.org/Microsoft/bond.svg?branch=master)](https://travis-ci.org/Microsoft/bond)
[![NuGet](https://img.shields.io/nuget/v/Bond.CSharp.svg?style=flat)](https://www.nuget.org/packages/Bond.CSharp/)

Bond
====

Bond is an open source, cross-platform framework for working with schematized
data. It supports cross-language serialization/deserialization and powerful
generic mechanisms for efficiently manipulating data. Bond is broadly used at
Microsoft in high scale services.

Bond is published on GitHub at [https://github.com/Microsoft/bond/](https://github.com/Microsoft/bond/).

For details, see the User's Manuals for
[C++](https://Microsoft.github.io/bond/manual/bond_cpp.html),
[C#](https://Microsoft.github.io/bond/manual/bond_cs.html) and
[Python](https://Microsoft.github.io/bond/manual/bond_py.html), and the
documentation of the compiler
[tool](https://microsoft.github.io/bond/manual/compiler.html) and
[library](https://hackage.haskell.org/package/bond).

For a discussion how Bond compares to similar frameworks see [Why Bond](https://Microsoft.github.io/bond/why_bond.html).

Dependencies
------------

The Bond repository uses Git submodules and should be cloned with the
`--recursive` flag:

    git clone --recursive https://github.com/Microsoft/bond.git

In order to build Bond you will need CMake (2.8.12+), Haskell (ghc 7.4+ and
cabal-install 1.18+) and Boost (1.54+). The core Bond C++ library can be used
with C++03 compilers, although Python support, unit tests and various examples
require some C++11 features.

Following are specific instructions for building on various platforms.

### Linux

Bond can be built with Clang (3.4+) or GNU C++ (4.7+). We recommend the latest
version of Clang as it's much faster with template-heavy code like Bond.

Run the following commands to install the minimal set of packages needed to
build the core Bond library on Ubuntu 14.04:

    sudo apt-get install \
        clang \
        cmake \
        zlib1g-dev \
        ghc \
        cabal-install \
        libboost-dev \
        libboost-thread-dev

    cabal update
    cabal install cabal-install

In the root `bond` directory run:

    mkdir build
    cd build
    cmake ..
    make
    sudo make install

The `build` directory is just an example. Any directory can be used as the build
destination.

In order to build all the C++ and Python tests and examples, a few more
packages are needed:

    sudo apt-get install \
        python2.7-dev \
        libboost-date-time-dev \
        libboost-test-dev \
        libboost-python-dev

    cabal install happy

Running the following command in the build directory will build and execute all
the tests and examples:

    make --jobs 8 check

(The unit tests are large so you may want to run 4-8 build jobs in parallel,
assuming you have enough memory.)

### OS X

Install Xcode and then run the following command to install the required
packages using Homebrew ([http://brew.sh/](http://brew.sh/)):

    brew install \
        cmake \
        ghc \
        cabal-install \
        boost \
        boost-python

(boost-python is optional and only needed for Python support.)

Update the cabal package database and install `happy` (only needed for tests):

    cabal update
    cabal install happy

Bond can be built on OS X using either standard \*nix makefiles or Xcode. In
order to generate and build from makefiles, in the root `bond` directory run:

    mkdir build
    cd build
    cmake ..
    make
    sudo make install

Alternatively, you can generate Xcode projects by passing the `-G Xcode` option
to cmake:

    cmake -G Xcode ..

You can build and run unit tests by building the `check` target in Xcode or by
running make in the build directory:

    make --jobs 8 check

Note that if you are using Homebrew's Python, you'll need to build
boost-python from source:

    brew install --build-from-source boost-python

and tell cmake the location of Homebrew's libpython by setting the
`PYTHON_LIBRARY` variable, e.g.:

    cmake .. \
        -DPYTHON_LIBRARY=/usr/local/Cellar/python/2.7.9/Frameworks/Python.framework/Versions/2.7/lib/libpython2.7.dylib

### Windows

[![Build Status](https://ci.appveyor.com/api/projects/status/github/Microsoft/bond?svg=true&branch=master)](https://ci.appveyor.com/project/sapek/bond/branch/master)

Install the following tools:

- Visual Studio 2013 or 2015
- CMake ([http://www.cmake.org/download/](http://www.cmake.org/download/))
- Haskell Platform ([http://haskell.org/platform/](http://haskell.org/platform/))

If you are building on a network behind a proxy, set the environment variable
`HTTP_PROXY`, e.g.:

    set HTTP_PROXY=http://your-proxy-name:80

Update the cabal package database:

    cabal update

Now you are ready to build the C# version of Bond. Open the solution file
`cs\cs.sln` in Visual Studio and build as usual. The C# unit tests can
also be run from within the solution.

The C++ and Python versions of Bond additionally require:

- Boost 1.54+ ([http://www.boost.org/users/download/](http://www.boost.org/users/download/))
- Python 2.7 ([https://www.python.org/downloads/](https://www.python.org/downloads/))

You may need to set the environment variables `BOOST_ROOT` and `BOOST_LIBRARYDIR`
to specify where Boost and its pre-built libraries for your environment can be
found, e.g.:

    set BOOST_ROOT=D:\boost_1_57_0
    set BOOST_LIBRARYDIR=D:\boost_1_57_0\lib64-msvc-12.0

The core Bond library and most examples only require Boost headers. The
pre-built libraries are only needed for unit tests and Python support. If Boost
or Python libraries are not found on the system, then some tests and examples will
not be built.

In order to generate a solution to build the C++ and Python versions with Visual
Studio 2013 run the following commands from the root `bond` directory:

    mkdir build
    cd build
    cmake -G "Visual Studio 12 2013 Win64" ..

Instead of `cmake` you can also use `cmake-gui` and specify configuration
settings in the UI. This configuration step has to be performed only once. From
then on you can use the generated solution `build\bond.sln` from Visual Studio
or build from command line using `cmake`:

    set PreferredToolArchitecture=x64
    cmake --build . --target
    cmake --build . --target INSTALL

In order to build and execute the unit tests and examples run:

    cmake --build . --target check -- /maxcpucount:8

Setting `PreferredToolArchitecture=x64` selects the 64-bit toolchain which
dramatically improves build speed. (The Bond unit tests are too big to build
with 32-bit tools.) This variable works for Visual Studio 2013 or 2015. For
Visual Studio 2012 set the following environment variable instead:

    set _IsNativeEnvironment=true



Apache License
                           Version 2.0, January 2004
                        https://www.apache.org/licenses/

   TERMS AND CONDITIONS FOR USE, REPRODUCTION, AND DISTRIBUTION

   1. Definitions.

      "License" shall mean the terms and conditions for use, reproduction,
      and distribution as defined by Sections 1 through 9 of this document.

      "Licensor" shall mean the copyright owner or entity authorized by
      the copyright owner that is granting the License.

      "Legal Entity" shall mean the union of the acting entity and all
      other entities that control, are controlled by, or are under common
      control with that entity. For the purposes of this definition,
      "control" means (i) the power, direct or indirect, to cause the
      direction or management of such entity, whether by contract or
      otherwise, or (ii) ownership of fifty percent (50%) or more of the
      outstanding shares, or (iii) beneficial ownership of such entity.

      "You" (or "Your") shall mean an individual or Legal Entity
      exercising permissions granted by this License.

      "Source" form shall mean the preferred form for making modifications,
      including but not limited to software source code, documentation
      source, and configuration files.

      "Object" form shall mean any form resulting from mechanical
      transformation or translation of a Source form, including but
      not limited to compiled object code, generated documentation,
      and conversions to other media types.

      "Work" shall mean the work of authorship, whether in Source or
      Object form, made available under the License, as indicated by a
      copyright notice that is included in or attached to the work
      (an example is provided in the Appendix below).

      "Derivative Works" shall mean any work, whether in Source or Object
      form, that is based on (or derived from) the Work and for which the
      editorial revisions, annotations, elaborations, or other modifications
      represent, as a whole, an original work of authorship. For the purposes
      of this License, Derivative Works shall not include works that remain
      separable from, or merely link (or bind by name) to the interfaces of,
      the Work and Derivative Works thereof.

      "Contribution" shall mean any work of authorship, including
      the original version of the Work and any modifications or additions
      to that Work or Derivative Works thereof, that is intentionally
      submitted to Licensor for inclusion in the Work by the copyright owner
      or by an individual or Legal Entity authorized to submit on behalf of
      the copyright owner. For the purposes of this definition, "submitted"
      means any form of electronic, verbal, or written communication sent
      to the Licensor or its representatives, including but not limited to
      communication on electronic mailing lists, source code control systems,
      and issue tracking systems that are managed by, or on behalf of, the
      Licensor for the purpose of discussing and improving the Work, but
      excluding communication that is conspicuously marked or otherwise
      designated in writing by the copyright owner as "Not a Contribution."

      "Contributor" shall mean Licensor and any individual or Legal Entity
      on behalf of whom a Contribution has been received by Licensor and
      subsequently incorporated within the Work.

   2. Grant of Copyright License. Subject to the terms and conditions of
      this License, each Contributor hereby grants to You a perpetual,
      worldwide, non-exclusive, no-charge, royalty-free, irrevocable
      copyright license to reproduce, prepare Derivative Works of,
      publicly display, publicly perform, sublicense, and distribute the
      Work and such Derivative Works in Source or Object form.

   3. Grant of Patent License. Subject to the terms and conditions of
      this License, each Contributor hereby grants to You a perpetual,
      worldwide, non-exclusive, no-charge, royalty-free, irrevocable
      (except as stated in this section) patent license to make, have made,
      use, offer to sell, sell, import, and otherwise transfer the Work,
      where such license applies only to those patent claims licensable
      by such Contributor that are necessarily infringed by their
      Contribution(s) alone or by combination of their Contribution(s)
      with the Work to which such Contribution(s) was submitted. If You
      institute patent litigation against any entity (including a
      cross-claim or counterclaim in a lawsuit) alleging that the Work
      or a Contribution incorporated within the Work constitutes direct
      or contributory patent infringement, then any patent licenses
      granted to You under this License for that Work shall terminate
      as of the date such litigation is filed.

   4. Redistribution. You may reproduce and distribute copies of the
      Work or Derivative Works thereof in any medium, with or without
      modifications, and in Source or Object form, provided that You
      meet the following conditions:

      (a) You must give any other recipients of the Work or
          Derivative Works a copy of this License; and

      (b) You must cause any modified files to carry prominent notices
          stating that You changed the files; and

      (c) You must retain, in the Source form of any Derivative Works
          that You distribute, all copyright, patent, trademark, and
          attribution notices from the Source form of the Work,
          excluding those notices that do not pertain to any part of
          the Derivative Works; and

      (d) If the Work includes a "NOTICE" text file as part of its
          distribution, then any Derivative Works that You distribute must
          include a readable copy of the attribution notices contained
          within such NOTICE file, excluding those notices that do not
          pertain to any part of the Derivative Works, in at least one
          of the following places: within a NOTICE text file distributed
          as part of the Derivative Works; within the Source form or
          documentation, if provided along with the Derivative Works; or,
          within a display generated by the Derivative Works, if and
          wherever such third-party notices normally appear. The contents
          of the NOTICE file are for informational purposes only and
          do not modify the License. You may add Your own attribution
          notices within Derivative Works that You distribute, alongside
          or as an addendum to the NOTICE text from the Work, provided
          that such additional attribution notices cannot be construed
          as modifying the License.

      You may add Your own copyright statement to Your modifications and
      may provide additional or different license terms and conditions
      for use, reproduction, or distribution of Your modifications, or
      for any such Derivative Works as a whole, provided Your use,
      reproduction, and distribution of the Work otherwise complies with
      the conditions stated in this License.

   5. Submission of Contributions. Unless You explicitly state otherwise,
      any Contribution intentionally submitted for inclusion in the Work
      by You to the Licensor shall be under the terms and conditions of
      this License, without any additional terms or conditions.
      Notwithstanding the above, nothing herein shall supersede or modify
      the terms of any separate license agreement you may have executed
      with Licensor regarding such Contributions.

   6. Trademarks. This License does not grant permission to use the trade
      names, trademarks, service marks, or product names of the Licensor,
      except as required for reasonable and customary use in describing the
      origin of the Work and reproducing the content of the NOTICE file.

   7. Disclaimer of Warranty. Unless required by applicable law or
      agreed to in writing, Licensor provides the Work (and each
      Contributor provides its Contributions) on an "AS IS" BASIS,
      WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or
      implied, including, without limitation, any warranties or conditions
      of TITLE, NON-INFRINGEMENT, MERCHANTABILITY, or FITNESS FOR A
      PARTICULAR PURPOSE. You are solely responsible for determining the
      appropriateness of using or redistributing the Work and assume any
      risks associated with Your exercise of permissions under this License.

   8. Limitation of Liability. In no event and under no legal theory,
      whether in tort (including negligence), contract, or otherwise,
      unless required by applicable law (such as deliberate and grossly
      negligent acts) or agreed to in writing, shall any Contributor be
      liable to You for damages, including any direct, indirect, special,
      incidental, or consequential damages of any character arising as a
      result of this License or out of the use or inability to use the
      Work (including but not limited to damages for loss of goodwill,
      work stoppage, computer failure or malfunction, or any and all
      other commercial damages or losses), even if such Contributor
      has been advised of the possibility of such damages.

   9. Accepting Warranty or Additional Liability. While redistributing
      the Work or Derivative Works thereof, You may choose to offer,
      and charge a fee for, acceptance of support, warranty, indemnity,
      or other liability obligations and/or rights consistent with this
      License. However, in accepting such obligations, You may act only
      on Your own behalf and on Your sole responsibility, not on behalf
      of any other Contributor, and only if You agree to indemnify,
      defend, and hold each Contributor harmless for any liability
      incurred by, or claims asserted against, such Contributor by reason
      of your accepting any such warranty or additional liability.

   END OF TERMS AND CONDITIONS

   APPENDIX: How to apply the Apache License to your work.

      To apply the Apache License to your work, attach the following
      boilerplate notice, with the fields enclosed by brackets "[]"
      replaced with your own identifying information. (Don't include
      the brackets!)  The text should be enclosed in the appropriate
      comment syntax for the file format. We also recommend that a
      file or class name and description of purpose be included on the
      same "printed page" as the copyright notice for easier
      identification within third-party archives.

   Copyright [2019] [Rolando Gopez Lacuata]

   Licensed under the Apache License, Version 2.0 (the "License");
   you may not use this file except in compliance with the License.
   You may obtain a copy of the License at

       https://www.apache.org/licenses/LICENSE-2.0

   Unless required by applicable law or agreed to in writing, software
   distributed under the License is distributed on an "AS IS" BASIS,
   WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
   See the License for the specific language governing permissions and
   limitations under the License.

