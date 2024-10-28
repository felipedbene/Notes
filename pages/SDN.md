- All networking starts with a node.
- External network ( that's you vlan )
- Each Node as has 2 Networks :
	- Cluster Net (10.99.x.x)
	- Pod NET(172.16.x.x)
- ``` yaml 
  # Flow 1: User -> DNS -> External LB -> NodePorts -> Pods
  
  +--------+       +---------+      +------------------+    +------------------+      +-------------+
  |  User  |-----> |  DNS    |----> | External LB      |--->|  NodePort        |----->|   Pods      |
  |        |       |         |      | (for NodePorts)  |    |  10.100.11.12    |      |   1 - 6     |
  +--------+       +---------+      +------------------+    |    32001 - 32006 |      +-------------+
                                                              |                |
                                                              +----------------+
  
  # Flow 2: User -> DNS -> Ingress -> Service (ClusterIP or NodePort) -> Pods
  
  +--------+       +---------+      +------------------+    +------------------+      +-------------+
  |  User  |-----> |  DNS    |----> | Ingress          |--->| Service          |----->|   Pods      |
  |        |       |         |      | (http/https only)|    |  (ClusterIP      |      |   1 - 6     |
  +--------+       +---------+      |                  |    |   or NodePort)   |      +-------------+
                                     +------------------+    +------------------+
  ```
-
- For a Node Perspective :
- ```Node
  # Kubernetes Node
  
  +---------------------------------------------------+
  |                                                   |
  |                  Node Components                  |
  |                                                   |
  |   +-------------------------------------------+   |
  |   |              NodePort Service             |   |
  |   |                                           |   |
  |   |       10.100.11.12:32001 - 32006          |   |
  |   +-------------------------------------------+   |
  |                                                   |
  |   +-------------------------------------------+   |
  |   |              ClusterIP Service            |   |
  |   |                                           |   |
  |   |           10.99.11.12 (internal)          |   |
  |   +-------------------------------------------+   |
  |                                                   |
  |   +----------------------+   +----------------+   |
  |   |     Pod 1           |   |     Pod 2      |    |
  |   | IP: 10.244.0.1      |   | IP: 10.244.0.2 |    |
  |   +----------------------+   +----------------+   |
  |                                                   |
  |   +----------------------+   +----------------+   |
  |   |     Pod 3           |   |     Pod 4      |    |
  |   | IP: 10.244.0.3      |   | IP: 10.244.0.4 |    |
  |   +----------------------+   +----------------+   |
  |                                                   |
  +---------------------------------------------------+
  ```
- Expose Objects
	- Cluster IP -> `Not reachable` from outside
	  logseq.order-list-type:: number
	- NodePort -> uses `NodeIP`:`32xxx` combination; it's port forwarded (D-NAT) to the POD Net ip.
	  logseq.order-list-type:: number
	- Typical cases :
		- (DNS) -> External LB -> NodePorts -> Pod(s) [Pod to Pod Comm]
		- (DNS) -> Ingress Controller (rules) -> Service ( ClusterIP or NodePort ) -> Pod
			- *** works for http/https *only***
			- Its a 'load balancer' and part of kubernetes natively.
-