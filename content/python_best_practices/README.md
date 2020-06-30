# 4. Python Usability Best Practices

## Making Best Use of Virtual Environment

Whenever we talk about developing different projects, we bounce back to same requirement which environment is best to work in? It becomes crucial when we want to make sure each and every developer have exactly the same environment. We know that our dependencies may change based on how we decide to publish or develop our software. 

Both management of system packages [Standard Python Version Library] and site packages [Third Party Libraries] are dependent on each other. For example, _Project A_ requires a certain version of Tensorflow(TFvA) and Keras(KvA) are supported by only a certain version of Numpy(NPvA) but in the same environment suddenly development starts for _Project B_ which requires Numpy version NPvB. This makes the environment unstable, and might breaks the funtionality and the environment for _Project A_. Hence developers should practice a healthy habit of virtual environment.

### What Is a Virtual Environment?
At its core, the main purpose of Python virtual environments is to create an isolated environment for Python projects. This means that each project can have its own dependencies, regardless of what dependencies every other project has. 

### Creation of Virtual Envrionment

The [venv](https://docs.python.org/3/library/venv.html) module provides support for creating lightweight “virtual environments” with their own site directories, optionally isolated from system site directories. Each virtual environment has its own Python binary (which matches the version of the binary that was used to create this environment) and can have its own independent set of installed Python packages in its site directories. 

You can create as many virtual environment as possible.

Creation of virtual environments is done by executing the command venv:
```bash
# Create New Virtual Environment
python3 -m venv /path/to/new/virtual/environment

# Activate Virtual Environment, if accessing via Terminal
source /path/to/new/virtual/environment/bin/activate
```

### Dependencies with IDE
IDEs can pointed to different virtual which can be used to develop different versions of project as well. `requirements.txt` file can keep track of all the site packages and their depencies to ensure stable similar env among different development or deploying environment. With IDEs, `requirements.txt` is auto-integrated to auto-update projects and its dependent site-pakcages.

There is an alternative for keeping track of all site-packages and their dependencies. Using `conda forge`. This ensures the `formula` we are using to forge library use the correct dependencies and site-packages all together. Use the [link](https://docs.conda.io/projects/conda/en/latest/user-guide/install/) to know more about it.

### Using Virtual Environment for Jupyter Notebook/Server:
One can also use multiple kernels while working on Jupyter Notebook or Server. Use the following command to add a new kernel:
```bash
python -m ipykernel install --user --name=/path/to/new/virtual/environment/bin/activate
```
This command helps in management of different POCs.

### What's happening under the hood?
Nothing rocket science. Whenever a python virtual environment is activated $PYTHON_PATH variable is updated for the enviroment and for the same site packages and system packages are picked up as binaries for reference.

### Deactivating and Removing Environments
To *deactivate* any active virtual python environment one need to write a very simple command:
```bash
deactivate
```
To *remove* the enviroment simply remove the directory.
