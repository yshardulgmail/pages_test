---
layout: default
title: Local Build of Tools 
parent: Build
grand_parent: Developer Information
---

# Local Builds of HostOS Tools 

* The HostOS tools, payloads, and released bundles are setup to be built using the IBM Managed Travis build infrastructure
* However, with just a few *normal*  pre\-requsities, i.e. python3 and docker installed, one can build all of the HostOS artifacts locally.
* This page servers as a HowTo guide for building tools locally.  It will start with a few trail and error examples, then morph into a better organized howTo guide.

## Local Build HowTo

* TBD

## Initial Trial and Error Examples

## Al's attempt on Linuxvm

* For a long while I've been using a mac laptop based linux mint vm for my devshell and cloudnet::cbuild devshell builds.
* I will attempt to install the few missing pieces and build from there.


```
> lsb_release -a  
No LSB modules are available.  
Distributor ID: LinuxMint  
Description: Linux Mint 18 Sarah  
Release: 18  
Codename: sarah
```
#### common\-tools, Attempt 1, vm as is

* Failed with setup command attempting to run unittests



**No Pre\-reqs explicitly installed** Expand source



```
> make
rm -f python/AUTHORS python/ChangeLog
rm -rf python/.eggs python/*.egg-info
rm -rf output python/build python/output
[[ -d /home/aross/hostos/common-tools//python/venv ]] || (mkdir -p /home/aross/hostos/common-tools//python/venv; python3 -m venv /home/aross/hostos/common-tools//python/venv)
/home/aross/hostos/common-tools//python/venv/bin/pip3 install --upgrade pip
Collecting pip
  Downloading https://files.pythonhosted.org/packages/5c/e0/be401c003291b56efc55aeba6a80ab790d3d4cece2778288d65323009420/pip-19.1.1-py2.py3-none-any.whl (1.4MB)
    100% |████████████████████████████████| 1.4MB 727kB/s
Installing collected packages: pip
  Found existing installation: pip 8.1.1
    Uninstalling pip-8.1.1:
      Successfully uninstalled pip-8.1.1
Successfully installed pip-19.1.1
/home/aross/hostos/common-tools//python/venv/bin/pip3 install pycodestyle flake8 pylint
Collecting pycodestyle
  Downloading https://files.pythonhosted.org/packages/0e/0c/04a353e104d2f324f8ee5f4b32012618c1c86dd79e52a433b64fceed511b/pycodestyle-2.5.0-py2.py3-none-any.whl (51kB)
     |████████████████████████████████| 51kB 634kB/s
Collecting flake8
  Downloading https://files.pythonhosted.org/packages/e9/76/b915bd28976068a9843bf836b789794aa4a8eb13338b23581005cd9177c0/flake8-3.7.7-py2.py3-none-any.whl (68kB)
     |████████████████████████████████| 71kB 1.0MB/s
Collecting pylint
  Downloading https://files.pythonhosted.org/packages/60/c2/b3f73f4ac008bef6e75bca4992f3963b3f85942e0277237721ef1c151f0d/pylint-2.3.1-py3-none-any.whl (765kB)
     |████████████████████████████████| 768kB 2.6MB/s
Collecting pyflakes<2.2.0,>=2.1.0 (from flake8)
  Downloading https://files.pythonhosted.org/packages/84/f2/ed0ffb887f8138a8fe5a621b8c0bb9598bfb3989e029f6c6a85ee66628ee/pyflakes-2.1.1-py2.py3-none-any.whl (59kB)
     |████████████████████████████████| 61kB 1.6MB/s
Collecting mccabe<0.7.0,>=0.6.0 (from flake8)
  Downloading https://files.pythonhosted.org/packages/87/89/479dc97e18549e21354893e4ee4ef36db1d237534982482c3681ee6e7b57/mccabe-0.6.1-py2.py3-none-any.whl
Collecting entrypoints<0.4.0,>=0.3.0 (from flake8)
  Downloading https://files.pythonhosted.org/packages/ac/c6/44694103f8c221443ee6b0041f69e2740d89a25641e62fb4f2ee568f2f9c/entrypoints-0.3-py2.py3-none-any.whl
Collecting isort<5,>=4.2.5 (from pylint)
  Downloading https://files.pythonhosted.org/packages/1c/d9/bf5848b376e441ff358a14b954476423eeeb8c9b78c10074b7f53ce2918d/isort-4.3.20-py2.py3-none-any.whl (42kB)
     |████████████████████████████████| 51kB 3.1MB/s
Collecting astroid<3,>=2.2.0 (from pylint)
  Downloading https://files.pythonhosted.org/packages/d5/ad/7221a62a2dbce5c3b8c57fd18e1052c7331adc19b3f27f1561aa6e620db2/astroid-2.2.5-py3-none-any.whl (193kB)
     |████████████████████████████████| 194kB 1.9MB/s
Collecting wrapt (from astroid<3,>=2.2.0->pylint)
  Downloading https://files.pythonhosted.org/packages/67/b2/0f71ca90b0ade7fad27e3d20327c996c6252a2ffe88f50a95bba7434eda9/wrapt-1.11.1.tar.gz
Collecting lazy-object-proxy (from astroid<3,>=2.2.0->pylint)
  Downloading https://files.pythonhosted.org/packages/1f/f4/65eb90d5c1d22861d80faac4af74e3adc41cd63c8aea198e21a00372f875/lazy_object_proxy-1.4.1-cp35-cp35m-manylinux1_x86_64.whl (49kB)
     |████████████████████████████████| 51kB 3.4MB/s
Collecting six (from astroid<3,>=2.2.0->pylint)
  Downloading https://files.pythonhosted.org/packages/73/fb/00a976f728d0d1fecfe898238ce23f502a721c0ac0ecfedb80e0d88c64e9/six-1.12.0-py2.py3-none-any.whl
Collecting typed-ast>=1.3.0; implementation_name == "cpython" (from astroid<3,>=2.2.0->pylint)
  Downloading https://files.pythonhosted.org/packages/b0/9a/7f6c8c690bf4bea7b2d4fe3cb278be20902a2d363fc95d81a1ac6b8a4e98/typed_ast-1.3.5-cp35-cp35m-manylinux1_x86_64.whl (735kB)
     |████████████████████████████████| 737kB 1.9MB/s
Installing collected packages: pycodestyle, pyflakes, mccabe, entrypoints, flake8, isort, wrapt, lazy-object-proxy, six, typed-ast, astroid, pylint
  Running setup.py install for wrapt ... done
Successfully installed astroid-2.2.5 entrypoints-0.3 flake8-3.7.7 isort-4.3.20 lazy-object-proxy-1.4.1 mccabe-0.6.1 pycodestyle-2.5.0 pyflakes-2.1.1 pylint-2.3.1 six-1.12.0 typed-ast-1.3.5 wrapt-1.11.1
[[ /home/aross/hostos/common-tools/ == /home/aross/hostos/common-tools/ ]] || /home/aross/hostos/common-tools//python/venv/bin/pip3 install -r /home/aross/hostos/common-tools//python/requirements.txt
[[ /home/aross/hostos/common-tools/ == /home/aross/hostos/common-tools/ ]] || /home/aross/hostos/common-tools//python/venv/bin/pip3 install -e /home/aross/hostos/common-tools//python
/home/aross/hostos/common-tools//python/venv/bin/pip3 install -r /home/aross/hostos/common-tools//python/requirements.txt
Collecting PyYAML (from -r /home/aross/hostos/common-tools//python/requirements.txt (line 1))
  WARNING: Retrying (Retry(total=4, connect=None, read=None, redirect=None, status=None)) after connection broken by 'ReadTimeoutError("HTTPSConnectionPool(host='files.pythonhosted.org', port=443): Read timed out. (read timeout=15)",)': /packages/9f/2c/9417b5c774792634834e730932745bc09a7d36754ca00acf1ccd1ac2594d/PyYAML-5.1.tar.gz
  Downloading https://files.pythonhosted.org/packages/9f/2c/9417b5c774792634834e730932745bc09a7d36754ca00acf1ccd1ac2594d/PyYAML-5.1.tar.gz (274kB)
     |████████████████████████████████| 276kB 1.1MB/s
Collecting jsonschema (from -r /home/aross/hostos/common-tools//python/requirements.txt (line 2))
  Downloading https://files.pythonhosted.org/packages/aa/69/df679dfbdd051568b53c38ec8152a3ab6bc533434fc7ed11ab034bf5e82f/jsonschema-3.0.1-py2.py3-none-any.whl (54kB)
     |████████████████████████████████| 61kB 901kB/s
Requirement already satisfied: six>=1.11.0 in ./python/venv/lib/python3.5/site-packages (from jsonschema->-r /home/aross/hostos/common-tools//python/requirements.txt (line 2)) (1.12.0)
Collecting attrs>=17.4.0 (from jsonschema->-r /home/aross/hostos/common-tools//python/requirements.txt (line 2))
  Downloading https://files.pythonhosted.org/packages/23/96/d828354fa2dbdf216eaa7b7de0db692f12c234f7ef888cc14980ef40d1d2/attrs-19.1.0-py2.py3-none-any.whl
Requirement already satisfied: setuptools in ./python/venv/lib/python3.5/site-packages (from jsonschema->-r /home/aross/hostos/common-tools//python/requirements.txt (line 2)) (20.7.0)
Collecting pyrsistent>=0.14.0 (from jsonschema->-r /home/aross/hostos/common-tools//python/requirements.txt (line 2))
  Downloading https://files.pythonhosted.org/packages/68/0b/f514e76b4e074386b60cfc6c8c2d75ca615b81e415417ccf3fac80ae0bf6/pyrsistent-0.15.2.tar.gz (106kB)
     |████████████████████████████████| 112kB 265kB/s
Installing collected packages: PyYAML, attrs, pyrsistent, jsonschema
  Running setup.py install for PyYAML ... done
  Running setup.py install for pyrsistent ... done
Successfully installed PyYAML-5.1 attrs-19.1.0 jsonschema-3.0.1 pyrsistent-0.15.2
/home/aross/hostos/common-tools//python/venv/bin/pip3 install -e /home/aross/hostos/common-tools//python
Obtaining file:///home/aross/hostos/common-tools/python


Requirement already satisfied: PyYAML in ./python/venv/lib/python3.5/site-packages (from nextgen-hostos-common==0.0.1.dev19) (5.1)
Requirement already satisfied: jsonschema in ./python/venv/lib/python3.5/site-packages (from nextgen-hostos-common==0.0.1.dev19) (3.0.1)
Requirement already satisfied: pyrsistent>=0.14.0 in ./python/venv/lib/python3.5/site-packages (from jsonschema->nextgen-hostos-common==0.0.1.dev19) (0.15.2)
Requirement already satisfied: setuptools in ./python/venv/lib/python3.5/site-packages (from jsonschema->nextgen-hostos-common==0.0.1.dev19) (20.7.0)
Requirement already satisfied: attrs>=17.4.0 in ./python/venv/lib/python3.5/site-packages (from jsonschema->nextgen-hostos-common==0.0.1.dev19) (19.1.0)
Requirement already satisfied: six>=1.11.0 in ./python/venv/lib/python3.5/site-packages (from jsonschema->nextgen-hostos-common==0.0.1.dev19) (1.12.0)
Installing collected packages: nextgen-hostos-common
  Running setup.py develop for nextgen-hostos-common
Successfully installed nextgen-hostos-common
Executing PEP8 on python files...
(source /home/aross/hostos/common-tools//python/venv/bin/activate; export PATH=/home/aross/hostos/common-tools//python/venv/bin; pycodestyle python/hostos_*)
Executing Flake8 on python files...
(source /home/aross/hostos/common-tools//python/venv/bin/activate; export PATH=/home/aross/hostos/common-tools//python/venv/bin; flake8 --jobs=auto --ignore=E501 --statistics --count python/hostos_*)
0
Executing PyLint on python files...
(source /home/aross/hostos/common-tools//python/venv/bin/activate; export PATH=/home/aross/hostos/common-tools//python/venv/bin; pylint --jobs=4 --rcfile=/home/aross/hostos/common-tools//python/.pylintrc python/hostos_*)

------------------------------------
Your code has been rated at 10.00/10

Executing unit tests...
Building Python code...
(cd python; python3 setup.py install --prefix /usr --install-lib=usr/lib/python3.6/dist-packages --root ./output)
Traceback (most recent call last):
  File "setup.py", line 13, in <module>
    from setuptools import setup
ImportError: No module named 'setuptools'
Makefile.common:34: recipe for target 'build' failed
make: *** [build] Error 1

[aross@aross-mint-vm]/home/aross/hostos/common-tools/
```


#### common\-tools, attempt 2, after installing a few python tools

* I installed 2 or 3 related python tools.  This was not very controlled, but it did the trick.
* The example below appears to be an incremental build from where it left off above.



**common\-tools** Expand source



```
> make
rm -f python/AUTHORS python/ChangeLog
rm -rf python/.eggs python/*.egg-info
rm -rf output python/build python/output
Executing PEP8 on python files...
(source /home/aross/hostos/common-tools//python/venv/bin/activate; export PATH=/home/aross/hostos/common-tools//python/venv/bin; pycodestyle python/hostos_*)
Executing Flake8 on python files...
(source /home/aross/hostos/common-tools//python/venv/bin/activate; export PATH=/home/aross/hostos/common-tools//python/venv/bin; flake8 --jobs=auto --ignore=E501 --statistics --count python/hostos_*)
0
Executing PyLint on python files...
(source /home/aross/hostos/common-tools//python/venv/bin/activate; export PATH=/home/aross/hostos/common-tools//python/venv/bin; pylint --jobs=4 --rcfile=/home/aross/hostos/common-tools//python/.pylintrc python/hostos_*)

--------------------------------------------------------------------
Your code has been rated at 10.00/10 (previous run: 10.00/10, +0.00)

Executing unit tests...
Building Python code...
(cd python; python3 setup.py install --prefix /usr --install-lib=usr/lib/python3.6/dist-packages --root ./output)
/usr/lib/python3.5/distutils/dist.py:261: UserWarning: Unknown distribution option: 'project_urls'
  warnings.warn(msg)
/usr/lib/python3.5/distutils/dist.py:261: UserWarning: Unknown distribution option: 'long_description_content_type'
  warnings.warn(msg)

Installed /home/aross/hostos/common-tools/python/.eggs/pbr-5.2.0-py3.5.egg
[pbr] Generating ChangeLog
running install
[pbr] Writing ChangeLog
[pbr] Generating ChangeLog
[pbr] ChangeLog complete (0.0s)
[pbr] Generating AUTHORS
[pbr] AUTHORS complete (0.0s)
running build
running build_py
creating build
creating build/lib
creating build/lib/hostos_common
copying hostos_common/task_executor.py -> build/lib/hostos_common
copying hostos_common/__init__.py -> build/lib/hostos_common
copying hostos_common/release_manifest.py -> build/lib/hostos_common
copying hostos_common/secrets_vault.py -> build/lib/hostos_common
copying hostos_common/command_setup.py -> build/lib/hostos_common
copying hostos_common/ssh_utils.py -> build/lib/hostos_common
copying hostos_common/node_inventory.py -> build/lib/hostos_common
running egg_info
creating nextgen_hostos_common.egg-info
writing pbr to nextgen_hostos_common.egg-info/pbr.json
writing top-level names to nextgen_hostos_common.egg-info/top_level.txt
writing dependency_links to nextgen_hostos_common.egg-info/dependency_links.txt
writing nextgen_hostos_common.egg-info/PKG-INFO
writing requirements to nextgen_hostos_common.egg-info/requires.txt
[pbr] Processing SOURCES.txt
writing manifest file 'nextgen_hostos_common.egg-info/SOURCES.txt'
[pbr] In git context, generating filelist from git
warning: no previously-included files found matching '.gitignore'
warning: no previously-included files found matching '.gitreview'
warning: no previously-included files matching '*.pyc' found anywhere in distribution
writing manifest file 'nextgen_hostos_common.egg-info/SOURCES.txt'
copying hostos_common/release-manifest-schema.yml -> build/lib/hostos_common
copying hostos_common/payload-manifest-schema.yml -> build/lib/hostos_common
copying hostos_common/zone-inventory-schema.yml -> build/lib/hostos_common
running install_lib
creating output
creating output/usr
creating output/usr/lib
creating output/usr/lib/python3.6
creating output/usr/lib/python3.6/dist-packages
creating output/usr/lib/python3.6/dist-packages/hostos_common
copying build/lib/hostos_common/task_executor.py -> ./output/usr/lib/python3.6/dist-packages/hostos_common
copying build/lib/hostos_common/release-manifest-schema.yml -> ./output/usr/lib/python3.6/dist-packages/hostos_common
copying build/lib/hostos_common/__init__.py -> ./output/usr/lib/python3.6/dist-packages/hostos_common
copying build/lib/hostos_common/release_manifest.py -> ./output/usr/lib/python3.6/dist-packages/hostos_common
copying build/lib/hostos_common/secrets_vault.py -> ./output/usr/lib/python3.6/dist-packages/hostos_common
copying build/lib/hostos_common/payload-manifest-schema.yml -> ./output/usr/lib/python3.6/dist-packages/hostos_common
copying build/lib/hostos_common/command_setup.py -> ./output/usr/lib/python3.6/dist-packages/hostos_common
copying build/lib/hostos_common/ssh_utils.py -> ./output/usr/lib/python3.6/dist-packages/hostos_common
copying build/lib/hostos_common/zone-inventory-schema.yml -> ./output/usr/lib/python3.6/dist-packages/hostos_common
copying build/lib/hostos_common/node_inventory.py -> ./output/usr/lib/python3.6/dist-packages/hostos_common
byte-compiling ./output/usr/lib/python3.6/dist-packages/hostos_common/task_executor.py to task_executor.cpython-35.pyc
byte-compiling ./output/usr/lib/python3.6/dist-packages/hostos_common/__init__.py to __init__.cpython-35.pyc
byte-compiling ./output/usr/lib/python3.6/dist-packages/hostos_common/release_manifest.py to release_manifest.cpython-35.pyc
byte-compiling ./output/usr/lib/python3.6/dist-packages/hostos_common/secrets_vault.py to secrets_vault.cpython-35.pyc
byte-compiling ./output/usr/lib/python3.6/dist-packages/hostos_common/command_setup.py to command_setup.cpython-35.pyc
byte-compiling ./output/usr/lib/python3.6/dist-packages/hostos_common/ssh_utils.py to ssh_utils.cpython-35.pyc
byte-compiling ./output/usr/lib/python3.6/dist-packages/hostos_common/node_inventory.py to node_inventory.cpython-35.pyc
running install_egg_info
Copying nextgen_hostos_common.egg-info to ./output/usr/lib/python3.6/dist-packages/nextgen_hostos_common-0.0.1.dev19-py3.5.egg-info
running install_scripts
Building Debian packages...
(mkdir output; mkdir python/output/DEBIAN; cp debian/control python/output/DEBIAN/)
sed -i -e 's|%%version%%|0.0.1-8b847e1|' python/output/DEBIAN/control
dpkg-deb --build python/output output/nextgen-hostos-common_0.0.1-8b847e1_all.deb
dpkg-deb: building package 'nextgen-hostos-common' in 'output/nextgen-hostos-common_0.0.1-8b847e1_all.deb'.


```


  


#### upgrade tools, attempt 1

* first attempt to build upgrade\-tools repo

**upgrade\-tools, attempt 1**

```
> make
rm -f python/AUTHORS python/ChangeLog
rm -rf python/.eggs python/*.egg-info
rm -rf output python/build python/output
Executing PEP8 on python files...
(source /home/aross/hostos/upgrade-tools//python/venv/bin/activate; export PATH=/home/aross/hostos/upgrade-tools//python/venv/bin; pycodestyle python/hostos_*)
/bin/bash: /home/aross/hostos/upgrade-tools//python/venv/bin/activate: No such file or directory
/bin/bash: pycodestyle: command not found
hostos-common-tools/Makefile.common:57: recipe for target 'pylint' failed
make: *** [pylint] Error 127
```

#### upgrade tools, attempt 2

* The only change in this attempt is that I ran 'make clean' first



**upgrade\-tools** Expand source



```
[aross@aross-mint-vm]/home/aross/hostos/upgrade-tools/
> make clean && make
rm -f python/AUTHORS python/ChangeLog
rm -rf python/.eggs python/*.egg-info
rm -rf output python/build python/output
rm -rf /home/aross/hostos/upgrade-tools//python/venv
rm -f python/AUTHORS python/ChangeLog
rm -rf python/.eggs python/*.egg-info
rm -rf output python/build python/output
[[ -d /home/aross/hostos/upgrade-tools//python/venv ]] || (mkdir -p /home/aross/hostos/upgrade-tools//python/venv; python3 -m venv /home/aross/hostos/upgrade-tools//python/venv)
/home/aross/hostos/upgrade-tools//python/venv/bin/pip3 install --upgrade pip
Collecting pip
  Using cached https://files.pythonhosted.org/packages/5c/e0/be401c003291b56efc55aeba6a80ab790d3d4cece2778288d65323009420/pip-19.1.1-py2.py3-none-any.whl
Installing collected packages: pip
  Found existing installation: pip 8.1.1
    Uninstalling pip-8.1.1:
      Successfully uninstalled pip-8.1.1
Successfully installed pip-19.1.1
/home/aross/hostos/upgrade-tools//python/venv/bin/pip3 install pycodestyle flake8 pylint
Collecting pycodestyle
  Using cached https://files.pythonhosted.org/packages/0e/0c/04a353e104d2f324f8ee5f4b32012618c1c86dd79e52a433b64fceed511b/pycodestyle-2.5.0-py2.py3-none-any.whl
Collecting flake8
  Using cached https://files.pythonhosted.org/packages/e9/76/b915bd28976068a9843bf836b789794aa4a8eb13338b23581005cd9177c0/flake8-3.7.7-py2.py3-none-any.whl
Collecting pylint
  Using cached https://files.pythonhosted.org/packages/60/c2/b3f73f4ac008bef6e75bca4992f3963b3f85942e0277237721ef1c151f0d/pylint-2.3.1-py3-none-any.whl
Collecting pyflakes<2.2.0,>=2.1.0 (from flake8)
  Using cached https://files.pythonhosted.org/packages/84/f2/ed0ffb887f8138a8fe5a621b8c0bb9598bfb3989e029f6c6a85ee66628ee/pyflakes-2.1.1-py2.py3-none-any.whl
Collecting entrypoints<0.4.0,>=0.3.0 (from flake8)
  Using cached https://files.pythonhosted.org/packages/ac/c6/44694103f8c221443ee6b0041f69e2740d89a25641e62fb4f2ee568f2f9c/entrypoints-0.3-py2.py3-none-any.whl
Collecting mccabe<0.7.0,>=0.6.0 (from flake8)
  Using cached https://files.pythonhosted.org/packages/87/89/479dc97e18549e21354893e4ee4ef36db1d237534982482c3681ee6e7b57/mccabe-0.6.1-py2.py3-none-any.whl
Collecting isort<5,>=4.2.5 (from pylint)
  Using cached https://files.pythonhosted.org/packages/1c/d9/bf5848b376e441ff358a14b954476423eeeb8c9b78c10074b7f53ce2918d/isort-4.3.20-py2.py3-none-any.whl
Collecting astroid<3,>=2.2.0 (from pylint)
  Using cached https://files.pythonhosted.org/packages/d5/ad/7221a62a2dbce5c3b8c57fd18e1052c7331adc19b3f27f1561aa6e620db2/astroid-2.2.5-py3-none-any.whl
Collecting wrapt (from astroid<3,>=2.2.0->pylint)
  Using cached https://files.pythonhosted.org/packages/67/b2/0f71ca90b0ade7fad27e3d20327c996c6252a2ffe88f50a95bba7434eda9/wrapt-1.11.1.tar.gz
Collecting lazy-object-proxy (from astroid<3,>=2.2.0->pylint)
  Using cached https://files.pythonhosted.org/packages/1f/f4/65eb90d5c1d22861d80faac4af74e3adc41cd63c8aea198e21a00372f875/lazy_object_proxy-1.4.1-cp35-cp35m-manylinux1_x86_64.whl
Collecting typed-ast>=1.3.0; implementation_name == "cpython" (from astroid<3,>=2.2.0->pylint)
  Using cached https://files.pythonhosted.org/packages/b0/9a/7f6c8c690bf4bea7b2d4fe3cb278be20902a2d363fc95d81a1ac6b8a4e98/typed_ast-1.3.5-cp35-cp35m-manylinux1_x86_64.whl
Collecting six (from astroid<3,>=2.2.0->pylint)
  Using cached https://files.pythonhosted.org/packages/73/fb/00a976f728d0d1fecfe898238ce23f502a721c0ac0ecfedb80e0d88c64e9/six-1.12.0-py2.py3-none-any.whl
Installing collected packages: pycodestyle, pyflakes, entrypoints, mccabe, flake8, isort, wrapt, lazy-object-proxy, typed-ast, six, astroid, pylint
  Running setup.py install for wrapt ... done
Successfully installed astroid-2.2.5 entrypoints-0.3 flake8-3.7.7 isort-4.3.20 lazy-object-proxy-1.4.1 mccabe-0.6.1 pycodestyle-2.5.0 pyflakes-2.1.1 pylint-2.3.1 six-1.12.0 typed-ast-1.3.5 wrapt-1.11.1
[[ /home/aross/hostos/upgrade-tools/ == /home/aross/hostos/upgrade-tools/hostos-common-tools/ ]] || /home/aross/hostos/upgrade-tools//python/venv/bin/pip3 install -r /home/aross/hostos/upgrade-tools/hostos-common-tools//python/requirements.txt
Collecting PyYAML (from -r /home/aross/hostos/upgrade-tools/hostos-common-tools//python/requirements.txt (line 1))
  Using cached https://files.pythonhosted.org/packages/9f/2c/9417b5c774792634834e730932745bc09a7d36754ca00acf1ccd1ac2594d/PyYAML-5.1.tar.gz
Collecting jsonschema (from -r /home/aross/hostos/upgrade-tools/hostos-common-tools//python/requirements.txt (line 2))
  Using cached https://files.pythonhosted.org/packages/aa/69/df679dfbdd051568b53c38ec8152a3ab6bc533434fc7ed11ab034bf5e82f/jsonschema-3.0.1-py2.py3-none-any.whl
Requirement already satisfied: six>=1.11.0 in ./python/venv/lib/python3.5/site-packages (from jsonschema->-r /home/aross/hostos/upgrade-tools/hostos-common-tools//python/requirements.txt (line 2)) (1.12.0)
Collecting attrs>=17.4.0 (from jsonschema->-r /home/aross/hostos/upgrade-tools/hostos-common-tools//python/requirements.txt (line 2))
  Using cached https://files.pythonhosted.org/packages/23/96/d828354fa2dbdf216eaa7b7de0db692f12c234f7ef888cc14980ef40d1d2/attrs-19.1.0-py2.py3-none-any.whl
Collecting pyrsistent>=0.14.0 (from jsonschema->-r /home/aross/hostos/upgrade-tools/hostos-common-tools//python/requirements.txt (line 2))
  Using cached https://files.pythonhosted.org/packages/68/0b/f514e76b4e074386b60cfc6c8c2d75ca615b81e415417ccf3fac80ae0bf6/pyrsistent-0.15.2.tar.gz
Requirement already satisfied: setuptools in ./python/venv/lib/python3.5/site-packages (from jsonschema->-r /home/aross/hostos/upgrade-tools/hostos-common-tools//python/requirements.txt (line 2)) (20.7.0)
Installing collected packages: PyYAML, attrs, pyrsistent, jsonschema
  Running setup.py install for PyYAML ... done
  Running setup.py install for pyrsistent ... done
Successfully installed PyYAML-5.1 attrs-19.1.0 jsonschema-3.0.1 pyrsistent-0.15.2
[[ /home/aross/hostos/upgrade-tools/ == /home/aross/hostos/upgrade-tools/hostos-common-tools/ ]] || /home/aross/hostos/upgrade-tools//python/venv/bin/pip3 install -e /home/aross/hostos/upgrade-tools/hostos-common-tools//python
Obtaining file:///home/aross/hostos/upgrade-tools/hostos-common-tools/python
Requirement already satisfied: PyYAML in ./python/venv/lib/python3.5/site-packages (from nextgen-hostos-common==0.0.1.dev19) (5.1)
Requirement already satisfied: jsonschema in ./python/venv/lib/python3.5/site-packages (from nextgen-hostos-common==0.0.1.dev19) (3.0.1)
Requirement already satisfied: six>=1.11.0 in ./python/venv/lib/python3.5/site-packages (from jsonschema->nextgen-hostos-common==0.0.1.dev19) (1.12.0)
Requirement already satisfied: pyrsistent>=0.14.0 in ./python/venv/lib/python3.5/site-packages (from jsonschema->nextgen-hostos-common==0.0.1.dev19) (0.15.2)
Requirement already satisfied: attrs>=17.4.0 in ./python/venv/lib/python3.5/site-packages (from jsonschema->nextgen-hostos-common==0.0.1.dev19) (19.1.0)
Requirement already satisfied: setuptools in ./python/venv/lib/python3.5/site-packages (from jsonschema->nextgen-hostos-common==0.0.1.dev19) (20.7.0)
Installing collected packages: nextgen-hostos-common
  Running setup.py develop for nextgen-hostos-common
Successfully installed nextgen-hostos-common
/home/aross/hostos/upgrade-tools//python/venv/bin/pip3 install -r /home/aross/hostos/upgrade-tools//python/requirements.txt
Requirement already satisfied: nextgen-hostos-common in ./hostos-common-tools/python (from -r /home/aross/hostos/upgrade-tools//python/requirements.txt (line 1)) (0.0.1.dev19)
Requirement already satisfied: PyYAML in ./python/venv/lib/python3.5/site-packages (from nextgen-hostos-common->-r /home/aross/hostos/upgrade-tools//python/requirements.txt (line 1)) (5.1)
Requirement already satisfied: jsonschema in ./python/venv/lib/python3.5/site-packages (from nextgen-hostos-common->-r /home/aross/hostos/upgrade-tools//python/requirements.txt (line 1)) (3.0.1)
Requirement already satisfied: attrs>=17.4.0 in ./python/venv/lib/python3.5/site-packages (from jsonschema->nextgen-hostos-common->-r /home/aross/hostos/upgrade-tools//python/requirements.txt (line 1)) (19.1.0)
Requirement already satisfied: pyrsistent>=0.14.0 in ./python/venv/lib/python3.5/site-packages (from jsonschema->nextgen-hostos-common->-r /home/aross/hostos/upgrade-tools//python/requirements.txt (line 1)) (0.15.2)
Requirement already satisfied: six>=1.11.0 in ./python/venv/lib/python3.5/site-packages (from jsonschema->nextgen-hostos-common->-r /home/aross/hostos/upgrade-tools//python/requirements.txt (line 1)) (1.12.0)
Requirement already satisfied: setuptools in ./python/venv/lib/python3.5/site-packages (from jsonschema->nextgen-hostos-common->-r /home/aross/hostos/upgrade-tools//python/requirements.txt (line 1)) (20.7.0)
/home/aross/hostos/upgrade-tools//python/venv/bin/pip3 install -e /home/aross/hostos/upgrade-tools//python
Obtaining file:///home/aross/hostos/upgrade-tools/python
Requirement already satisfied: nextgen-hostos-common in ./hostos-common-tools/python (from nextgen-hostos-upgrade==0.0.1.dev24) (0.0.1.dev19)
Requirement already satisfied: PyYAML in ./python/venv/lib/python3.5/site-packages (from nextgen-hostos-common->nextgen-hostos-upgrade==0.0.1.dev24) (5.1)
Requirement already satisfied: jsonschema in ./python/venv/lib/python3.5/site-packages (from nextgen-hostos-common->nextgen-hostos-upgrade==0.0.1.dev24) (3.0.1)
Requirement already satisfied: setuptools in ./python/venv/lib/python3.5/site-packages (from jsonschema->nextgen-hostos-common->nextgen-hostos-upgrade==0.0.1.dev24) (20.7.0)
Requirement already satisfied: pyrsistent>=0.14.0 in ./python/venv/lib/python3.5/site-packages (from jsonschema->nextgen-hostos-common->nextgen-hostos-upgrade==0.0.1.dev24) (0.15.2)
Requirement already satisfied: attrs>=17.4.0 in ./python/venv/lib/python3.5/site-packages (from jsonschema->nextgen-hostos-common->nextgen-hostos-upgrade==0.0.1.dev24) (19.1.0)
Requirement already satisfied: six>=1.11.0 in ./python/venv/lib/python3.5/site-packages (from jsonschema->nextgen-hostos-common->nextgen-hostos-upgrade==0.0.1.dev24) (1.12.0)
Installing collected packages: nextgen-hostos-upgrade
  Running setup.py develop for nextgen-hostos-upgrade
Successfully installed nextgen-hostos-upgrade
Executing PEP8 on python files...
(source /home/aross/hostos/upgrade-tools//python/venv/bin/activate; export PATH=/home/aross/hostos/upgrade-tools//python/venv/bin; pycodestyle python/hostos_*)
Executing Flake8 on python files...
(source /home/aross/hostos/upgrade-tools//python/venv/bin/activate; export PATH=/home/aross/hostos/upgrade-tools//python/venv/bin; flake8 --jobs=auto --ignore=E501 --statistics --count python/hostos_*)
0
Executing PyLint on python files...
(source /home/aross/hostos/upgrade-tools//python/venv/bin/activate; export PATH=/home/aross/hostos/upgrade-tools//python/venv/bin; pylint --jobs=4 --rcfile=/home/aross/hostos/upgrade-tools/hostos-common-tools//python/.pylintrc python/hostos_*)

------------------------------------
Your code has been rated at 10.00/10

Executing unit tests...
Building Python code...
(cd python; python3 setup.py install --prefix /usr --install-lib=usr/lib/python3.6/dist-packages --root ./output)
running install
[pbr] Writing ChangeLog
[pbr] Generating ChangeLog
[pbr] ChangeLog complete (0.0s)
[pbr] Generating AUTHORS
[pbr] AUTHORS complete (0.0s)
running build
running build_py
creating build
creating build/lib
creating build/lib/hostos_upgrade
copying hostos_upgrade/verify_packages.py -> build/lib/hostos_upgrade
copying hostos_upgrade/upgrade_nodes.py -> build/lib/hostos_upgrade
copying hostos_upgrade/upgrade_cli.py -> build/lib/hostos_upgrade
copying hostos_upgrade/package_installer.py -> build/lib/hostos_upgrade
copying hostos_upgrade/__init__.py -> build/lib/hostos_upgrade
copying hostos_upgrade/verify_disruption.py -> build/lib/hostos_upgrade
running egg_info
writing pbr to nextgen_hostos_upgrade.egg-info/pbr.json
writing dependency_links to nextgen_hostos_upgrade.egg-info/dependency_links.txt
writing entry points to nextgen_hostos_upgrade.egg-info/entry_points.txt
writing requirements to nextgen_hostos_upgrade.egg-info/requires.txt
writing nextgen_hostos_upgrade.egg-info/PKG-INFO
writing top-level names to nextgen_hostos_upgrade.egg-info/top_level.txt
[pbr] Reusing existing SOURCES.txt
running install_lib
creating output
creating output/usr
creating output/usr/lib
creating output/usr/lib/python3.6
creating output/usr/lib/python3.6/dist-packages
creating output/usr/lib/python3.6/dist-packages/hostos_upgrade
copying build/lib/hostos_upgrade/verify_packages.py -> ./output/usr/lib/python3.6/dist-packages/hostos_upgrade
copying build/lib/hostos_upgrade/upgrade_nodes.py -> ./output/usr/lib/python3.6/dist-packages/hostos_upgrade
copying build/lib/hostos_upgrade/upgrade_cli.py -> ./output/usr/lib/python3.6/dist-packages/hostos_upgrade
copying build/lib/hostos_upgrade/package_installer.py -> ./output/usr/lib/python3.6/dist-packages/hostos_upgrade
copying build/lib/hostos_upgrade/__init__.py -> ./output/usr/lib/python3.6/dist-packages/hostos_upgrade
copying build/lib/hostos_upgrade/verify_disruption.py -> ./output/usr/lib/python3.6/dist-packages/hostos_upgrade
byte-compiling ./output/usr/lib/python3.6/dist-packages/hostos_upgrade/verify_packages.py to verify_packages.cpython-35.pyc
byte-compiling ./output/usr/lib/python3.6/dist-packages/hostos_upgrade/upgrade_nodes.py to upgrade_nodes.cpython-35.pyc
byte-compiling ./output/usr/lib/python3.6/dist-packages/hostos_upgrade/upgrade_cli.py to upgrade_cli.cpython-35.pyc
byte-compiling ./output/usr/lib/python3.6/dist-packages/hostos_upgrade/package_installer.py to package_installer.cpython-35.pyc
byte-compiling ./output/usr/lib/python3.6/dist-packages/hostos_upgrade/__init__.py to __init__.cpython-35.pyc
byte-compiling ./output/usr/lib/python3.6/dist-packages/hostos_upgrade/verify_disruption.py to verify_disruption.cpython-35.pyc
running install_egg_info
Copying nextgen_hostos_upgrade.egg-info to ./output/usr/lib/python3.6/dist-packages/nextgen_hostos_upgrade-0.0.1.dev24-py3.5.egg-info
running install_scripts
Installing nextgen-hostos-upgrade script to ./output/usr/bin
Building Debian packages...
(mkdir output; mkdir python/output/DEBIAN; cp debian/control python/output/DEBIAN/)
sed -i -e 's|%%version%%|0.0.1-ec5e1af|' python/output/DEBIAN/control
dpkg-deb --build python/output output/nextgen-hostos-upgrade_0.0.1-ec5e1af_all.deb
dpkg-deb: building package 'nextgen-hostos-upgrade' in 'output/nextgen-hostos-upgrade_0.0.1-ec5e1af_all.deb'.

[aross@aross-mint-vm]/home/aross/hostos/upgrade-tools/
```


  


#### upgrade\-payload, attempt 1

* This build failed due to inability to reach artifactoys
* I had previously requested access to the devops 'sandbox' repositories within artifactory.
* This wasn't quite enough.



 Expand source



```
> make
*** Building payloads for hostos-upgrade ***
rm -rf ./output
mkdir -p ./output ./localrepo
cp -r ./manifests ./output/
docker build --no-cache -f Dockerfile -t payload_creator:latest .
Sending build context to Docker daemon 527.4 kB
Step 1 : FROM wcp-genctl-sandbox-docker-local.artifactory.swg-devops.com/genctl-devtools:latest
Get https://wcp-genctl-sandbox-docker-local.artifactory.swg-devops.com/v2/genctl-devtools/manifests/latest: unknown: Authentication is required
Makefile:8: recipe for target 'build' failed
make: *** [build] Error 1


```


#### upgrade\-payload, attempt 2

* [Nolan Ring](https://confluence.swg.usma.ibm.com:8445/display/~nolan.ring@ibm.com) helped debug this a bit.  She gave me a docker login command for the devop sandbox repo.
* After entering my SSO creds, the build succeeded.  This should be a one time, or rare, setup item.

**docker login to sandbox**

```
[aross@aross-mint-vm]/home/aross/genctl/genctl-platform/
> docker login wcp-genctl-sandbox-docker-local.artifactory.swg-devops.com
Username: alan.ross@ibm.com
Password:
Login Succeeded
```

  




**upgrade\-payload, success** Expand source



```
> make
*** Building payloads for hostos-upgrade ***
rm -rf ./output
mkdir -p ./output ./localrepo
cp -r ./manifests ./output/
docker build --no-cache -f Dockerfile -t payload_creator:latest .
Sending build context to Docker daemon 527.4 kB
Step 1 : FROM wcp-genctl-sandbox-docker-local.artifactory.swg-devops.com/genctl-devtools:latest

latest: Pulling from genctl-devtools
a48c500ed24e: Pull complete
1e1de00ff7e1: Pull complete
0330ca45a200: Pull complete
471db38bcfbf: Pull complete
0b4aba487617: Pull complete
bc41187664e2: Pull complete
4a0aacb7161b: Pull complete
20b8958f7308: Pull complete
d7b05ea7b087: Pull complete
Successfully built 9248cd7ee04d
docker run -it -e PRERELEASE="-f17f22f" -v /home/aross/hostos/upgrade-payloads//output:/mnt/software_payload payload_creator

Reading package lists... Done
Building dependency tree
Reading state information... Done
The following additional packages will be installed:
  python-funcsigs python-functools32 python-mock python-pbr python3-pkg-resources
Suggested packages:
  python-funcsigs-doc python-mock-doc python3-setuptools
The following NEW packages will be installed:
  python-funcsigs python-functools32 python-jsonschema python-mock python-pbr python3-pkg-resources
0 upgraded, 6 newly installed, 0 to remove and 28 not upgraded.
Need to get 256 kB of archives.
After this operation, 1410 kB of additional disk space will be used.
Get:1 http://9.41.34.95:8888/ubuntu bionic/main amd64 python-funcsigs all 1.0.2-4 [13.5 kB]
Get:2 http://9.41.34.95:8888/ubuntu bionic/main amd64 python-functools32 all 3.2.3.2-3 [10.8 kB]
Get:3 http://9.41.34.95:8888/ubuntu bionic/main amd64 python-pbr all 3.1.1-3ubuntu3 [53.7 kB]
Get:4 http://9.41.34.95:8888/ubuntu bionic/main amd64 python-mock all 2.0.0-3 [47.4 kB]
Get:5 http://9.41.34.95:8888/ubuntu bionic/main amd64 python3-pkg-resources all 39.0.1-2 [98.8 kB]
Get:6 http://9.41.34.95:8888/ubuntu bionic/main amd64 python-jsonschema all 2.6.0-2 [31.5 kB]
Fetched 256 kB in 1s (293 kB/s)
debconf: delaying package configuration, since apt-utils is not installed
Selecting previously unselected package python-funcsigs.
(Reading database ... 18063 files and directories currently installed.)
Preparing to unpack .../0-python-funcsigs_1.0.2-4_all.deb ...
Unpacking python-funcsigs (1.0.2-4) ...
Selecting previously unselected package python-functools32.
Preparing to unpack .../1-python-functools32_3.2.3.2-3_all.deb ...
Unpacking python-functools32 (3.2.3.2-3) ...
Selecting previously unselected package python-pbr.
Preparing to unpack .../2-python-pbr_3.1.1-3ubuntu3_all.deb ...
Unpacking python-pbr (3.1.1-3ubuntu3) ...
Selecting previously unselected package python-mock.
Preparing to unpack .../3-python-mock_2.0.0-3_all.deb ...
Unpacking python-mock (2.0.0-3) ...
Selecting previously unselected package python3-pkg-resources.
Preparing to unpack .../4-python3-pkg-resources_39.0.1-2_all.deb ...
Unpacking python3-pkg-resources (39.0.1-2) ...
Selecting previously unselected package python-jsonschema.
Preparing to unpack .../5-python-jsonschema_2.6.0-2_all.deb ...
Unpacking python-jsonschema (2.6.0-2) ...
Setting up python-functools32 (3.2.3.2-3) ...
Setting up python3-pkg-resources (39.0.1-2) ...
Setting up python-pbr (3.1.1-3ubuntu3) ...
update-alternatives: using /usr/bin/python2-pbr to provide /usr/bin/pbr (pbr) in auto mode
Setting up python-funcsigs (1.0.2-4) ...
Setting up python-mock (2.0.0-3) ...
Setting up python-jsonschema (2.6.0-2) ...
update-alternatives: using /usr/bin/python2-jsonschema to provide /usr/bin/jsonschema (jsonschema) in auto mode
Collecting wget
  Retrying (Retry(total=4, connect=None, read=None, redirect=None, status=None)) after connection broken by 'ReadTimeoutError("HTTPSConnectionPool(host='files.pythonhosted.org', port=443): Read timed out. (read timeout=15)",)': /packages/47/6a/62e288da7bcda82b935ff0c6cfe542970f04e29c756b0e147251b2fb251f/wget-3.2.zip
  Downloading https://files.pythonhosted.org/packages/47/6a/62e288da7bcda82b935ff0c6cfe542970f04e29c756b0e147251b2fb251f/wget-3.2.zip
Building wheels for collected packages: wget
  Running setup.py bdist_wheel for wget ... done
  Stored in directory: /root/.cache/pip/wheels/40/15/30/7d8f7cea2902b4db79e3fea550d7d7b85ecb27ef992b618f3f
Successfully built wget
Installing collected packages: wget
Successfully installed wget-3.2
Collecting PyYAML
  Downloading https://files.pythonhosted.org/packages/9f/2c/9417b5c774792634834e730932745bc09a7d36754ca00acf1ccd1ac2594d/PyYAML-5.1.tar.gz (274kB)
    100% |################################| 276kB 389kB/s
Building wheels for collected packages: PyYAML
  Running setup.py bdist_wheel for PyYAML ... done
  Stored in directory: /root/.cache/pip/wheels/ad/56/bc/1522f864feb2a358ea6f1a92b4798d69ac783a28e80567a18b
Successfully built PyYAML
Installing collected packages: PyYAML
  Found existing installation: PyYAML 3.13
    Uninstalling PyYAML-3.13:
      Successfully uninstalled PyYAML-3.13
Successfully installed PyYAML-5.1
--2019-05-21 14:42:00--  http://9.41.34.95:8888/mirror/genesis/cloudlab.key
Connecting to 9.41.34.95:8888... connected.
HTTP request sent, awaiting response... 200 OK
Length: 2448 (2.4K) [application/octet-stream]
Saving to: 'cloudlab.key'

cloudlab.key                                                  100%[================================================================================================================================================>]   2.39K  --.-KB/s    in 0s

2019-05-21 14:42:00 (402 MB/s) - 'cloudlab.key' saved [2448/2448]

OK
--2019-05-21 14:42:00--  http://9.41.34.95:8888/docker/gpg
Connecting to 9.41.34.95:8888... connected.
HTTP request sent, awaiting response... 200 OK
Length: 3817 (3.7K) [application/octet-stream]
Saving to: 'gpg'

gpg                                                           100%[================================================================================================================================================>]   3.73K  --.-KB/s    in 0.001s

2019-05-21 14:42:01 (2.50 MB/s) - 'gpg' saved [3817/3817]

OK
--2019-05-21 14:42:01--  http://9.41.34.95:8888/kube/gpg
Connecting to 9.41.34.95:8888... connected.
HTTP request sent, awaiting response... 200 OK
Length: 1326 (1.3K) [application/octet-stream]
Saving to: 'gpg'

gpg                                                           100%[================================================================================================================================================>]   1.29K  --.-KB/s    in 0s

2019-05-21 14:42:01 (225 MB/s) - 'gpg' saved [1326/1326]

OK
--2019-05-21 14:42:01--  http://9.41.34.95:8888/mirror/private-ppa.launchpad.net/ibm-cloud/test/ubuntu/gpg
Connecting to 9.41.34.95:8888... connected.
HTTP request sent, awaiting response... 200 OK
Length: 1607 (1.6K) [application/octet-stream]
Saving to: 'gpg'

gpg                                                           100%[================================================================================================================================================>]   1.57K  --.-KB/s    in 0s

2019-05-21 14:42:02 (312 MB/s) - 'gpg' saved [1607/1607]

OK
Hit:1 http://9.41.34.95:8888/ubuntu bionic InRelease
Get:2 http://9.41.34.95:8888/ubuntu bionic-updates InRelease [88.7 kB]
Ign:3 http://9.41.34.95:8888/genesis genesis InRelease
Get:4 http://9.41.34.95:8888/docker xenial InRelease [66.2 kB]
Get:5 http://9.41.34.95:8888/kube kubernetes-xenial InRelease [8993 B]
Get:6 http://9.41.34.95:8888/canonical-kernel bionic InRelease [21.3 kB]
Get:7 http://9.41.34.95:8888/genesis genesis Release [7557 B]
Ign:8 http://9.41.34.95:8888/ubuntu bionic-updates/universe amd64 Packages
Ign:9 http://9.41.34.95:8888/ubuntu bionic-updates/main amd64 Packages
Get:10 http://9.41.34.95:8888/docker xenial/stable amd64 Packages [8785 B]
Ign:11 http://9.41.34.95:8888/kube kubernetes-xenial/main amd64 Packages
Get:12 http://9.41.34.95:8888/genesis genesis Release.gpg [683 B]
Ign:13 http://9.41.34.95:8888/canonical-kernel bionic/main amd64 Packages
Get:8 http://9.41.34.95:8888/ubuntu bionic-updates/universe amd64 Packages [1199 kB]
Get:9 http://9.41.34.95:8888/ubuntu bionic-updates/main amd64 Packages [802 kB]
Get:11 http://9.41.34.95:8888/kube kubernetes-xenial/main amd64 Packages [26.0 kB]
Get:14 http://9.41.34.95:8888/genesis genesis/main amd64 Packages [539 kB]
Get:13 http://9.41.34.95:8888/canonical-kernel bionic/main amd64 Packages [16.4 kB]
Fetched 2785 kB in 2s (1138 kB/s)
Reading package lists... Done
Building dependency tree
Reading state information... Done
112 packages can be upgraded. Run 'apt list --upgradable' to see them.
NEWVERION=0.0.1-f17f22f
NEWVERION=0.0.1-f17f22f
NEWVERION=0.0.1-f17f22f
base-os-sw base-net-sw nextgen-os-sw
--------------------------------------------------------------------
base-os-sw
--------------------------------------------------------------------

calicoctl
 > calicoctl 1.6.3-rc1
100% [..........................................................................] 6000372 / 6000372 Done
curl
 > curl 7.58.0-2ubuntu3
100% [............................................................................] 158884 / 158884 Done
fping
 > fping 4.0-6
100% [..............................................................................] 32052 / 32052 Done
ifenslave
 > ifenslave 2.9ubuntu1
100% [..............................................................................] 13272 / 13272 Done
ifupdown
 > ifupdown 0.8.17ubuntu1
100% [..............................................................................] 55780 / 55780 Done
ntpdate
 > ntpdate 1:4.2.8p10+dfsg-5ubuntu7
100% [..............................................................................] 51560 / 51560 Done
screen
 > screen 4.6.2-1
100% [............................................................................] 575360 / 575360 Done
frr
 > frr 5.0.1-1~ubuntu18.04+1
100% [..........................................................................] 2311224 / 2311224 Done
kernel-mft-dkms
 > kernel-mft-dkms 4.10.0-104
100% [..............................................................................] 22502 / 22502 Done
mft
 > mft 4.10.0-600
100% [........................................................................] 53148976 / 53148976 Done
libvirt0
 > libvirt0 4.8.0-2ibm4
100% [..........................................................................] 1328044 / 1328044 Done
libvirt-bin
 > libvirt-bin 4.8.0-2ibm4
100% [................................................................................] 7252 / 7252 Done
libvirt-clients
 > libvirt-clients 4.8.0-2ibm4
100% [............................................................................] 619212 / 619212 Done
libvirt-daemon
 > libvirt-daemon 4.8.0-2ibm4
100% [..........................................................................] 1744024 / 1744024 Done
libvirt-daemon-system
 > libvirt-daemon-system 4.8.0-2ibm4
100% [..............................................................................] 73188 / 73188 Done
qemu-block-extra
 > qemu-block-extra 2.11+dfsg-1ubuntu7.8-1ibm1
100% [..............................................................................] 81380 / 81380 Done
qemu-system
 > qemu-system 2.11+dfsg-1ubuntu7.8-1ibm1
100% [..............................................................................] 80512 / 80512 Done
qemu-system-common
 > qemu-system-common 2.11+dfsg-1ubuntu7.8-1ibm1
100% [............................................................................] 231272 / 231272 Done
qemu-system-misc
 > qemu-system-misc 2.11+dfsg-1ubuntu7.8-1ibm1
100% [..............................................................................] 83252 / 83252 Done
qemu-system-x86
 > qemu-system-x86 2.11+dfsg-1ubuntu7.8-1ibm1
100% [..........................................................................] 2445908 / 2445908 Done
qemu-utils
 > qemu-utils 2.11+dfsg-1ubuntu7.8-1ibm1
100% [..........................................................................] 1020440 / 1020440 Done
nfs-common
 > nfs-common 1:1.3.4-2.1ubuntu5
100% [............................................................................] 205028 / 205028 Done
nfs-kernel-server
 > nfs-kernel-server 1:1.3.4-2.1ubuntu5
100% [..............................................................................] 93976 / 93976 Done
containerd.io
 > containerd.io 1.2.2-3
100% [........................................................................] 19870542 / 19870542 Done
conntrack
 > conntrack 1:1.4.4+snapshot20161117-6ubuntu2
100% [..............................................................................] 30580 / 30580 Done
docker-ce
 > docker-ce 18.09.2~3-0~ubuntu-xenial
100% [........................................................................] 17371786 / 17371786 Done
docker-ce-cli
 > docker-ce-cli 18.09.2~3-0~ubuntu-xenial
100% [........................................................................] 12993236 / 12993236 Done
python-docker
 > python-docker 3.1.1
100% [..............................................................................] 71884 / 71884 Done
python-docker-pycreds
 > python-docker-pycreds 0.2.2
100% [................................................................................] 3612 / 3612 Done
socat
 > socat 1.7.3.2-2ubuntu2
100% [............................................................................] 341772 / 341772 Done
tp-cfssl
 > tp-cfssl 1.2
100% [..........................................................................] 2494536 / 2494536 Done
tmux
 > tmux 2.6-3
100% [............................................................................] 248168 / 248168 Done
cryptsetup-bin
 > cryptsetup-bin 2:2.0.2-1ubuntu1.1
100% [..............................................................................] 92936 / 92936 Done
irqbalance
 > irqbalance 1.3.0-0.1
100% [..............................................................................] 47928 / 47928 Done
netcat-openbsd
 > netcat-openbsd 1.187-1
100% [..............................................................................] 39596 / 39596 Done
pipexec
 > pipexec 2.5.5-1
100% [..............................................................................] 16842 / 16842 Done
python-backports.ssl-match-hostname
 > python-backports.ssl-match-hostname 3.5.0.1-1
100% [................................................................................] 7024 / 7024 Done
python-grpcio
 > python-grpcio 1.14.0
100% [..........................................................................] 5836368 / 5836368 Done
python-jmespath
 > python-jmespath 0.9.3-1ubuntu1
100% [..............................................................................] 21220 / 21220 Done
python-jsonschema
 > python-jsonschema 2.6.0-2
100% [..............................................................................] 31484 / 31484 Done
python-netaddr
 > python-netaddr 0.7.19-1
100% [............................................................................] 213348 / 213348 Done
python-protobuf
 > python-protobuf 3.6.1
100% [............................................................................] 116244 / 116244 Done
python-websocket
 > python-websocket 0.44.0-0ubuntu2
100% [..............................................................................] 30658 / 30658 Done
python-yaml
 > python-yaml 3.12-1build2
100% [............................................................................] 115300 / 115300 Done
--------------------------------------------------------------------
base-net-sw
--------------------------------------------------------------------

devlink-mlx
 > devlink-mlx 4.15.0-24.27.2
100% [..............................................................................] 41652 / 41652 Done
mlx-config
 > mlx-config 2.4.0
100% [..............................................................................] 14064 / 14064 Done
mlx-firmware
 > mlx-firmware 2.2.0
100% [..........................................................................] 4338128 / 4338128 Done
cloudnet-gobgp
 > cloudnet-gobgp 3.4.0
100% [..........................................................................] 8248988 / 8248988 Done
fabcon-cli
 > fabcon-cli 3.31.0
100% [..........................................................................] 7405324 / 7405324 Done
fabcon-mlx
 > fabcon-mlx 3.31.0
100% [..........................................................................] 5372580 / 5372580 Done
hmonagent-mlx
 > hmonagent-mlx 2.2.0
100% [..........................................................................] 5071316 / 5071316 Done
iobricks-mlx
 > iobricks-mlx 3.17.0
100% [........................................................................] 31859492 / 31859492 Done
neohost-backend
 > neohost-backend 1.3.0-21
100% [........................................................................] 50647830 / 50647830 Done
neohost-sdk
 > neohost-sdk 1.3.0-21
100% [..............................................................................] 23714 / 23714 Done
nscon-cli
 > nscon-cli 3.13.0
100% [..............................................................................] 10144 / 10144 Done
--------------------------------------------------------------------
nextgen-os-sw
--------------------------------------------------------------------

cni-plugins
 > cni-plugins 1.0-1~20190502.1508
100% [..........................................................................] 2549040 / 2549040 Done
cld-computeproxy
 > cld-computeproxy 1.1~20190518.1951
100% [..........................................................................] 5606402 / 5606402 Done
cld-computetools
 > cld-computetools 1.0-1~20190517.1848
100% [..........................................................................] 8997324 / 8997324 Done
cld-blockagent-cli
 > cld-blockagent-cli 1.0-1~20190518.0326
100% [..........................................................................] 2139688 / 2139688 Done
cld-blockagent-server
 > cld-blockagent-server 1.0-1~20190518.0326
100% [..........................................................................] 2678456 / 2678456 Done
cld-nfsimageservice-cli
 > cld-nfsimageservice-cli 1.0-1~20190518.0329~4caf8eee7ea1aab1fa5de996df9ff8bbde623c02
100% [..........................................................................] 1712296 / 1712296 Done
cld-nfsimageservice-server
 > cld-nfsimageservice-server 1.0-1~20190518.0329~4caf8eee7ea1aab1fa5de996df9ff8bbde623c02
100% [..........................................................................] 1642260 / 1642260 Done
cld-poolagent-cli
 > cld-poolagent-cli 1.0-1~20190518.0328~4caf8eee7ea1aab1fa5de996df9ff8bbde623c02
100% [..........................................................................] 2153854 / 2153854 Done

>

calicoctl
curl
 > libcurl4 7.58.0-2ubuntu3.6
100% [............................................................................] 214148 / 214148 Done
fping
ifenslave
ifupdown
ntpdate
screen
 > libutempter0 1.1.6-3
100% [................................................................................] 7898 / 7898 Done
frr
 > libc-ares2 1.14.0-1
100% [..............................................................................] 37144 / 37144 Done
kernel-mft-dkms
 > dkms 2.3-3ubuntu9.2
100% [..............................................................................] 67960 / 67960 Done
mft
libvirt0
 > libavahi-client3 0.7-3.1ubuntu1.2
100% [..............................................................................] 25236 / 25236 Done
 > libavahi-common3 0.7-3.1ubuntu1.2
100% [..............................................................................] 21624 / 21624 Done
 > libavahi-common-data 0.7-3.1ubuntu1.2
100% [..............................................................................] 22136 / 22136 Done
 > libcurl3-gnutls 7.58.0-2ubuntu3.6
100% [............................................................................] 212324 / 212324 Done
 > libnl-3-200 3.2.29-0ubuntu3
100% [..............................................................................] 52844 / 52844 Done
 > libnuma1 2.0.11-2.1ubuntu0.1
100% [..............................................................................] 21980 / 21980 Done
 > libssh-4 0.8.0~20170825.94fa1e38-1ubuntu0.2
100% [............................................................................] 168548 / 168548 Done
 > libyajl2 2.1.0-2build1
100% [..............................................................................] 20008 / 20008 Done
libvirt-bin
 > libnetcf1 1:0.2.8-1ubuntu2
100% [..............................................................................] 46366 / 46366 Done
 > libaugeas0 1.10.1-2
100% [............................................................................] 159460 / 159460 Done
 > augeas-lenses 1.10.1-2
100% [............................................................................] 299948 / 299948 Done
 > libnl-route-3-200 3.2.29-0ubuntu3
100% [............................................................................] 146162 / 146162 Done
 > libxslt1.1 1.1.29-5ubuntu0.1
100% [............................................................................] 149984 / 149984 Done
 > libpciaccess0 0.14-1
100% [..............................................................................] 17908 / 17908 Done
 > libxen-4.9 4.9.2-0ubuntu1
100% [............................................................................] 398952 / 398952 Done
 > libxenstore3.0 4.9.2-0ubuntu1
100% [..............................................................................] 19692 / 19692 Done
 > policykit-1 0.105-20ubuntu0.18.04.5
100% [..............................................................................] 53532 / 53532 Done
 > libpolkit-agent-1-0 0.105-20ubuntu0.18.04.5
100% [..............................................................................] 14928 / 14928 Done
 > libpolkit-gobject-1-0 0.105-20ubuntu0.18.04.5
100% [..............................................................................] 36428 / 36428 Done
 > libpolkit-backend-1-0 0.105-20ubuntu0.18.04.5
100% [..............................................................................] 36316 / 36316 Done
libvirt-clients
libvirt-daemon
libvirt-daemon-system
qemu-block-extra
qemu-system
 > libaio1 0.3.110-5ubuntu0.1
100% [................................................................................] 6476 / 6476 Done
 > libpixman-1-0 0.34.0-2
100% [............................................................................] 228516 / 228516 Done
 > seabios 1.10.3
100% [............................................................................] 131630 / 131630 Done
 > ipxe-qemu 1.0.0+git-20180124.fbe8c52d-0ubuntu2.2
100% [............................................................................] 911712 / 911712 Done
qemu-system-common
qemu-system-misc
qemu-system-x86
qemu-utils
nfs-common
 > libcomerr2 1.44.1-1ubuntu1.1
100% [................................................................................] 2712 / 2712 Done
 > libnfsidmap2 0.25-5.1
100% [..............................................................................] 27180 / 27180 Done
 > libtirpc1 0.2.5-1.2ubuntu0.1
100% [..............................................................................] 75708 / 75708 Done
 > rpcbind 0.2.3-0.6
100% [..............................................................................] 40644 / 40644 Done
 > keyutils 1.5.9-9.2ubuntu2
100% [..............................................................................] 47896 / 47896 Done
nfs-kernel-server
containerd.io
conntrack
docker-ce
 > libltdl7 2.4.6-2
100% [..............................................................................] 38760 / 38760 Done
docker-ce-cli
python-docker
 > python-requests 2.18.4-2ubuntu0.1
100% [..............................................................................] 58496 / 58496 Done
 > python-certifi 2018.1.18-2
100% [............................................................................] 144096 / 144096 Done
 > python-chardet 3.0.4-1
100% [..............................................................................] 80292 / 80292 Done
 > python-urllib3 1.22-1
100% [..............................................................................] 85052 / 85052 Done
python-docker-pycreds
socat
tp-cfssl
tmux
cryptsetup-bin
irqbalance
netcat-openbsd
pipexec
python-backports.ssl-match-hostname
python-grpcio
 > python-concurrent.futures 3.2.0-1
100% [..............................................................................] 34228 / 34228 Done
 > libjs-sphinxdoc 1.6.7-1ubuntu1
100% [..............................................................................] 85568 / 85568 Done
 > libjs-jquery 3.2.1-1
100% [............................................................................] 151904 / 151904 Done
 > libjs-underscore 1.8.3~dfsg-1
100% [..............................................................................] 59930 / 59930 Done
python-jmespath
python-jsonschema
 > python-functools32 3.2.3.2-3
100% [..............................................................................] 10834 / 10834 Done
 > python-mock 2.0.0-3
100% [..............................................................................] 47428 / 47428 Done
 > python-funcsigs 1.0.2-4
100% [..............................................................................] 13452 / 13452 Done
 > python-pbr 3.1.1-3ubuntu3
100% [..............................................................................] 53688 / 53688 Done
python-netaddr
 > ieee-data 20180204.1
100% [..........................................................................] 1538848 / 1538848 Done
python-protobuf
python-websocket
python-yaml

>

devlink-mlx
mlx-config
mlx-firmware
cloudnet-gobgp
fabcon-cli
fabcon-mlx
hmonagent-mlx
iobricks-mlx
neohost-backend
neohost-sdk
nscon-cli

>

cni-plugins
cld-computeproxy
cld-computetools
cld-blockagent-cli
cld-blockagent-server
cld-nfsimageservice-cli
cld-nfsimageservice-server
cld-poolagent-cli
('base-os-sw', '/tmp/base-os-sw_0.0.1-f17f22f_amd64/base-os-sw_0.0.1-f17f22f_amd64.yml')
('base-net-sw', '/tmp/base-net-sw_0.0.1-f17f22f_amd64/base-net-sw_0.0.1-f17f22f_amd64.yml')
('nextgen-os-sw', '/tmp/nextgen-os-sw_0.0.1-f17f22f_amd64/nextgen-os-sw_0.0.1-f17f22f_amd64.yml')
dpkg-scanpackages: warning: Packages in archive but missing from override file:
dpkg-scanpackages: warning:   cloudnet-gobgp devlink-mlx fabcon-cli fabcon-mlx hmonagent-mlx iobricks-mlx mlx-config mlx-firmware neohost-backend neohost-sdk nscon-cli
dpkg-scanpackages: info: Wrote 11 entries to output Packages file.
cat: conf/distributions: No such file or directory
./
./hmonagent-mlx_2.2.0_amd64.deb
./cloudnet-gobgp_3.4.0_amd64.deb
./base-net-sw_0.0.1-f17f22f_amd64.yml
./Packages
./Packages.gz
./fabcon-mlx_3.31.0_amd64.deb
./mlx-config_2.4.0_all.deb
./neohost-backend_1.3.0-21_amd64.deb
./neohost-sdk_1.3.0-21_amd64.deb
./fabcon-cli_3.31.0_amd64.deb
./nscon-cli_3.13.0_amd64.deb
./iobricks-mlx_3.17.0_amd64.deb
./Release
./devlink-mlx_4.15.0-24.27.2_amd64.deb
./mlx-firmware_2.2.0_all.deb
./InRelease
dpkg-scanpackages: warning: Packages in archive but missing from override file:
dpkg-scanpackages: warning:   augeas-lenses calicoctl conntrack containerd.io cryptsetup-bin curl dkms docker-ce docker-ce-cli fping frr ieee-data ifenslave ifupdown ipxe-qemu irqbalance kernel-mft-dkms keyutils libaio1 libaugeas0 libavahi-client3 libavahi-common-data libavahi-common3 libc-ares2 libcomerr2 libcurl3-gnutls libcurl4 libjs-jquery libjs-sphinxdoc libjs-underscore libltdl7 libnetcf1 libnfsidmap2 libnl-3-200 libnl-route-3-200 libnuma1 libpciaccess0 libpixman-1-0 libpolkit-agent-1-0 libpolkit-backend-1-0 libpolkit-gobject-1-0 libssh-4 libtirpc1 libutempter0 libvirt-bin libvirt-clients libvirt-daemon libvirt-daemon-system libvirt0 libxen-4.9 libxenstore3.0 libxslt1.1 libyajl2 mft netcat-openbsd nfs-common nfs-kernel-server ntpdate pipexec policykit-1 python-backports.ssl-match-hostname python-certifi python-chardet python-concurrent.futures python-docker python-docker-pycreds python-funcsigs python-functools32 python-grpcio python-jmespath python-jsonschema python-mock python-netaddr python-pbr python-protobuf python-requests python-urllib3 python-websocket python-yaml qemu-block-extra qemu-system qemu-system-common qemu-system-misc qemu-system-x86 qemu-utils rpcbind screen seabios socat tmux tp-cfssl
dpkg-scanpackages: info: Wrote 91 entries to output Packages file.
cat: conf/distributions: No such file or directory
./
./augeas-lenses_1.10.1-2_all.deb
./nfs-kernel-server_1.3.4-2.1ubuntu5_amd64.deb
./libnuma1_2.0.11-2.1ubuntu0.1_amd64.deb
./ipxe-qemu_1.0.0+git-20180124.fbe8c52d-0ubuntu2.2_all.deb
./python-chardet_3.0.4-1_all.deb
./python-concurrent.futures_3.2.0-1_all.deb
./libavahi-common-data_0.7-3.1ubuntu1.2_amd64.deb
./libcurl3-gnutls_7.58.0-2ubuntu3.6_amd64.deb
./containerd.io_1.2.2-3_amd64.deb
./python-grpcio_1.14.0_all.deb
./libpolkit-agent-1-0_0.105-20ubuntu0.18.04.5_amd64.deb
./qemu-block-extra_2.11+dfsg-1ubuntu7.8-1ibm1_amd64.deb
./libltdl7_2.4.6-2_amd64.deb
./python-funcsigs_1.0.2-4_all.deb
./libjs-underscore_1.8.3~dfsg-1_all.deb
./python-backports.ssl-match-hostname_3.5.0.1-1_all.deb
./qemu-system_2.11+dfsg-1ubuntu7.8-1ibm1_amd64.deb
./libxenstore3.0_4.9.2-0ubuntu1_amd64.deb
./libnl-3-200_3.2.29-0ubuntu3_amd64.deb
./libavahi-client3_0.7-3.1ubuntu1.2_amd64.deb
./libnl-route-3-200_3.2.29-0ubuntu3_amd64.deb
./ifupdown_0.8.17ubuntu1_amd64.deb
./python-docker-pycreds_0.2.2_all.deb
./cryptsetup-bin_2.0.2-1ubuntu1.1_amd64.deb
./dkms_2.3-3ubuntu9.2_all.deb
./libnfsidmap2_0.25-5.1_amd64.deb
./python-mock_2.0.0-3_all.deb
./python-websocket_0.44.0-0ubuntu2_all.deb
./libvirt-bin_4.8.0-2ibm4_amd64.deb
./netcat-openbsd_1.187-1_amd64.deb
./libutempter0_1.1.6-3_amd64.deb
./mft_4.10.0-600_amd64.deb
./policykit-1_0.105-20ubuntu0.18.04.5_amd64.deb
./libyajl2_2.1.0-2build1_amd64.deb
./libc-ares2_1.14.0-1_amd64.deb
./nfs-common_1.3.4-2.1ubuntu5_amd64.deb
./python-protobuf_3.6.1_all.deb
./calicoctl_1.6.3-rc1_amd64.deb
./ntpdate_4.2.8p10+dfsg-5ubuntu7_amd64.deb
./ifenslave_2.9ubuntu1_all.deb
./docker-ce-cli_18.09.2~3-0~ubuntu-xenial_amd64.deb
./libaio1_0.3.110-5ubuntu0.1_amd64.deb
./libaugeas0_1.10.1-2_amd64.deb
./seabios_1.10.3_all.deb
./Packages
./python-pbr_3.1.1-3ubuntu3_all.deb
./python-requests_2.18.4-2ubuntu0.1_all.deb
./libvirt-daemon_4.8.0-2ibm4_amd64.deb
./libavahi-common3_0.7-3.1ubuntu1.2_amd64.deb
./base-os-sw_0.0.1-f17f22f_amd64.yml
./Packages.gz
./libcurl4_7.58.0-2ubuntu3.6_amd64.deb
./libssh-4_0.8.0~20170825.94fa1e38-1ubuntu0.2_amd64.deb
./tmux_2.6-3_amd64.deb
./frr_5.0.1-1~ubuntu18.04+1_amd64.deb
./qemu-utils_2.11+dfsg-1ubuntu7.8-1ibm1_amd64.deb
./libvirt-clients_4.8.0-2ibm4_amd64.deb
./python-urllib3_1.22-1_all.deb
./python-certifi_2018.1.18-2_all.deb
./python-functools32_3.2.3.2-3_all.deb
./qemu-system-x86_2.11+dfsg-1ubuntu7.8-1ibm1_amd64.deb
./keyutils_1.5.9-9.2ubuntu2_amd64.deb
./libpciaccess0_0.14-1_amd64.deb
./libcomerr2_1.44.1-1ubuntu1.1_amd64.deb
./libpixman-1-0_0.34.0-2_amd64.deb
./irqbalance_1.3.0-0.1_amd64.deb
./fping_4.0-6_amd64.deb
./python-netaddr_0.7.19-1_all.deb
./tp-cfssl_1.2_all.deb
./libnetcf1_0.2.8-1ubuntu2_amd64.deb
./libvirt0_4.8.0-2ibm4_amd64.deb
./python-jsonschema_2.6.0-2_all.deb
./docker-ce_18.09.2~3-0~ubuntu-xenial_amd64.deb
./libxslt1.1_1.1.29-5ubuntu0.1_amd64.deb
./screen_4.6.2-1_amd64.deb
./libpolkit-backend-1-0_0.105-20ubuntu0.18.04.5_amd64.deb
./rpcbind_0.2.3-0.6_amd64.deb
./libjs-jquery_3.2.1-1_all.deb
./python-docker_3.1.1_all.deb
./libpolkit-gobject-1-0_0.105-20ubuntu0.18.04.5_amd64.deb
./qemu-system-common_2.11+dfsg-1ubuntu7.8-1ibm1_amd64.deb
./libjs-sphinxdoc_1.6.7-1ubuntu1_all.deb
./libxen-4.9_4.9.2-0ubuntu1_amd64.deb
./libvirt-daemon-system_4.8.0-2ibm4_amd64.deb
./python-yaml_3.12-1build2_amd64.deb
./ieee-data_20180204.1_all.deb
./libtirpc1_0.2.5-1.2ubuntu0.1_amd64.deb
./qemu-system-misc_2.11+dfsg-1ubuntu7.8-1ibm1_amd64.deb
./pipexec_2.5.5-1_amd64.deb
./curl_7.58.0-2ubuntu3_amd64.deb
./Release
./socat_1.7.3.2-2ubuntu2_amd64.deb
./InRelease
./python-jmespath_0.9.3-1ubuntu1_all.deb
./kernel-mft-dkms_4.10.0-104_all.deb
./conntrack_1.4.4+snapshot20161117-6ubuntu2_amd64.deb
dpkg-scanpackages: warning: Packages in archive but missing from override file:
dpkg-scanpackages: warning:   cld-blockagent-cli cld-blockagent-server cld-computeproxy cld-computetools cld-nfsimageservice-cli cld-nfsimageservice-server cld-poolagent-cli cni-plugins
dpkg-scanpackages: info: Wrote 8 entries to output Packages file.
cat: conf/distributions: No such file or directory
./
./cld-nfsimageservice-cli_1.0-1~20190518.0329~4caf8eee7ea1aab1fa5de996df9ff8bbde623c02_amd64.deb
./cld-computeproxy_1.1~20190518.1951_amd64.deb
./cld-nfsimageservice-server_1.0-1~20190518.0329~4caf8eee7ea1aab1fa5de996df9ff8bbde623c02_amd64.deb
./cld-blockagent-server_1.0-1~20190518.0326_amd64.deb
./cld-poolagent-cli_1.0-1~20190518.0328~4caf8eee7ea1aab1fa5de996df9ff8bbde623c02_amd64.deb
./cld-computetools_1.0-1~20190517.1848_amd64.deb
./nextgen-os-sw_0.0.1-f17f22f_amd64.yml
./Packages
./Packages.gz
./cni-plugins_1.0-1~20190502.1508_amd64.deb
./cld-blockagent-cli_1.0-1~20190518.0326_amd64.deb
./Release
./InRelease
rm -rf ./output/manifests


```


  


#### base\-net\-sw\-release attempt 1

* failed because ARTIFACTORY\_LOGIN environment variable is not set.
* Will look for all necessary environment variable that need to be set.

  




**base\-net\-sw\-release** Expand source



```
[aross@aross-mint-vm]/home/aross/hostos/base-net-sw-release/
> make clean && make
rm -rf ./output
Makefile:25: *** ARTIFACTORY_LOGIN is not set.  Stop.


```


  


#### base\-net\-sw\-release attempt 2

* I set two artifactory credential environment variables and this helped details below.
* Note, you have to have an artifactory log and api key.  Someone (who) has to add you to IBM artifactory, I think a 'blue something', then you can generate the api key using the artifactory url.  In tactical days, must users had their artifactory api key stored in \~/.cache/art\_apikey
* Build still failed.

**Artifactory credentials**

```
> export ARTIFACTORY_LOGIN=alan.ross@ibm.com
> export ARTIFACTORY_ACCESS_TOKEN=$(cat ~/.cache/art_apikey)
```

  




**base\-net\-sw\-release attempt 2** Expand source



```
> make
*** Building release bundle for hostos-base-net-sw-release ***
rm -rf ./output
mkdir -p ./output
cp -r ./hostos-base-net-sw-release.yml ./output/hostos-base-net-sw-release_0.0.1-87dd027_all.yml
sed -i -e 's|release_version: 0.0.1|release_version: 0.0.1-87dd027|' ./output/hostos-base-net-sw-release_0.0.1-87dd027_all.yml
curl https://wcp-genctl-sandbox-docker-local.artifactory.swg-devops.com:443/artifactory/wcp-genctl-sandbox-generic-local/cloudlab/hostos/payloads/base-net-sw_0.0.1-0a28cf1_amd64.tgz ...
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  106M  100  106M    0     0  1704k      0  0:01:04  0:01:04 --:--:-- 2508k
curl https://wcp-genctl-sandbox-docker-local.artifactory.swg-devops.com:443/artifactory/wcp-genctl-sandbox-generic-local/cloudlab/hostos/payloads/base-net-sw_0.0.1-0a28cf1_amd64.yml ...
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  2794  100  2794    0     0   4508      0 --:--:-- --:--:-- --:--:--  4506
docker build -t hostos-base-net-sw-release:0.0.1-87dd027 --build-arg TOOLS_DOCKER_IMAGE=wcp-genctl-sandbox-docker-local.artifactory.swg-devops.com/hostos/hostos-upgrade-tools:0.0.1-5906a16 -f /home/aross/hostos/base-net-sw-release//Dockerfile .
Sending build context to Docker daemon   112 MB
Step 1 : ARG TOOLS_DOCKER_IMAGE=hostos-upgrade-tools:latest
Please provide a source image with `from` prior to commit
Makefile:25: recipe for target 'build' failed
make: *** [build] Error 1

```


  


  


  




 


Document generated by Confluence on Jul 15, 2024 13:04


[Atlassian](https://www.atlassian.com/)


 


