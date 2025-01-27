---
reviewers:
- lavalamp
title: Компоненти Kubernetes
content_type: concept
description: >
  Кластер Kubernetes складається з компонентів, що представляють площину управління
  і набір машин (machines), які називаються nodes.
weight: 20
card: 
  name: concepts
  weight: 20
---

<!--
---
reviewers:
- lavalamp
title: Kubernetes Components
content_type: concept
description: >
  A Kubernetes cluster consists of the components that represent the control plane
  and a set of machines called nodes.
weight: 20
card: 
  name: concepts
  weight: 20
---
-->

<!-- overview -->
<!--When you deploy Kubernetes, you get a cluster.
{{< glossary_definition term_id="cluster" length="all" prepend="A Kubernetes cluster consists of">}}
-->
Коли ви розгортаєте Kubernetes, ви отримуєте кластер. 

<!--A Kubernetes cluster consists of a set of worker machines, called nodes, 
that run containerized applications. Every cluster has at least one worker node.
-->
Кластер Kubernetes складаєтсья з набору робочих машин, які звуться nodes 
і які запускають контейнеризовані застосунки. Кожен кластер містить принаймні один робочий node. 

<!--The worker node(s) host the Pods that are the components of the application workload. 
The control plane manages the worker nodes and the Pods in the cluster. In production 
environments, the control plane usually runs across multiple computers and a cluster 
usually runs multiple nodes, providing fault-tolerance and high availability.
-->
На робочих node(s) розміщуютсья Pods, які є компонентами робочого навантаження застосунку. 
Площина управління керує робочими nodes і Pods в кластері. На проді 
площина управління зазвичай запускається на багатьох комп'ютерах і в одному кластері
зазвичай запускається багато nodes, надаючи відмовостійкість і високий рівень доступності. 

<!--This document outlines the various components you need to have for
a complete and working Kubernetes cluster.
-->
Цей документ описує різні компоненти, які вам знадобляться для повноцінно працюючого кластеру Kubernetes. 

<!--{{< figure src="/images/docs/components-of-kubernetes.svg" alt="Components of Kubernetes" caption="The components of a Kubernetes cluster" class="diagram-large" >}}
-->
{{< figure src="/images/docs/components-of-kubernetes.svg" alt="Компоненти Kubernetes" caption="Компоненти кластеру Kubernetes" class="diagram-large" >}}

<!-- body -->
<!--## Control Plane Components
-->
## Компоненти площини управління

<!--The control plane's components make global decisions about the cluster (for example, scheduling), as well as detecting and responding to cluster events (for example, starting up a new {{< glossary_tooltip text="pod" term_id="pod">}} when a deployment's `replicas` field is unsatisfied).
-->
Компоненти площини управління відповідають за кластер (наприклад, за розподілення (scheduling)), а також за виявлення і реагування на події кластера (наприклад, запуск нового {{< glossary_tooltip text="pod" term_id="pod">}} коли значення поля розгортання `replicas` є незадовільним). 

<!--Control plane components can be run on any machine in the cluster. However,
for simplicity, set up scripts typically start all control plane components on
the same machine, and do not run user containers on this machine. See
[Creating Highly Available clusters with kubeadm](/docs/setup/production-environment/tools/kubeadm/high-availability/)
for an example control plane setup that runs across multiple machines.
-->
Компоненти площини управління можуть бути запущені на будь-якій машині кластеру. Тим не менш, 
задля простоти скрипти налаштування зазвичай запускають всі компоненти площини управління на
одній і тій самій машині і не запускають контейнери користувача на цій машині. Дивіться приклад 
як налаштувати компоненти площини управління, щоб запускати їх на багатьох машинах, тут 
[Creating Highly Available clusters with kubeadm](/docs/setup/production-environment/tools/kubeadm/high-availability/). 

### kube-apiserver

{{< glossary_definition term_id="kube-apiserver" length="all" >}}

### etcd

{{< glossary_definition term_id="etcd" length="all" >}}

### kube-scheduler

{{< glossary_definition term_id="kube-scheduler" length="all" >}}

### kube-controller-manager

{{< glossary_definition term_id="kube-controller-manager" length="all" >}}

<!--Some types of these controllers are:
-->
Деякі типи таких контролерів:

<!--  * Node controller: Responsible for noticing and responding when nodes go down.-->
  * Node контролер: відповідає за виявлення і реагування на вимикання nodes.
<!--  * Job controller: Watches for Job objects that represent one-off tasks, then creates
    Pods to run those tasks to completion.-->
  * Job контролер: слідкує за Job об'єктами, які представляють єдиноразові задачі, створює
    Pods щоб виконувати такі задачі.
<!--  * Endpoints controller: Populates the Endpoints object (that is, joins Services & Pods).-->
  * Endpoints контролер: заповнює Endpoints об'єкт (який, в свою чергу, об'єднує Services і Pods).
<!--  * Service Account & Token controllers: Create default accounts and API access tokens for new namespaces.-->
  * Service Account і Token контролери: створюють аккаунти і токени доступу API за замовченням для нових просторів імен (namespaces).

### cloud-controller-manager

{{< glossary_definition term_id="cloud-controller-manager" length="short" >}}

<!--The cloud-controller-manager only runs controllers that are specific to your cloud provider.
If you are running Kubernetes on your own premises, or in a learning environment inside your
own PC, the cluster does not have a cloud controller manager.
-->
cloud-controller-manager запускає тільки ті контролери, які є специфічними для вашого хмарного провайдера. 
Якщо ви запускаєте Kubernetes у власному або навчальному оточенні на вашому 
ПК, то кластер не буде мати cloud-controller-manager. 

<!--As with the kube-controller-manager, the cloud-controller-manager combines several logically
independent control loops into a single binary that you run as a single process. You can
scale horizontally (run more than one copy) to improve performance or to help tolerate failures.
-->
Так само як і kube-controller-manager, cloud-controller-manager поєднує в собі кілька логічно 
незалежних процесів управління в один двійковий файл, який ви запускаєте як єдиний процес. Ви можете 
горизонтально розширювати це (запускаючи більше одної копії), щоб підвищити продуктивність або відмовостійкість. 

<!--The following controllers can have cloud provider dependencies:-->
Наступні контролери можуть мати залежності від хмарних провайдерів: 

<!--  * Node controller: For checking the cloud provider to determine if a node has been deleted in the cloud after it stops responding-->
  * Node контролер: щоб перевіряти чи може хмарний провайдер визначити чи був видалений node в хмарі після того, як перестав відповідати. 
<!--  * Route controller: For setting up routes in the underlying cloud infrastructure-->
  * Route контролер: щоб встановлювати шляхи в середині поточної хмарної інфраструктури. 
<!--  * Service controller: For creating, updating and deleting cloud provider load balancers-->
  * Service контролер: щоб створювати, оновлювати ти видаляти балансувальники навантаження (load balancers) хмарного провайдера. 

## Node Components

Node components run on every node, maintaining running pods and providing the Kubernetes runtime environment.

### kubelet

{{< glossary_definition term_id="kubelet" length="all" >}}

### kube-proxy

{{< glossary_definition term_id="kube-proxy" length="all" >}}

### Container runtime

{{< glossary_definition term_id="container-runtime" length="all" >}}

## Addons

Addons use Kubernetes resources ({{< glossary_tooltip term_id="daemonset" >}},
{{< glossary_tooltip term_id="deployment" >}}, etc)
to implement cluster features. Because these are providing cluster-level features, namespaced resources
for addons belong within the `kube-system` namespace.

Selected addons are described below; for an extended list of available addons, please
see [Addons](/docs/concepts/cluster-administration/addons/).

### DNS

While the other addons are not strictly required, all Kubernetes clusters should have [cluster DNS](/docs/concepts/services-networking/dns-pod-service/), as many examples rely on it.

Cluster DNS is a DNS server, in addition to the other DNS server(s) in your environment, which serves DNS records for Kubernetes services.

Containers started by Kubernetes automatically include this DNS server in their DNS searches.

### Web UI (Dashboard)

[Dashboard](/docs/tasks/access-application-cluster/web-ui-dashboard/) is a general purpose, web-based UI for Kubernetes clusters. It allows users to manage and troubleshoot applications running in the cluster, as well as the cluster itself.

### Container Resource Monitoring

[Container Resource Monitoring](/docs/tasks/debug/debug-cluster/resource-usage-monitoring/) records generic time-series metrics
about containers in a central database, and provides a UI for browsing that data.

### Cluster-level Logging

A [cluster-level logging](/docs/concepts/cluster-administration/logging/) mechanism is responsible for
saving container logs to a central log store with search/browsing interface.


## {{% heading "whatsnext" %}}

* Learn about [Nodes](/docs/concepts/architecture/nodes/)
* Learn about [Controllers](/docs/concepts/architecture/controller/)
* Learn about [kube-scheduler](/docs/concepts/scheduling-eviction/kube-scheduler/)
* Read etcd's official [documentation](https://etcd.io/docs/)
