# so-jupyter
JupyterLab container prepared for interacting with Security Onion

## Get Manager IP:
```console
sudo salt-call pillar.get global:managerip | sed -n 2p | cut -d ' ' -f5
```
## Get Jupyter Lab Token:
```console
sudo docker exec -it  so-jupyter jupyter server list | sed -n 2p | cut -d '/' -f4 | cut -d ' ' -f1
```

## Build docker container:
```console
sudo docker buildx build --network=host -t so-jupyter:so-jupyter .
```

## Tag Image for local regisry:
```console
sudo docker image tag so-jupyter:so-jupyter securityonion-managersearch:5000/security-onion-solutions/so-jupyter
```

## Push Image to local registry:
```console
sudo docker image push securityonion-managersearch:5000/security-onion-solutions/so-jupyter
```

## Run local image with password
```console
sudo docker run --rm -detached --name so-jupyter -p 8889:8888 -v /home/seconion/jupyter_lab:/home/jovyan/work:rw so-jupyter:so-jupyter start-notebook.py --ServerApp.allow_remote_access='True' --PasswordIdentityProvider.hashed_password='argon2:$argon2id$v=19$m=10240,t=10,p=8$rUpZrusSSaw0zLRTj0hYAw$vFZ3/Fg9yq0KBFtmWoHKZ+hSszGl9ue8lJt74iF9RJI'
```

## Run from registry with token:
```console
sudo docker run --rm -detached --name so-jupyter -p 8889:8888 --network=sobridge -v /home/seconion/jupyter_lab:/home/jovyan/work:rw securityonion-managersearch:5000/security-onion-solutions/so-jupyter start-notebook.py --ServerApp.allow_remote_access='True' --NotebookApp.token=$(uuidgen)```
```
