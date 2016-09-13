# Kubernetes

Endgame is to get all these things into Kubernetes.  Instead of just running on a local docker installation this enables the container to be orchestrated.

### `make k8s`

On line 167 I give an example of how a kubernetes yaml file can be generated:
https://github.com/joshuacox/local-nginx/blob/stretch/Makefile#L167

```

k8s: k8deploy k8svc

k8svc: local-nginx-svc.yaml
	kubectl create -f local-nginx-svc.yaml

k8deploy: local-nginx-deploy.yaml
    kubectl create -f local-nginx-deploy.yaml
```

the two yaml files were generated with:

```
local-nginx-svc.yaml: NGINX_DATADIR REGISTRY REGISTRY_PORT TAG NAME
	$(eval NGINX_DATADIR := $(shell cat NGINX_DATADIR))
	$(eval REGISTRY := $(shell cat REGISTRY))
	$(eval REGISTRY_PORT := $(shell cat REGISTRY_PORT))
	$(eval TAG := $(shell cat TAG))
	$(eval NAME := $(shell cat NAME))
	cp -i templates/local-nginx-svc.template local-nginx-svc.yaml
	sed -i "s!REPLACEME_DATADIR!$(NGINX_DATADIR)!g" local-nginx-svc.yaml
	sed -i "s/REPLACEME_REGISTRY/$(REGISTRY)/g" local-nginx-svc.yaml
	sed -i "s/REPLACEME_PORT_OF_REGISTRY/$(REGISTRY_PORT)/g" local-nginx-svc.yaml
	sed -i "s!REPLACEME_TAG!$(TAG)!g" local-nginx-svc.yaml
	sed -i "s!REPLACEME_NAME!$(NAME)!g" local-nginx-svc.yaml

local-nginx-deploy.yaml: NGINX_DATADIR REGISTRY REGISTRY_PORT TAG NAME
	$(eval NGINX_DATADIR := $(shell cat NGINX_DATADIR))
	$(eval REGISTRY := $(shell cat REGISTRY))
	$(eval REGISTRY_PORT := $(shell cat REGISTRY_PORT))
	$(eval TAG := $(shell cat TAG))
	$(eval NAME := $(shell cat NAME))
	cp -i templates/local-nginx-deploy.template local-nginx-deploy.yaml
	sed -i "s!REPLACEME_DATADIR!$(NGINX_DATADIR)!g" local-nginx-deploy.yaml
	sed -i "s/REPLACEME_REGISTRY/$(REGISTRY)/g" local-nginx-deploy.yaml
	sed -i "s/REPLACEME_PORT_OF_REGISTRY/$(REGISTRY_PORT)/g" local-nginx-deploy.yaml
	sed -i "s!REPLACEME_TAG!$(TAG)!g" local-nginx-deploy.yaml
sed -i "s!REPLACEME_NAME!$(NAME)!g" local-nginx-deploy.yaml
```

the templates themselves can be seen [here](https://github.com/joshuacox/local-nginx/tree/stretch/templates)


