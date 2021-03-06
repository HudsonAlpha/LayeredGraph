# LayeredGraph
Methods for building layered graphs and running graph traversal algorithms

## Software Requirements
1. Python - developed and tested using Python 3.6.2.
2. Python packages - numpy, scipy

## LayeredGraph Data Structure
### Purpose
The main purpose of this repository is to store a LayeredGraph (LG) data structure that is stored in LayeredGraph.py.  The LG is a directed graph composed of "layers" of nodes that are connected in different way.
For example, one might have a layer for storing a meal ontology and then a layer for storing an ingredients ontology.  Some meals are related to each other (e.g. a hamburger and a cheeseburger)
and some ingredients will similarly be related (e.g. cucumber and pickle).  Also, there are connections between the layers such as a meal being composed of specific ingredients.  Once
all this information is present in a layered, algorithms can be run over the graph to identify closely related things.  For example, someone might have bread and turkey, the appropriate
graph algorithm might help them realize that adding cheese will create a turkey and cheese sandwich.

### Building a LayeredGraph for Random Walk with Restart (RWR)
1. Create a graph and load all nodes into the graph.
```python
#instantiate a graph
import LayeredGraph
lg = LayeredGraph.LayeredGraph()
#the first value is the layer, second is the node label
lg.addNode('ingredient', 'cheese')
...
lg.addNode('meal', 'sandwich')
#call this only when all nodes have been added
lg.finalizeNodeList()
```
2. Add all edges to the graph.
```python
#values are layer1, node1, layer2, node2, edge weight, and whether the edge is undirected
lg.addEdge('ingredient', 'cheese', 'meal', 'sandwich', 1, True)
...
lg.addEdge('ingredient', 'turkey', 'meal', 'turkey sandwich', 1, True)
lg.addEdge('meal', 'sandwich', 'meal', 'turkey sandwich', 1, True)
```
3. Build the graph transition matrix.  The first line describes how different layers interact during random walk, the second actually generates the matrix necessary for random walk.
```python
#sets transition rates from one layer to another as equal
lg.setGraphJumpEqual()
#calculates the final transition probabilities for a given LayeredGraph
lg.calculateTransitionMatrix()
```
4. Run the RWR algorithm.  This example will return a ranking of all 'meal' nodes in the graph based on the RWR algorithm.
```python
#define the RWR parameters; we have cheese and bread and want to know what meals are closest related to those ingredients
startNodes={('ingredient', 'cheese'):1.0, ('ingredient', 'bread'):1.0}
restartProb=0.1
targetLayers=set(['meal'])
lg.RWR_rank(startNodes, restartProb, targetLayers)
```

## Constructing HPO and PPI Graphs
Currently, ```PyxisMapBuilder.py``` and ```ProtBuilder.py``` contain the two sets of functions that build and save the two pre-constructed graphs associated
with PyxisMap.  Currently, this file is hard-coded to datasets that have been downloaded and processed on a particular machine.  There are several normalization and weighting decisions built
into the uploaded HPO to gene graph that are discussed in this document.  For more information, refer to the source code or email jholt@hudsonalpha.org.

