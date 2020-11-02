# The Galaxy Aces Discord Bot Helm Chart

* * *

This helm chart is used to install your own discord bot based upon [The Galaxy Aces Discord Bot](https://github.com/The-Galaxy-Aces/Discord-Bot)

## Requirements

* * *

This char is built using [helm v3](https://helm.sh/) so you will need to be using at least helm v3. You will also need your own kubernetes cluster up and running, which if you are using helm, you should already have.

## Required Configuration

* * *

You will need a valid config.yaml which you can read more about [here](<>) and you can use the [default.yaml here](https://raw.githubusercontent.com/The-Galaxy-Aces/Discord-Bot/main/defaults.yaml) as a base to create your own.

Once ready, copy your config.yaml into 

```sh
tga helm chart directory/config
```

The helm chart will look in this directory, attempt to find your config.yaml, and load it into a configmap called tgabot-config

This configmap will then be mounted to /TGA-Bot/config.yaml where the application expects it to be.

## Values

* * *

Most of values.yaml will be fine with their defaults, however if you plan to use the local music playback feature, you will need to configure the paths so kubernetes can mount the volume to the pod and the bot can find the music.

In the below example the bot configuration in your config.yaml local_path must match the local_path in the values.yaml. The example is also using a hostPath mount meaning the node which is running the pod will be expected to have a location 'host_mount.path' avaiable locally which will be the directory music is stored. You can also use other volume types supported by your k8s cluster, you can view more details on those [here](https://kubernetes.io/docs/concepts/storage/volumes/).

```yaml
music:
  local:
    enabled: True
    local_path: /music
    host_mount:
      enabled: True
      path: /media/music
```

For example, if using the above configuration and you have three nodes in your cluster, all of which the pod could be deployed to. Each node will need to have directory at /media/music which contains the music you want the bot to have access to. Instead of having copies of all the music on each node, you could create a network share at that location on each node and point them all to the same share. Alternatively you can also use other solutions such as Cephfs which will require some additional configuration on your end.

## Install

* * *

From the The Galaxy Aces helm chart directory

```sh
helm install tgabot .

```
