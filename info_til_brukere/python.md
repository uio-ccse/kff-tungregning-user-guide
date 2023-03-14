# Python
A recent version of `Python 3` is always available on all nodes (at the moment of writing, version 3.8.10). Also, some popular Python packages are preinstalled on all nodes, including `numpy`, `scipy` and `matplotlib`. However, since it is impossible to preinstall all Python packages that the users may need, we encourage you to setup your own Python environment


## Setting up a Python environment 
There are several different Python environments available, typically with different applications. Some popular are

- **virtualenv (venv)** lets you create a basic environment. Integrated in the Python distribution
- **pyenv** lets you create an environment with a specific Python version (ex. 3.8.10)
- **Armadillo** external Python package manager
- **Poetry** external Python package manager

For the cluster, we recommend using `pyenv`.

### virtualenv
Create a new environment named "env":
```bash
python3 -m venv env
```
Activate environment:
```bash
source env/bin/activate
```
Now, packages can be installed directly on this local environment, ex. numpy:
```bash
pip install numpy
```
Deactivate environment:
```bash
deactivate
```

### pyenv
Run the following commands to install `pyenv-virtualenv`:
```
git clone https://github.com/pyenv/pyenv.git ~/.pyenv
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc
echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc
echo 'eval "$(pyenv init --path)"' >> ~/.bashrc
echo -e 'if command -v pyenv 1>/dev/null 2>&1; then\n eval "$(pyenv init -)"\nfi' >> ~/.bashrc
exec $SHELL
git clone https://github.com/pyenv/pyenv-virtualenv.git $(pyenv root)/plugins/pyenv-virtualenv
echo 'eval "$(pyenv virtualenv-init -)"' >> ~/.bashrc
exec $SHELL
```
Then, create a new virtual environment, with Python 3.8.10, for instance:
```
pyenv install 3.8.10
pyenv virtualenv 3.8.10 env
```
The upper command usually takes some time. Then, activate the environment:

```
pyenv activate env
```
Now, packages can be installed directly on this local environment, ex. numpy:
```bash
pip install numpy
```


### Anaconda and poetry
Anaconda and poetry are not installed yet, but can be set up on request.
