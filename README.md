
<H1 ALIGN =CENTER> IMPLEMENTATION OF APPROXIMATE INFERENCE IN BAYESIAN NETWORKS </H1>
<H3> NAME :Manoj M </H3>
<H3> REGISTER NUMBER : 212221240027 </H3>
<H3> EXPERIMENT NO : 03 </H3>

## AIM: 
To construct a python program to implement approximate inference using Gibbs Sampling.</br>
## ALGORITHM:
### Step 1: Bayesian Network Definition and CPDs:<br>
   Define the Bayesian network structure using the BayesianNetwork class from pgmpy.models.
   Define Conditional Probability Distributions (CPDs) for each variable using the TabularCPD class.
   Add the CPDs to the network.
### Step 2: Printing Bayesian Network Structure:<br>
   Print the structure of the Bayesian network using the print(network) statement.
### Step 3: Graph Visualization:
   Import the necessary libraries (networkx and matplotlib).
   Create a directed graph using networkx.DiGraph().
   Define the nodes and edges of the graph.
   Add nodes and edges to the graph.
   Optionally, define positions for the nodes.
   Use nx.draw() to visualize the graph using matplotlib.
### Step 4: Gibbs Sampling and MCMC:<br>
   Initialize Gibbs Sampling for MCMC using the GibbsSampling class and provide the Bayesian network.
   Set the number of samples to be generated using num_samples.
### Step 5: Perform MCMC Sampling:<br>
   Use the sample() method of the GibbsSampling instance to perform MCMC sampling.
   Store the generated samples in the samples variable.
### Step 6: Approximate Probability Calculation:<br>
   Specify the variable for which you want to calculate the approximate probabilities (query_variable).
   Use .value_counts(normalize=True) on the samples of the query_variable to calculate approximate probabilities.
### Step 7: Print Approximate Probabilities:<br>
   Print the calculated approximate probabilities for the specified query_variable.


## PROGRAM:
```python
# Importing required libraries
from pgmpy.models import BayesianNetwork
from pgmpy.factors.discrete import TabularCPD
from pgmpy.sampling import GibbsSampling
import networkx as nx
import matplotlib.pyplot as plt

# Define bayesian network structure
network=BayesianNetwork([
    ('Burglary','Alarm'),
    ('Earthquake','Alarm'),
    ('Alarm','JohnCalls'),
    ('Alarm','MaryCalls')
])

# Define the conditional probability distributions
cpd_burglary = TabularCPD(variable='Burglary',variable_card=2,values=[[0.999],[0.001]])
cpd_earthquake = TabularCPD(variable='Earthquake',variable_card=2,values=[[0.998],[0.002]])
cpd_alarm = TabularCPD(variable ='Alarm',variable_card=2, values=[[0.999, 0.71, 0.06, 0.05],[0.001, 0.29, 0.94, 0.95]],evidence=['Burglary','Earthquake'],evidence_card=[2,2])
cpd_john_calls = TabularCPD(variable='JohnCalls',variable_card=2,values=[[0.95,0.1],[0.05,0.9]],evidence=['Alarm'],evidence_card=[2])
cpd_mary_calls = TabularCPD(variable='MaryCalls',variable_card=2,values=[[0.99,0.3],[0.01,0.7]],evidence=['Alarm'],evidence_card=[2])

# Add CPDs to the network
network.add_cpds(cpd_burglary,cpd_earthquake,cpd_alarm,cpd_john_calls,cpd_mary_calls)

# Print the Bayesian network structure
print("Bayesian Network Structure :")
print(network)

# Create a directed graph
G = nx.DiGraph()

# Define nodes and edges
nodes=['Burglary','Earthquake','Alarm','JohnCalls','MaryCalls']
edges=[('Burglary','Alarm'),('Earthquake','Alarm'),('Alarm','JohnCalls'),('Alarm','MaryCalls')]

# Add nodes and edges to the graph
G.add_nodes_from(nodes)
G.add_edges_from(edges)

# Set positions for nodes(optional)
pos ={
    'Burglary':(0,0),
    'Earthquake':(2,0),
    'Alarm':(1,-2),
    'JohnCalls':(0,-4),
    'MaryCalls':(2,-4)
}

# Draw the graph
nx.draw(G,pos,with_labels=True,node_size=5000,node_color='yellow',font_size=10,font_weight='bold',arrowsize=12)
plt.title("Bayesian Network: Alarm Problem")
plt.show()

# Initialize Gibbs sampling for MCMC
gibbs_sampler =GibbsSampling(network)

# Set the number of samples
num_samples=10000

# Perform MCMC sampling
samples=gibbs_sampler.sample(size=num_samples)

# Calculate approximate probabilities based on the samples
query_variable='Burglary'
query_result= samples[query_variable].value_counts(normalize=True)

# Print the approximate probabilities
print("\n Approximate Probabilities of {}".format(query_variable))
print(query_result)

```


## OUTPUT:
![output1](https://github.com/Shrruthilaya-Gangadaran/Ex-3--AAI/assets/93427705/b24554ce-32a8-42a5-8186-3de833e17490)

![output2](https://github.com/Shrruthilaya-Gangadaran/Ex-3--AAI/assets/93427705/c64e68ca-c7f7-44fb-ace2-3bb82712ae86)



## RESULT:
Thus, Gibb's Sampling( Approximate Inference method) is succuessfully implemented using python.
