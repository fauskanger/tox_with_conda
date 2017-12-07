[tox]: https://tox.readthedocs.io

Get from GitHub: ``git clone https://github.com/fauskanger/tox_with_conda.git``

Install with pip: ``pip install tox_with_conda``

# Tox with conda

[tox][tox] is a Python framework for creating multiple environments, and then install your project and run tests in each of them to make the process of running tests across versions an easy task.

However, I (and other) experienced that using ``tox`` and ``conda`` (miniconda or Anaconda) was at a first glance _not a good match_, or at least on Windows. Tox expects to work from a virtualenv and not a conda environment. However, it’s possible with a few, simple steps.

## Motivation
To use tox and conda on Windows, the following recommendations worked for me:

 - For tox to find your environments, consider to __either__:
    - Install environments in C:\PythonXY, where X and Y is major and minor version, respectively; __or__
    - Make a symlink from C:\PythonXY to your real path. E.g.: ``mklink /J C:\Python27 C:\Anaconda3\envs\myPy27env``

 - In the environment where you call tox: install virtualenv with conda and not with pip. This seems to work well with Anaconda.

__Instead__ of going through these steps manually for each new project, __use the__ ``ToxEnvMatcher``-class or run the _tox\_with\_conda.py_-script described in the __next section__.


## Examples on how to use

### From code

    my_envs = join('E:\\', 'Anaconda3', 'envs')
    tem = ToxEnvMatcher(my_envs, env_prefix='py')
    for version in '27,34,35,36'.split(','):
        tem.make(version)
        
This will make four conda environments in the E:\Anaconda3\envs folder:
 
 - E:\Anaconda3\envs\py27 
 - E:\Anaconda3\envs\py34 
 - E:\Anaconda3\envs\py35 
 - E:\Anaconda3\envs\py36
 
 For each of the versions, a symbolic link is made from C:\PythonXY to E:\Anaconda3\envs\pyXY

### From command line
Instead of creating a script to use ``ToxEnvMatcher``, the _tox\_with\_conda.py_ script can be called with arguments:

    python tox_with_conda.py E:\Anaconda3\envs 27 34 35 36 37
    
__Note:__ if installed with pip, instead of ``python tox_with_conda.py ...`` use ``python -m tox_with_conda ...``.
    
Environment prefix (defaults to _py_) can be overridden with ``-p``/``--env_prefix`` options:

    python tox_with_conda.py E:\Anaconda3\envs 27 34 35 36 37 -p Python
    
This will create new environments in E:\Anaconda3\envs\PythonXY instead of E:\Anaconda3\envs\pyXY
    
If, for some reason you need to, it's possible to use the ``-b``/``--base`` 
option to override the default base location (C:\Python):

    E:\dev> tox_with_conda.py E:\Anaconda3\envs 27 34 35 36 37 --base D:\Python
