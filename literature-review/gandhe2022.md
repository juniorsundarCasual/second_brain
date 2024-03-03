---
id: gandhe2022
aliases: 
tags: 
"title:": "Decentralized Robot Swarm Clustering: Adding Resilience to Malicious Masquerade Attacks"
---
![[gandhe2022.pdf]]

# Objective
> [Page 1](Page 1) ... compare the resilience of four distributed robot swarm clustering algorithms to masquerade attacks launched from malicious robots within the swarm.
- Clustering algorithms are distributed variants of DBSCAN and k-Means.
- Modified such that each entity only has access to local communication and
  local distance measurements.
- These are then subjected to malicious masquerade attacks to see how
  performance is affected.

> [Page 1](Page 1) ... We then modify each variant to include a distributed Intrusion Detection and Response System (IDRS) to detect malicious robots and maintain the swarm’s integrity despite an attack.
- Tested in simulation and in hardware testbed with 25 Kilobot robot swarm.

> [Page 1](Page 1) ... We find that centralizing data within the swarm makes the swarm more vulnerable to malicious attacks, and that distributed IDRS relying on local message passing can effectively identify malicious robots and reduce their negative effects on swarm clustering performance.

# Introduction
> [Page 1](Page 1) ... Swarm algorithms are typically decentralized to eliminate single points of failure and distributed to leverage the sensors, actuators, and computation across many robots. The hardware swarms may contain many robots (10s, 100s, 1000s, etc.), and so robot swarm algorithms should be scalable, retaining functionality even if the number of robots in the swarm changes by orders of magnitude.

**Local Communication** - Involves message passing between neighbouring robots.
**Global Communication** - Allows message passing from any robot to any other robot.
**Clustering** - Process of dividing objects into groups based on like qualities, where objects with similar features are placed in the same cluster and objects with dissimilar features are placed into different clusters

> [Page 2](Page 2) ... Clustering a swarm’s robots into groups based on proximity helps analyze the swarm’s topology, and may serve as a pre-processing step for swarm behaviors such as sub-team formation, task division, geometric analysis, data aggregation, and information distribution.

> [Page 2](Page 2) ... the distributed process of clustering robots into groups presents an opportunity for a malicious robot to hijack the swarm. If a robot was tampered with before programming or captured and returned to the swarm, existing security such as encryption may not protect the swarm

**Masquerade Attacks** - An insider attack intended to disrupt the outcome of the algorithm.
> [Page 5](Page 5) ... Existing work defines a masquerade attack as an insider attack that generates fake data to target specific weakness of an algorithm

**Intrusion Detection and Response System (IDRS)** - IDS detects aversaries and IRS counteracts the effect of the adversaries. IDRS does both.
## Thesis
> [Page 3](Page 3) ... In this paper we adapt two clustering algorithms, **k-Means** and **DBSCAN (Density-Based Spatial Clustering of Applications with Noise)**, to suit a decentralized robot swarm operating in scenarios **without global** **communication and without global position data**. Next, we design masquerade attacks that take advantage of natural vulnerabilities in these algorithms. Finally, we create new variants of the algorithms that have IDRS designed to detect such attacks. The main contribution of this paper is an evaluation of whether or not—and to what extent—a decentralized IDRS can improve robot swarm clustering performance by detecting and neutralizing malicious robots.

# Related Works
> [Page 3](Page 3) ... our algorithms are implemented to suit swarm-specific constraints, including message dropping, finite bandwidth, restricted space overhead, and limited computational power.

- Swarm algorithms often focus on "graph distance" rather than the normal physical distance between robots.
- Graph distance means how many communication "hops" separate robots instead of the straight-line distance in space.
- This is useful for swarms because robots often lack global knowledge of the entire swarm's layout. They can easily measure how many messages get passed along, figuring out graph distance with only local information.

# Notation & Problem Statements
Robot swarm $S = {r_1, ..., r_n}$ contains n robots with unique IDs $1...n$.

$d_{ij}$ denotes the distance between $r_i$ and $r_j$

> [Page 4](Page 4) ... The distance between two neighboring robots can be determined without global positions, for example, by observing message time of flight or received signal strength.

Communication graph over swarm is $G= (V, E)$.

$V \equiv S = {r_1, ..., r_n}$

If $(r_i, r_j) \in E$ then the two robots can communicate with each other.

Imposing symmetry while not true in reality, therefore:
$(r_i, r_j) \in E \iff (r_j, r_i) \in E$

To emulate reality, $r_j$ drops message from $r_i$ when $d_{ij} > d_{max}$

Hence:
$((r_i, r_j) \in E \iff (r_j, r_i) \in E )\implies d_{ij} = d_{ji} < d$ for $d \leq d_{max}$

Let $E_d = \{(i, j) | d_{ij} \leq d\}$ be set of all edges of length $d$ or less. Define $G_d = (V, E_d)$. This guarantees:

$((r_i, r_j) \in E_d) \wedge (d \leq d_{max}) \implies ((r_j, r_i) \in E_d)$
## Problem Statements
### PS1
>  [Page 5](Page 5) ... 
>  **Distributed Swarm Clustering**: Given a swarm of n robots that communicate locally, the swarm must collectively divide itself into mutually exclusive clusters based on robot’s relative proximity such that robots within a particular cluster are closer to other robots in their own cluster then they are to robots in the other clusters.
### PS2
>  [Page 5](Page 5) ... 
> **Swarm Clustering Masquerade Attack**: Given a swarm of n robots that communicate locally and that is attempting to solve Problem 1, one or more malicious robot(s) must cause the swarm to find a lower quality solution (or prevent the swarm from finding any solution) by injecting incorrect data into the distributed algorithm–but not by simply disrupting communication. A notable element of Problem 2 is that the malicious robots seek to alter the outcome of the swarm algorithm instead of simply blocking communication
### PS3
> [Page 5](Page 5) ...
> **Resilient Swarm Clustering**: Given a swarm of n robots that communicate locally and that are solving Problem 1, as well as one or more malicious robots that are attempting to hack the solution by solving Problem 2, the swarm must identify and neutralize the malicious robots.

# Distributed Swarm DBSCAN, Attack, and IDRS
Original DBSCAN algorithm has two parameters:
- distance threshold d that determines whether two nodes are neighbours
- minimum number of neighbours m a node must have to be internal

Algorithm creates a d-disk graph, such that a particular node's neighbour set N has all nodes within d distance of that node.

Nodes are defined as internal if $|N| \geq m$ neighbours 
Nodes are defined as boundary if $m > |N| \geq 1$
Nodes are outlier if $|N| = 0$

> [Page 6](Page 6) ... DBSCAN defines that neighboring internal nodes are in the same cluster, boundary nodes are allowed to join any one of their neighbor’s clusters, and outlier nodes are not considered to be in any cluster.

> [Page 6](Page 6) ... For any physically defined communication radius $d_{max}$, it is possible to run distributed swarm DBSCAN for any $d \leq d_{max}$. After nodes (robots) have determined if they are internal, boundary, or outlier, the internal robots of each cluster run a distributed consensus algorithm to agree on a cluster ID. This is accomplished by having internal nodes iteratively exchange and average a cluster ID number with their neighboring internal nodes.

> [Page 6](Page 6) ... Each malicious robot advertises itself as an internal robot and then injects data designed to hijack the consensus algorithm (for example, repeatedly broadcasting $−\infty$ or 0). If two or more adversarial robots are located in different clusters, then each cluster containing an adversarial robot can be tricked to converge to the same label. The overall effect is that different clusters that should be considered unique erroneously believe that they are part of a single larger cluster.

Implemented IDRS by having each node maintain a list of malicious actors that don't change their values over multiple communication.

> [Page 7](Page 7) ... A main goal of our research is to evaluate the extent to which using a distributed IDRS can increase a swarm’s resilience to malicious attacks—assuming that malicious robots can be detected

# Distributed Swarm k-Means: Algorithm, Attack, IDRS
Designed for use with robot swarm that uses (only) local communication and local distance information.

Each k clusters associated with a cluster "root" robot - analogous to cluster centres used in k-Means.

Initial k root robots chosen randomly (for unsupervised clustering) or selected by user (semi-supervised). Cluster membership determined by closest root robot.

Algorithm is iterative at swarm level. In each iteration, two processes are run:
1. Determine current cluster membership of all robots
2. Transfer each cluster's root toward middle of that cluster
> [Page 9]() ... First, all nodes participate in a distributed calculation that provides each node with the size of its current sub-tree and those of its children. Next, each of the k cluster roots determines which of its own children has the largest sub-tree, and selects that child to become its cluster’s new root. This causes the cluster root to move one hop toward the cluster’s middle.

This hop is like gradient ascent. Makes sense if swarm only has local communication. Sometimes root location will settle into small cycles in the vicinity of local optima. Stopping criteria needed to stop looping.

User parameter $z_{max}$ defines maximum number of times a node may pass root responsibility to another robot.

Malicious robot tricks swarm by advertising it has a large sub-tree. Guaranteed to become root in next iteration.

Implemented IDRS by having each robot send IDS of all nodes in its sub-trees instead of only sub-tree size.
> [Page 10]() ... If two robots on different branches claim to have the same robot as part of their sub-tree, then the other robots add both (potentially malicious) robots to a malicious robot list. This reduces the damage a single malicious robot can cause.

> [Page 10]() ... This particular IDRS requires an increase in message size from $O(1)$ to $O(n)$, which may not be appropriate in all cases.

# Experiments
- In simulation and in hardware test bed
- Homogeneous swarm of 25 Kilobots
- Infrared LEDs and sensors to communicate within 100mm radius
- Convey information through RGB LED
## Silhouette Coefficient Performance Metric
> [Page 13]() ... The final silhouette coefficient, summarizing the clustering accuracy for all nodes, is the maximum of the $n$ silhouette values. If the whole swarm is classified as a single cluster, the silhouette coefficient is zero.

# Discussion of Results
> [Page 14]() ... We find that Distributed Swarm DBSCAN tends to produce more accurate clustering than the Distributed Swarm k-Means Algorithm.

> [Page 14]() ... Swarm k-Means, with its top-down tree structure, appears more vulnerable to disruption than the bottom-up DBSCAN. This reinforces the idea that potential for disruption by a malicious robot increases with centralization.

> [Page 15]() ... A single malicious robot has the potential to hijack all three roots, and so adding more adversarial robots did not increase its potential for disruption. In contrast, each additional malicious robot present during DBSCAN Algorithm’s caused a notable drop in accuracy and a greater variation in performance.

# Conclusion
> [Page 15]() ... We find that the Distributed Swarm k-Means Algorithm is more susceptible to attack the Distributed Swarm DBSCAN Algorithm. Results also show that increasing the number of adversarial robots caused larger disturbances in the DBSCAN Attack, while more adversaries had less effect on the k-Means Attack. Finally, we find that distributed IDRS can largely restore the swarm’s performance.
