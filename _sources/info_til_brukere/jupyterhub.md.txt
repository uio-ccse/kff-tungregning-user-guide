# JupyterHub
JupyterHub is installed on `egil-analyze`, `neuroai` and `bioai`. For security reasons, it is only available through the cabeled network at CCSE. However, it is possible to set up a SSH tunnel. JupyterHub is set up to send and receive signals on port 8000. Suppose that port 8000 is available on your computer, set up a SSH tunnel through the port by running:
```
ssh -L 8000:localhost:8000 <brukernavn>@<egil-analyze>
```
JupyterHub can now be reached at `localhost:8000` from the browser.

The before you run JupyterHub for the first time, create a directory where the Python specifications are stored. This directory is read by jupyter:
```bash
mkdir  .local/share/jupyter/kernels
``` 

## Running JupyterHub from a Python environment
Suppose that a virtual environment with name "env" is already set up like explained in the guide on the previous page. Activate the environment and install `ipykernel`:
```
pyenv activate env
pip install ipykernel 
```

Then, register this environment to be used in JupyterHub
```
python -m ipykernel install --name "env" --display-name "My Env" --user
```

Now the environment is available in jupyterlab. To install pip-packages directly in jupyterlab, activate your environment in the jupyterlab terminal and install packages:

```
pyenv activate env
pip install numpy
``` 
