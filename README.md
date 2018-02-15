List the Components of Hadoop 2.x and explain each component in detail. 

The components of Hadoop 2.x are:

1) HDFS - Haddop Distributed File System

 HDFS is a distributed file system which stores the data in distributed manner. 
 Rather than storing a complete file it divides a file into small blocks of 128 MB size by default.
 and distributes them across the cluster. Each blocks is replicated 3 times as per default configuration 
 multiple times and is stored on different nodes to ensure data availability. 

 HDFS in Hadoop 2.x mainly has 3 daemons 
 which are 
 Name Node, 
 Secondary Name Node and 
 Data Node.

 a) Name Node

    - There is only single instance of this process runs on a cluster and that is on a master node
    - It is responsible for manage metadata about files distributed across the cluster
    - It manages information like location of file blocks across cluster and it’s permission
    - This process reads all the metadata from a file named fsimage and keeps it in memory
    - After this process is started, it updates metadata for newly added or removed files in RAM
    - It periodically writes the changes in one file called edits as edit logs
    - This process is a heart of HDFS, if it is down HDFS is not accessible any more

 b) Secondary Name Node

    - For this also, only single instance of this process runs on a cluster
    - This process can run on a master node (for smaller clusters) or can run on a separate node 
      (in larger clusters) depends on the size of the cluster
    - One misinterpretation from name is “This is a backup Name Node” but IT IS NOT!!!!!
    - It manages the metadata for the Name Node. In the sense, it reads the information written in edit logs (by Name Node) and creates an updated file of current cluster metadata
    - Then it transfers that file back to Name Node so that fsimage file can be updated
    - So, whenever Name Node daemon is restarted it can always find updated information in fsimage file

 c) Data Node

    - There are many instances of this process running on various slave nodes(referred as Data nodes)
    - It is responsible for storing the individual file blocks on the slave nodes in Hadoop cluster
    - Based on the replication factor, a single block is replicated in multiple slave nodes(only if 
      replication factor is > 1) to prevent the data loss
    - Whenever required, this process handles the access to a data block by communicating with Name Node
    - This process periodically sends heart bits to Name Node to make Name Node aware that slave process is running


2) MapReduce 
 
 Mapreduce is a programming model designed for processing large volumes of data in parallel by dividing the work into a 
 set of independent tasks. It Uses 3 YARN components: Resource Manager and Node Manager. These two components are responsible 
 for executing distributed data computation jobs in Hadoop 2

 a) Resource Manager

    - This daemon process runs on master node (may run on the same machine as name node for smaller clusters)
    - It is responsible for getting job submitted from client and schedule it on cluster, monitoring running 
      jobs on cluster and allocating proper resources on the slave node
    - It communicates with Node Manager daemon process on the slave node to track the resource utilization
    - It uses two other processes named Application Manager and Scheduler for MapReduce task and resource management
 
    a.1) Scheduler

    - This daemon process resides on the Master Node (runs along with ResourceManager daemon )
    - Responsible for,
        * Scheduling the job execution as per submission request received by ResourceManager
        * Allocating resources to applications submitted to the cluster
        * Coordinating with ApplicationManager daemon and keeping track of resources of running applications

    a.2) ApplicationManager

    - This daemon process resides on the Master Node (runs along with ResourceManager daemon )
    - Responsible for,
        * Helping Scheduler daemon to keeps track of running application by coordination
        * Accepting job submissions from client
        * Negotiating first container for executing application specific task with suitable ApplicationMaster on slave node


 b) Node Manager

    - This daemon process runs on slave nodes (normally on HDFS Data node machines)
    - It is responsible for coordinating with Resource Manager for task scheduling and tracking the resource utilization on the slave node
    - It also reports the resource utilization back to the Resource Manager
    - It uses other daemon process like Application Master and Container for MapReduce task scheduling and execution on the slave node

    b.1) ApplicationMaster

    - This daemon process runs on the slave node (along with the NodeManager daemon)
    - It is per application specific library works with NodeManager to execute the task
    - The instance of this daemon is per application, which means in case of multiple jobs submitted on cluster, it may 
      have more than one instances of ApplicationMaster on slave nodes
    - Responsible for,
        * Negotiating suitable resource containers on slave node from ResourceManager
        * Working with one or multiple NodeManagers to monitor task execution on slave nodes

    b.2) Container

    - Scheduler process running along with ResourceManager daemon allocates the resources as a container
    - At the beginning of a job execution with YARN, container allows ApplicationMaster process to make a 
      use of some resources on any slave node on the cluster
    - Then ApplicationMaster manages the application execution across other containers on slave nodes of a YARN cluster

