GYP can Generate Your Projects.
===================================

This fork of the [upstream GIT repo](https://chromium.googlesource.com/external/gyp)
at Google has 4 additional features:

1. The xcode generator recognizes the full set of build variables
that correspond to the set of destination names for the copy phase,
as appears in the Xcode UI. Do
 ```
 git diff upstream_master xcode_changes
 ```
 to see the list. See branch `xcode_changes`.
 
    After these changes, the Destination option menu in the Xcode UI for Copy
    phases will show the correct destination for a destination specified with
    any of the Xcode destination environment variables. Previously the
    option menu would always show "Products Directory" and the only supported
    environment variable was `UNLOCALIZED_RESOURCES_FOLDER_PATH`. Any other
    destination would have to be specified using an explicit path name. This
    is difficult because the paths differ according to platform (iOS, OS X)
    and bundle type. See [this .gyp file](test/mac/copies-with-xcode-envvars/copies-with-xcode-envvars.gyp)
    from the new test for the complete list and to see usage.

    All GYP tests for Xcode, make & ninja formats pass on a Mac with these
    changes.

2. The msvs generator supports Emscripten's [vs-tool](https://github.com/juj/vs-tool/)
plug-in for Visual Studio 2010. That is, it recognizes the properties
`vs-tool` offers to control the Emscripten compiler and linker. These
properties can be included in an `msvs_settings` dictionary. Note this
feature has only been tested with VS 2010. See branch `vs-tool_support`.

    All GYP msvs tests are expected to still pass. That will be tested soon.
    
3. The make generator "sourceifys" source paths in `copies` that contain
environment variables, e.g $(BUILDTYPE). Standard `gyp` only "sourceifys"
paths without environment variables. The make generator also "sourceifys"
paths specified in `library_dirs`. See branch `make_changes_copies`.

    All gyp make, ninja and cmake tests pass on Linux with these changes.

4. Various changes to the `make` generator to better support generating
projects to build Android native applications. These include additions
to the `make_global_settings` dictionary such as being able to create
 _simply expanded variables_ and to specify a `TARGET_ABI`. The latter
 becomes a component of `builddir` in the generated make files. They
 also in include changes to the handling of path relativization in
 various cases. See branch `make_changes_all`.
 
    These changes are a work in progress and are known to break several
    of the GYP `make` generator tests.

Branch `master` contains 1, 2 & 3 as of this writing.

No. 1 has been submitted to Google complete with tests and is
awaiting review.

No. 2 will not be submitted upstream as there are no plans for
`vs-tool` to support anything more modern than Visual Studio 2010.

No. 3 may be submitted in future.

No. 4 is not yet complete.

GYP is a Python application requiring Python 2.7.

To install, clone the repo to your machine and run the following
commands in a shell:

```bash
cd <your_gyp_clone>; sudo ./setup.py install
```

On Windows, use either a Git Bash or Cygwin Bash shell and omit the
`sudo`. Alternatively enter the following commands at a Windows Command
Prompt (`cmd.exe`):

```cmd
cd <your_gyp_clone>
python setup.py install
```

Documents are available at [gyp.gsrc.io](https://gyp.gsrc.io), or you
can check out the ```md-pages``` branch to read those documents offline.

### Installing Python 2.7

Visit the [Python Downloads](https://www.python.org/downloads/) page
to learn how to install Python for your OS.

On OS X you can use the Apple provided Python 2.7 which is in `/usr/bin`
or you can install a slightly more recent version using the [python.org](www.python.org)
installer. If using the latter, and you want to run the GYP tests or generate Ninja
format projects on a Mac, you must install [PyObjC](https://pythonhosted.org/pyobjc/),
which is included in the Apple-provided Python. The easiest way is

```bash
pip install -u pyobjc
```

On Windows you can use either the [native Windows version](https://www.python.org/downloads/windows/)
(recommended) or the version found in [Cygwin](https://www.cygwin.com).

If you install the native Windows version you must add `<python>` and
`<python>/Scripts` to the PATH environment in Windows. `<python>` represents
your actual install directory which defaults to `C:\Python27`.
