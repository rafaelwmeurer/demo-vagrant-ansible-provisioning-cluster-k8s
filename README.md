# Descomplicado o Ansible

Cluster Kubernetes utilizando Vagrant + VirtualBox + Ansible.

![enter image description here](https://churrops.files.wordpress.com/2018/05/ansiblevagrant.png)

# Como utilizar?

Existem alguns passos que precisam ser seguidos para que funcione corretamente a instalação dos cluster e os respectivos serviços configurados neste repositório.

## Primeiro passo

É necessário alterar o arquivo roles/main.yml

Comente as roles:
- install-monit-tools
- deploy-app-v1
- deploy-app-v2

```
- hosts: k8s-master
  become: yes
  user: vagrant
  roles:
  - { role: install-monit-tools, tags: ["install_monit-tools"] }

- hosts: k8s-master
  become: yes
  user: vagrant
  roles:
  - { role: deploy-app-v1, tags: ["deploy_app_v1_role"] }

- hosts: k8s-master
  become: yes
  user: vagrant
  roles:
  - { role: deploy-app-v2, tags: ["deploy_app_v2_role"] }
```

Agora sim, pode executar o comando, para provisionar as máquinas e o cluster de forma correta e sem erros

```
vagrant up
```

Finalizado o processo inicial de criação e provisionamento do cluster, acesse o nó master

```
vagrant ssh k8s-master
```

Execute o comando e será possível ver o resultado do cluster criado

```
kubectl get node
```

```
root@k8s-master:/home/vagrant# kubectl get node
NAME            STATUS   ROLES    AGE   VERSION
k8s-master      Ready    master   86m   v1.18.2
k8s-workers-1   Ready    <none>   83m   v1.18.2
k8s-workers-2   Ready    <none>   80m   v1.18.2
```

## Segundo passo

É necessário alterar novamente o arquivo roles/main.yml

Comente as roles:
- install-docker
- install-k8s
- create-cluster
- join-workers
- install-helm

```
  #roles:
  # - { role: install-docker, tags: ["install_docker_role"] }
  # - { role: install-k8s, tags: ["install_k8s_role"] }

#- hosts: k8s-master
# become: yes
# user: vagrant
# roles:
# - { role: create-cluster, tags: ["create_cluster_role"] }

#- hosts: k8s-workers
# become: yes
# user: vagrant
# roles:
# - { role: join-workers, tags: ["join_cluster_role"] }

#- hosts: k8s-master
# become: yes
# user: vagrant
# roles:
# - { role: install-helm, tags: ["install_helm_role"] }
```

Agora para fazer a instalação do prometheus-operator de corretamente, assim como o giropops, pode ser executado o comando

```
vagrant provision
```

Como isto, por fim teremos tudo instalado corretamente

```
root@k8s-master:/home/vagrant# kubectl get pods
NAME                                                        READY   STATUS    RESTARTS   AGE
alertmanager-meu-querido-prometheus-pro-alertmanager-0      2/2     Running   2          87m
giropops-v2-9995d7758-5tgrv                                 1/1     Running   1          85m
giropops-v2-9995d7758-9cqs2                                 1/1     Running   1          85m
giropops-v2-9995d7758-9vvjc                                 1/1     Running   1          85m
giropops-v2-9995d7758-cg7fz                                 1/1     Running   1          85m
giropops-v2-9995d7758-cgnzf                                 1/1     Running   1          85m
giropops-v2-9995d7758-gs8hb                                 1/1     Running   1          85m
giropops-v2-9995d7758-h5n2b                                 1/1     Running   1          85m
giropops-v2-9995d7758-kjwbx                                 1/1     Running   1          85m
giropops-v2-9995d7758-pgrcg                                 1/1     Running   1          85m
giropops-v2-9995d7758-vvxs7                                 1/1     Running   1          85m
meu-querido-prometheus-grafana-74bf9f84cf-tvhh5             2/2     Running   2          87m
meu-querido-prometheus-kube-state-metrics-f96d9d9c8-8hpst   1/1     Running   1          87m
meu-querido-prometheus-pro-operator-7498cdb79b-vhl6n        2/2     Running   2          87m
meu-querido-prometheus-prometheus-node-exporter-7cb9v       1/1     Running   1          87m
meu-querido-prometheus-prometheus-node-exporter-kjpvh       1/1     Running   2          87m
meu-querido-prometheus-prometheus-node-exporter-kwl7f       1/1     Running   1          87m
prometheus-meu-querido-prometheus-pro-prometheus-0          3/3     Running   4          87m
```

## Próximos passos

Para inserir novos aplicativos assim como os que foram inseridos no passo 2, basta deixar todas as roles comentadas e inserir apenas as novas referente as novas aplicações e executaro o comando

```
vagrant provision
```

## Complemento

Pode-se fazer isto de outra forma, utilizado o arquivo hosts, que é comum na utilização do ansible.