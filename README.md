# so-jupyter
JupyterLab container prepared for interacting with Security Onion

## Get Manager IP
```
sudo salt-call pillar.get global:managerip | sed -n 2p | cut -d ' ' -f5
```
or
```
sudo salt '*_manager*' --out=json pillar.get global:managerip | jq '.[]' | cut -d '"' -f2
```

## Get Registry Host
```
sudo salt '*_manager*' --out=json pillar.get global:registry_host | jq '.[]' | cut -d '"' -f2
```

## Get Jupyter Lab Token
```
sudo docker exec -it  so-jupyter jupyter server list | sed -n 2p | cut -d '/' -f4 | cut -d ' ' -f1
```

## Build docker container
```
sudo docker buildx build --network=host -t so-jupyter:so-jupyter .
```

## Tag Image for local regisry
```
sudo docker image tag so-jupyter:so-jupyter securityonion-managersearch:5000/security-onion-solutions/so-jupyter
```

## Push Image to local registry
```
sudo docker image push securityonion-managersearch:5000/security-onion-solutions/so-jupyter
```

## Run local image with password
```
sudo docker run --rm -detached --name so-jupyter -p 8889:8888 -v /home/seconion/jupyter_lab:/home/jovyan/work:rw so-jupyter:so-jupyter start-notebook.py --ServerApp.allow_remote_access='True' --PasswordIdentityProvider.hashed_password='<password_hash_from_jupyter_server>'
```

## Run from registry with token
```
sudo docker run --rm -detached --name so-jupyter -p 8889:8888 --network=sobridge -v /home/seconion/jupyter_lab:/home/jovyan/work:rw securityonion-managersearch:5000/security-onion-solutions/so-jupyter start-notebook.py --ServerApp.allow_remote_access='True' --NotebookApp.token=$(uuidgen)```
```
