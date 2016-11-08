This document answer some questions I encountered when working with Docker

* Swarm Mode:
   * Can we restrict service deploying to some nodes? Yes
   * Can we restrict number of task in any deployed nodes? No
* Docker Swarm
   * Can we restrict container deploying to some nodes? Yes
   * Can we restrict number of container deployed in any nodes? Yes

* Docker Compose with Docker Swarm
   * Can we restrict service deploying to some nodes? Yes
   * Can we restrict number of container deployed in any nodes? Yes

### Deploying Swarm Service to which node in Swarm Mode

* Nodes in Swarm Mode have some properties which can be used to group/identify them:
   * Engine labels
      * Engine labels are configured in dockerd, with multiple options `--label=${key}=${value}`
      * Some labels are created by dockerd: operatingsystem, 
      * Engine labels are referred in Swarm Mode by `engine.labels.${key}`
   * Node labels, node role, node ID, node hostname
      * Node role, ID and hostname are referred by `node.role`, `node.id`, `node.hostname`
      * Node labels are configured using command `docker node update --label-add ${key}=${value} --label-rm ${key}`
      * Node labels are referred by `node.labels.${key}`

* How to restrict deploying service to some nodes

Use constraints options when creating service

```
docker service create \
  --name redis \
  --constraint 'node.labels.type == queue' \
  redis:3.0.6
```

### Deploying Docker container to which node in Docker Swarm

* Node in Docker Swarm have some properties which can be used to restrict container deploying:
   * constraint: restrict container deploying by node labels:
      * Available labels: `node`, `storagedriver`, `executiondriver`, `kernelversion`, `operatingsystem`
      * Custom labels: are Engine labels which are introduced in previous section
      * constraint is applied by environment option `-e`, e.g:

      ```
      docker run -e constraint:storage==disk --name frontend nginx
      ```
   * health: do not deploy containers on unhealthy nodes
   * containerslots: restrict number of containers deployed in a node
      * containerslots is created by using Engine label `containerslots`, e.g. `--label=containerslots=3`

* Node in Docker Swarm can also use existed containers to restrict container deploying:
   * affinity: container can be deployed to some nodes which have container having specified name or ID, or have custom labels, or have some pulled images
      * Nodes have container having specified name or ID

      ```
      docker run -e affinity:container==${container_name} redis
      docker run -e affinity:container!=${container_name} redis
      docker run -e affinity:container==${container_id} redis
      docker run -e affinity:container!=${container_id} redis
      ```

      * Nodes have custome labels

      ```
      docker run -e affinity:${key}==${value} redis
      docker run -e affinity:${key}!=${value} redis
      ```

      * Nodes have some pulled images

      ```
      docker run -e affinity:image==${image_name} redis
      docker run -e affinity:image!=${image_name} redis
      docker run -e affinity:image==${image_id} redis
      docker run -e affinity:image!=${image_id} redis
      ```
   * dependency: containers have dependencies (shared volume, linking, shared network) can be deployed to same node. If we have multiple dependencies, Docker Swarm try to find nodes match all dependencies, so if any node match only few dependencies it will not be choosed.

   ```
   docker run --volumes-from=${shared_volume} redis
   docker run --link=${container}:${alias} redis
   docker run --net=container:${shared_net} redis
   ```

   * port
      * bridge mode: container have a publish port does not be deployed to a node having another container (which have same publish port)

      ```
docker run -d -p 80:80 nginx
docker run -d -p 80:80 nginx
docker ps
      CONTAINER ID        IMAGE          COMMAND        PORTS                           NAMES
963841b138d8        nginx:latest   "nginx"        192.168.0.43:80->80/tcp         node-2/dreamy_turing
87c4376856a8        nginx:latest   "nginx"        192.168.0.42:80->80/tcp         node-1/prickly_engelbart
      ```

      * host mode: container have a expose port does not be deployed to a node having another container (which have same expose port)

      ```
docker run -d --expose=80 --net=host nginx
docker run -d --expose=80 --net=host nginx
docker ps
CONTAINER ID        IMAGE               COMMAND                CREATED                  STATUS              PORTS               NAMES
7ecf562b1b3f        nginx:1             "nginx -g 'daemon of   Less than a second ago   Up 28 seconds                           node-2/ecstatic_meitner
09a92f582bc2        nginx:1             "nginx -g 'daemon of   46 seconds ago           Up 27 seconds                           node-1/mad_goldstine
      ```

* All above restriction are enabled by default, we can restrict to some restriction by Docker Swarm option `filter`

```
swarm manage --filter=health --filter=dependency
```

* constraint and affinity evaluated expression can be:
   * equals or matches: `${key}==${value}`, where `value` can be globbing pattern or regular expression
   * not equals or not matches: `${key}!=${value}` where `value` can be globbing pattern or regular expression

* if Docker Swarm can not find any node sastify constraint or affinity, Docker Swarm will not run container. But we can still let Docker Swarm run container by using tilde ~
```
docker run -e constraint:node==~/(?i)node1/ redis
```

### Deploying service of Docker Compose to which node in Docker Swarm

### References

* [Docker object labels](https://docs.docker.com/engine/userguide/labels-custom-metadata/)
* [Specify service constraints (–constraint)](https://docs.docker.com/engine/reference/commandline/service_create/#specify-service-constraints---constraint)
* [Swarm filters](https://docs.docker.com/swarm/scheduler/filter/)