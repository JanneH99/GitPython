![Python package](https://github.com/gitpython-developers/GitPython/workflows/Python%20package/badge.svg)
[![Documentation Status](https://readthedocs.org/projects/gitpython/badge/?version=stable)](https://readthedocs.org/projects/gitpython/?badge=stable)
[![Packaging status](https://repology.org/badge/tiny-repos/python:gitpython.svg)](https://repology.org/metapackage/python:gitpython/versions)

## [Gitoxide](https://github.com/Byron/gitoxide): A peek into the future…

I started working on GitPython in 2009, back in the days when Python was 'my thing' and I had great plans with it.
Of course, back in the days, I didn't really know what I was doing and this shows in many places. Somewhat similar to
Python this happens to be 'good enough', but at the same time is deeply flawed and broken beyond repair.

By now, GitPython is widely used and I am sure there is a good reason for that, it's something to be proud of and happy about.
The community is maintaining the software and is keeping it relevant for which I am absolutely grateful. For the time to come I am happy to continue maintaining GitPython, remaining hopeful that one day it won't be needed anymore.

More than 15 years after my first meeting with 'git' I am still in excited about it, and am happy to finally have the tools and
probably the skills to scratch that itch of mine: implement `git` in a way that makes tool creation a piece of cake for most.

If you like the idea and want to learn more, please head over to [gitoxide](https://github.com/Byron/gitoxide), an
implementation of 'git' in [Rust](https://www.rust-lang.org).

*(Please note that `gitoxide` is not currently available for use in Python, and that Rust is required.)*

## GitPython

GitPython is a python library used to interact with git repositories, high-level like git-porcelain,
or low-level like git-plumbing.

It provides abstractions of git objects for easy access of repository data often backed by calling the `git`
command-line program.

### DEVELOPMENT STATUS

This project is in **maintenance mode**, which means that

- …there will be no feature development, unless these are contributed
- …there will be no bug fixes, unless they are relevant to the safety of users, or contributed
- …issues will be responded to with waiting times of up to a month

The project is open to contributions of all kinds, as well as new maintainers.

### REQUIREMENTS

GitPython needs the `git` executable to be installed on the system and available in your
`PATH` for most operations. If it is not in your `PATH`, you can help GitPython find it
by setting the `GIT_PYTHON_GIT_EXECUTABLE=<path/to/git>` environment variable.

- Git (1.7.x or newer)
- Python >= 3.7

The list of dependencies are listed in `./requirements.txt` and `./test-requirements.txt`.
The installer takes care of installing them for you.

### INSTALL

GitPython and its required package dependencies can be installed in any of the following ways, all of which should typically be done in a [virtual environment](https://docs.python.org/3/tutorial/venv.html).

#### From PyPI

To obtain and install a copy [from PyPI](https://pypi.org/project/GitPython/), run:

```sh
pip install GitPython
```

(A distribution package can also be downloaded for manual installation at [the PyPI page](https://pypi.org/project/GitPython/).)

#### From downloaded source code

If you have downloaded the source code, run this from inside the unpacked `GitPython` directory:

```sh
pip install .
```

#### By cloning the source code repository

To clone the [the GitHub repository](https://github.com/gitpython-developers/GitPython) from source to work on the code, you can do it like so:

```sh
git clone https://github.com/gitpython-developers/GitPython
cd GitPython
./init-tests-after-clone.sh
```

On Windows, `./init-tests-after-clone.sh` can be run in a Git Bash shell.

If you are cloning [your own fork](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks/about-forks), then replace the above `git clone` command with one that gives the URL of your fork. Or use this [`gh`](https://cli.github.com/) command (assuming you have `gh` and your fork is called `GitPython`):

```sh
gh repo clone GitPython
```

Having cloned the repo, create and activate your [virtual environment](https://docs.python.org/3/tutorial/venv.html).

Then make an [editable install](https://pip.pypa.io/en/stable/topics/local-project-installs/#editable-installs):

```sh
pip install -e ".[test]"
```

In the less common case that you do not want to install test dependencies, `pip install -e .` can be used instead.

#### With editable *dependencies* (not preferred, and rarely needed)

In rare cases, you may want to work on GitPython and one or both of its [gitdb](https://github.com/gitpython-developers/gitdb) and [smmap](https://github.com/gitpython-developers/smmap) dependencies at the same time, with changes in your local working copy of gitdb or smmap immediately reflected in the behavior of your local working copy of GitPython. This can be done by making editable installations of those dependencies in the same virtual environment where you install GitPython.

If you want to do that *and* you want the versions in GitPython's git submodules to be used, then pass `-e git/ext/gitdb` and/or `-e git/ext/gitdb/gitdb/ext/smmap` to `pip install`. This can be done in any order, and in separate `pip install` commands or the same one, so long as `-e` appears before *each* path. For example, you can install GitPython, gitdb, and smmap editably in the currently active virtual environment this way:

```sh
pip install -e ".[test]" -e git/ext/gitdb -e git/ext/gitdb/gitdb/ext/smmap
```

The submodules must have been cloned for that to work, but that will already be the case if you have run `./init-tests-after-clone.sh`. You can use `pip list` to check which packages are installed editably and which are installed normally.

To reiterate, this approach should only rarely be used. For most development it is preferable to allow the gitdb and smmap dependencices to be retrieved automatically from PyPI in their latest stable packaged versions.

### Limitations

#### Leakage of System Resources

GitPython is not suited for long-running processes (like daemons) as it tends to
leak system resources. It was written in a time where destructors (as implemented
in the `__del__` method) still ran deterministically.

In case you still want to use it in such a context, you will want to search the
codebase for `__del__` implementations and call these yourself when you see fit.

Another way assure proper cleanup of resources is to factor out GitPython into a
separate process which can be dropped periodically.

#### Windows support

See [Issue #525](https://github.com/gitpython-developers/GitPython/issues/525).

### RUNNING TESTS

_Important_: Right after cloning this repository, please be sure to have executed
the `./init-tests-after-clone.sh` script in the repository root. Otherwise
you will encounter test failures.

#### Install test dependencies

Ensure testing libraries are installed. This is taken care of already if you installed with:

```sh
pip install -e ".[test]"
```

If you had installed with a command like `pip install -e .` instead, you can still run
the above command to add the testing dependencies.

#### Test commands

To test, run:

```sh
pytest
```

To lint, and apply some linting fixes as well as automatic code formatting, run:

```sh
pre-commit run --all-files
```

This includes the linting and autoformatting done by Ruff, as well as some other checks.

To typecheck, run:

```sh
mypy
```

#### CI (and tox)

Style and formatting checks, and running tests on all the different supported Python versions, will be performed:

- Upon submitting a pull request.
- On each push, *if* you have a fork with GitHub Actions enabled.
- Locally, if you run [`tox`](https://tox.wiki/) (this skips any Python versions you don't have installed).

#### Configuration files

Specific tools are all configured in the `./pyproject.toml` file:

- `pytest` (test runner)
- `coverage.py` (code coverage)
- `ruff` (linter and formatter)
- `mypy` (type checker)

Orchestration tools:

- Configuration for `pre-commit` is in the `./.pre-commit-config.yaml` file.
- Configuration for `tox` is in `./tox.ini`.
- Configuration for GitHub Actions (CI) is in files inside `./.github/workflows/`.

### Contributions

Please have a look at the [contributions file][contributing].

### INFRASTRUCTURE

- [User Documentation](http://gitpython.readthedocs.org)
- [Questions and Answers](http://stackexchange.com/filters/167317/gitpython)
- Please post on Stack Overflow and use the `gitpython` tag
- [Issue Tracker](https://github.com/gitpython-developers/GitPython/issues)
  - Post reproducible bugs and feature requests as a new issue.
    Please be sure to provide the following information if posting bugs:
    - GitPython version (e.g. `import git; git.__version__`)
    - Python version (e.g. `python --version`)
    - The encountered stack-trace, if applicable
    - Enough information to allow reproducing the issue

### How to make a new release

1. Update/verify the **version** in the `VERSION` file.
2. Update/verify that the `doc/source/changes.rst` changelog file was updated. It should include a link to the forthcoming release page: `https://github.com/gitpython-developers/GitPython/releases/tag/<version>`
3. Commit everything.
4. Run `git tag -s <version>` to tag the version in Git.
5. _Optionally_ create and activate a [virtual environment](https://packaging.python.org/en/latest/guides/installing-using-pip-and-virtual-environments/#creating-a-virtual-environment). (Then the next step can install `build` and `twine`.)
6. Run `make release`.
7. Go to [GitHub Releases](https://github.com/gitpython-developers/GitPython/releases) and publish a new one with the recently pushed tag. Generate the changelog.

### Projects using GitPython

- [PyDriller](https://github.com/ishepard/pydriller)
- [Kivy Designer](https://github.com/kivy/kivy-designer)
- [Prowl](https://github.com/nettitude/Prowl)
- [Python Taint](https://github.com/python-security/pyt)
- [Buster](https://github.com/axitkhurana/buster)
- [git-ftp](https://github.com/ezyang/git-ftp)
- [Git-Pandas](https://github.com/wdm0006/git-pandas)
- [PyGitUp](https://github.com/msiemens/PyGitUp)
- [PyJFuzz](https://github.com/mseclab/PyJFuzz)
- [Loki](https://github.com/Neo23x0/Loki)
- [Omniwallet](https://github.com/OmniLayer/omniwallet)
- [GitViper](https://github.com/BeayemX/GitViper)
- [Git Gud](https://github.com/bthayer2365/git-gud)

### LICENSE

[3-Clause BSD License](https://opensource.org/license/bsd-3-clause/), also known as the New BSD License. See the [LICENSE file][license].

One file exclusively used for fuzz testing is subject to [a separate license, detailed here](./fuzzing/README.md#license).
This file is not included in the wheel or sdist packages published by the maintainers of GitPython.

[contributing]: https://github.com/gitpython-developers/GitPython/blob/main/CONTRIBUTING.md
[license]: https://github.com/gitpython-developers/GitPython/blob/main/LICENSE
