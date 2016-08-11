This application simulates a version of the distributed, asynchronous Bellman -Ford algorithm that determines the shortest paths between nodes 
in a router network. Each client maps to a localhost address with an attached port number as instructed on the command line.

Clients maintain and exchange multiple tables of data with each others to build their networks. The most common data structure is a thread-safe 
mapping of Nodes and Paths, custom classes that represent entries in the routing table:

For instance, ConcurrentHashMap distance contains the client's current estimated distance for all reachable nodes in the network.
Clients exchange messages by serializing these objects into byte arrays, and passing them through DatagramPacket Sockes. The objects are 
recovered on the other side of the socket and interpretted in one of three ways: route update messages, linkdown messages, and linkup messages.
Linkdown messages are indicated with negative cost values and linkup messages are inferred by Clients who recognize a prior neighbor's address.

The algorithm performs as expected and does not experience errors other than those that occur naturally from the Bellman-Ford algorithm.
For instance, the counting to infinity problem can occur in the following scenario:

Node 1 to Node 2 = 1
Node 1 to Node 3 = 1
Node 2 to Node 3 = 100
If Node 1 is removed from the network (either by timeout or by linkdowns), Nodes 2 and 3 will count to infinity.

USER COMMANDS
SHOWRT - Displays the client's current routing table. Syntax - showrt or SHOWRT
LINKDOWN - destroy a link between two neighboring nodes. Syntax - linkdown IP:port. For example, linkdown 192.168.63.1:4115 (NOTE- I have
put a colon between IP and port). 
LINKUP - restore a link with a prior neighboring node. Syntax - linkup 192.168.63.1:4115
CLOSE - top the client's operation. Syntax - close or CLOSE

EXTRA FEATURES

First of all, one extra feature I have implemented is to make the commands case insensitive. For example, the program will treat 'showrt'
and 'SHOWRT' as same.

I have also implemented a host of other extra commands. Some of them are-

neighbors - display the client's neighbors and their costs
network - list all nodes that the client is aware of
nd - display all neighbors' distance vectors currently held
timers - display the number of seconds elapsed on each neighbor's timeout timer

INSTRUCTIONS TO COMPILE AND RUN

NOTE - I have named my file Client rather than bfclient. I don't like the name bfclient!!

Enter the project director

~/$ cd ~/.../PA3

Compile project

~/.../PA3$ make

Start a client by specifying its port number, timeout,
and any number of {localhost port weight} neighbors.

For example - java Client 4115 10 192.168.63.1 4116 5.0 

This should run.


