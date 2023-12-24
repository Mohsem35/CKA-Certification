### Cluster Networking

Unlike in the docker world were an IP address is always assigned to a Docker CONTAINER,
in Kubernetes the IP address is assigned to a POD.

Each POD in kubernetes gets its own internal IP Address. In this case its in the range 10.244 series

_How is it getting this IP address?_

When **Kubernetes is initially configured** it creates an **internal private network** with the address **10.244.0.0** and all PODs are attached to it.

- When you deploy **multiple PODs**, they all get a **separate IP assigned**

But **accessing** other PODs using this internal IP address **MAY not be a good idea** as its subject to **change when PODs are recreated**

_For now its important to understand how the internal networking works in kubernetes?_ 

আমার কাছে যদি multiple nodes নিয়ে cluster থাকে এবং প্রতিটা node এর pods গুলো যদি same IP পায় তাহলে সেইটা একটা problem

When a **kubernetes cluster is SETUP**, kubernetes **does NOT automatically setup any kind of networking** to handle these issues. As a matter of fact, **kubernetes expects US to setup networking** to meet certain fundamental requirements


- All the **containers/PODs** in a kubernetes cluster MUST be able to **communicate with one another without** having to configure **NAT**

- **All nodes** can **communicate with all containers** and vice-versa without NAT in the cluster

Kubernetes expects US to setup a networking solution that meets these criteria.

Fortunately, we don’t have to set it up ALL on our own as there are multiple **pre-built solutions available**.

Depending on the platform you are deploying your Kubernetes cluster on you may use any of these solutions

- If we were setting up a **kubernetes cluster from scratch** on your own systems, you may use any of these solutions like `Calico`, `Flannel` etc.
- Deploying on a **Vmware environment** `NSX-T` may be a good option
- Look at the play-with-**k8s labs** they use `WeaveNet`.