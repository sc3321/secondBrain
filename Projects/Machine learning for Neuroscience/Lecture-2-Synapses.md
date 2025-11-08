Neuron structure continued:

Saltatory conduction: Neurons are insulated with fatty in sequential nodes called myelin sheaths. These fatty chunks insulate the neuron and allow the signal to be transmitted along the neuron effectively. This is important as the signal needs to reach the "axon terminal" to transmit to other neurons.


Chemical synapses: These are actually not "gaps" as portrayed in diagrams. When the action potential reaches the axon terminal, it causes Ca2+ ionsopen and enter the neuron terminal. This infusion triggers the vesicle- which is a "bag of neurotransmitters" to fuse with the neuron membrane and release the neurotransmitters across the synaptic cleft. These neurotransmitters bind to the "post-synaptic neuron" and starts a subsequent action potential (if of sufficient size).

Many different kinds of neurotransmitters. Dale's principle states that a neuron will transmit the exact same set of transmitters at all of its synapses which is different to ANNs. Upon exploration, making ANN's behave Dale's principle leads us to only matching the ANN error rate, so why they follow this principle remains an open question. 

These synapses can be modelled by varying the concentration of neurostransmitters, vesicle concentrations etc. all to get more accurate models of the synapse junction.

Hebbian Learning principle:

	"When an axon of cell A is near enough to excite a cell B and repeatedly or persistently takes part in firing it, some growth process or metabolic change takes place in one or both cells such that A’s efficiency, as one of the cells firing B, is increased.”

This describes long term plasticity but is equally relevant to short term plasticity that is caused by local dynamic changes to neurotransmitter, Ca2+ and vesicle concentrations among many other things leading to local synapse facilitation and depression effects.


Fundamentally, the rest of the course explores the question of what a network of neurons can do that a single neuron cant do. This is all to do with neural networks and decision making etc.

The idea is that neural networks and neurons are aiming to be "Universal Function Approximators". There are many examples of different functions, but biologically things like sound orientation identification, balance etc. are what are considered. Some key points:

	- Tuning curves & Coincidence detection. A tuning curve in neuroscience is the response of a neuron, averaged over many repetitions, to a stimulus defined by one or more variables. Spike time tuning curves are explored, where the number of spikes(response) are recorded when the time between inputs is varied(stimulus).

	- Spatial structure explores how biological changes over time and learning happens where different neurons will become sensitive to particular inputs and this differential response is reflected in literal spatial positioning of the neurons. The mice whisker example is noted where whiskers on the right side trigger responses in neurons concentrated in a different part of the mice brain. This is a form of decision making, as the mice will learn to record spikes in that particular region as originating from stimuli in its right whiskers. 

	- Encoding & Decoding: Lot of clever ways to encode and decode information. There are many models which try to reflect this. Using perceptron based networks, bayesian approximations etc.
	
	- Some cool population coding theories on how to go from using the tuning curves of neurons of a population to deduce an inputs. Its all just fancy probabilistic models about using population variation, averages etc. to get to probabilistic answer of maximum likelihood of what the exact stimulus was. No need to worry about details.








