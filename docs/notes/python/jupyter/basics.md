# Jupyter

## Notebook

...Reference Lab for essentially the same behavior

> Source: [ljvmiranda921](https://ljvmiranda921.github.io/notebook/2018/01/31/running-a-jupyter-notebook/)

This one is very simple but I know I got the below one to work too...

## Lab

### Run jupyter remotely from container

> Source: [benjlindsay](https://benjlindsay.com/posts/running-jupyter-lab-remotely)


To run Jupyter Lab on a login node, you need to open 2 terminal windows. In the first window:

Note to self, when doing this for containers, run this remembering we are mapping the container port ```5678``` to server port ```5678``` with ```-p 5678:5678```. Example command:
```bash
docker run -it --ipc=host --name=ben_pytorch_tutorial -p 5678:5678 --runtime=nvidia -v /home/bb927/Documents/pytorch:/workspace pytorch/pytorch:latest bash
```

> Note, additional step to install jupyter could be required ```$ pip install jupyterlab```. (You'll need a container with python in it)

Then inside the container start jupyter:

```bash
$ jupyter lab --ip 0.0.0.0 --port 5678 --no-browser --allow-root

```
and see it running...

```bash
[I 22:05:13.843 LabApp] Writing notebook server cookie secret to /root/.local/share/jupyter/runtime/notebook_cookie_secret
[I 22:05:14.047 LabApp] JupyterLab extension loaded from /opt/conda/lib/python3.7/site-packages/jupyterlab
[I 22:05:14.047 LabApp] JupyterLab application directory is /opt/conda/share/jupyter/lab
[I 22:05:14.050 LabApp] Serving notebooks from local directory: /workspace
[I 22:05:14.050 LabApp] The Jupyter Notebook is running at:
[I 22:05:14.050 LabApp] http://353e1fa49ecd:5678/?token=XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
[I 22:05:14.050 LabApp]  or http://127.0.0.1:5678/?token=XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
[I 22:05:14.050 LabApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
[C 22:05:14.053 LabApp] 
    
    To access the notebook, open this file in a browser:
        file:///root/.local/share/jupyter/runtime/nbserver-45-open.html
    Or copy and paste one of these URLs:
        http://353e1fa49ecd:5678/?token=XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
     or http://127.0.0.1:5678/?token=XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
```

Then in the second terminal window run:

```bash
$ ssh -CNL localhost:5678:localhost:5678 username@hostname
```

Finally copy the link from the jupyter lab step into your browser (http://127.0.0.1:5678/?token=XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX).

### Run jupyter remotely from server

Log into server and start jupyter:

```bash
$ ssh username@hostname
$ jupyter lab --no-browser --port=5678
...
[I 10:17:14.160 LabApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
[C 10:17:14.160 LabApp]

    Copy/paste this URL into your browser when you connect for the first time,
    to login with a token:
        http://localhost:5678/?token=XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
```

Then in the second window (local machine) run:

```bash
$ ssh -CNL localhost:5678:localhost:5678 username@hostname
```

You should be able to go to http://localhost:5678 to see jupyter lab now.