# Geometric Monitoring (GM)

Implementation and testing of the GM protocol with [SimulaQron](http://www.simulaqron.org/) v.3.0.10

## Getting Started

In this example we have N nodes where the root node is the only parent node (the coordinator) while all the others are child nodes (the producers).

Child nodes can suffer a local violation.
Each child node maintains its own global sub-state that communicates to the parent node only if it exceeds a pre-set threshold.

When a child node suffers a local violation, it communicates the variation of its local state to the parent node, which acquires also the variation of the other child nodes.
Subsequently, it calculates and averages all the local states acquired, and checks whether the threshold has been exceeded.

*Note: the current version has been tested with a maximum of 7 nodes*

### Prerequisites

* [SimulaQron](http://www.simulaqron.org/)

  Please refer to the [Getting started](https://softwarequtech.github.io/SimulaQron/html/GettingStarted.html) page of the SimulaQron guide for installation instructions.

* Python module [bitarray](https://pypi.org/project/bitarray/)
  ```
  pip install bitarray
  ```

### Setup

1. To change the setup of the network, edit the .simulaqron.json file in the testGM/ folder.
See: https://softwarequtech.github.io/SimulaQron/html/ConfNodes.html
The default configuration is:
```
{
    "backend": "projectq",
    "log-level": 10
    "max-qubits": 40
    "max-registers":100
    "recv-timeout":5.0
}

```


## Running

1. To start SimulaQron, open a first shell and execute:
   ```
   simulaqron start --nodes node0,node1,node2,node3,node4,node5,node6

   ```

2. Open a second shell, enter the testGM/ folder and execute:
   ```
   ./run.sh
   ```

### Settings

If you want to set up a different network of nodes you can change some parameters in the following files:
* In the *run.sh* file you can change the number of nodes, and specify for each node its name and the probability that it will suffer a local violation.
  Note: if you change the number of nodes you also need to update the value of the 'n' variable in the main function of the *gmnode.py* file.
* In the *gmnode.py* file you can change the value of the variable 'd' where d/2 represents the number of qubits used by each node.
* In the *gmnode.py* file you can change the value of the variable 't' if you want to change the threshold value.
* In the *gmnode.py* file you can change the value of the variable 'l' if you want to change the number of protocol executions.

### Logging

For each node a log file is created in the *log* folder and in the *log2* folder.

In the main folder there are two scripts:
- *localStatesAvg.py*: which uses the log files contained in the *log* folder to calculate the percentage error between the root node state and the local state average of all other nodes of the binary tree.
	The output is contained in the *results.txt* file.
- *bitCounter.py*: which uses the log files contained in the *log2* folder to calculate the total bits exchanged between each threshold overrun by the root node.
	The output is contained in the *results2.txt* file.

You can run these scripts by simply typing:
```
python localStatesAvg.py
python bitCounter.py
```

In the *G12* folder there is a file that contains the log of all times that the root node has updated the global state.

## License

Please refer to the [LICENSE](https://github.com/qis-unipr/QuantumNetworking/blob/master/LICENSE) file for details.
