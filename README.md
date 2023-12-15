# Protein Interaction Prediction via Semantic Similarity of Gene Ontology Annotations and Neural Networks

## Hypothesis:

I proposed that by using the Gene Ontology (GO) terms associated with established interaction partners, we could predict interactions between a protein - given its GO terms - and a predefined target. The vectorized representation of these GO terms will be utilized to train a neural network, identifying distinctive patterns between interacting and non-interacting proteins.

## Methods:

In order to assess the hypothesis, I utilized the API of the STRING database. This API is capable of providing the interaction partners of a specified target, along with their confidence scores denoting the intensity of the interaction (in this study, I filtered for proteins having scores higher then 0.6). This filtration aimed to strike a balance between avoiding false interactions returned by STRING and gathering sufficient data for training the model. Additionally, STRING retains the GO terms linked to each protein through its functional annotation feature. Following the compilation of interaction partners and their respective GO terms, I categorized each term into three sub-groups: process, function, and component, aligning with the inherent categorization used by GO. To encode these terms for the neural network, I've adopted the HiG2Vec methodology introduced by Jaesik Kim et al (https://doi.org/10.1093/bioinformatics/btab193). Their approach employs a hierarchical representation of Gene Ontology (GO) vectors using Poincaré embedding, offering a 200D vector for each GO term. Following this conversion, I implemented a filtering stage to retain proteins that possess at least one term in each category. During subsequent testing, I omitted the "component" group due to its unexpectedly weak correlation in predicting interactions. Managing the varying number of terms per protein involved aligning the 200D vectors and executing a maxpooling operation. As a result, each protein, regardless of interaction status, could be represented by a 600D feature vector.

The goal is to perform binary classification by predicting whether a protein interacts with the defined target. I developed a basic multi-layer perceptron (MLP) architecture, comprising an input layer with 600 nodes (aligned with the input size), a hidden layer of 64 nodes, and an output neuron utilizing a sigmoid activation function. After shuffling the data and splitting the dataset into train, dev, test sets, I fit the model on the training set and plotted the accuracy, precision, recall, ROC curve for all three datasets. I tuned the hyperparameters (learning_rate, weight_decay) based on the dev set.

The integration with the STRING API allows us to instantly run the program for any desired protein. The limitation of this approach, is that only the most well studied proteins have enough known interaciton partners to train the model. Also, only a subset of proteins are annotated with GO terms, and even those can be erroneous, or incomplete.

## Results:

Evaluation metrics on a randomly generated feature vector of 600D:

Evaluation metrics on the protein BRCA1:

Train, dev, test set composition:

![image](https://github.com/tothp5991/protein-protein-interaction/assets/61978722/88b87924-c6a4-42c9-9af4-f1db435d67db)


ROC curve

![image](https://github.com/tothp5991/protein-protein-interaction/assets/61978722/4457453a-4162-4526-9ae9-890022abdaa8)


Accuracy

![image](https://github.com/tothp5991/protein-protein-interaction/assets/61978722/212ea7b2-4bba-41ec-a542-0b161d8be43c)

## Conclusion

Considering that protein-protein interactions are fundamentally physical, I lean toward structural features being more predictive in this scenario. The quantity of established interaction partners, the random selection of non-interacting proteins, and the diversity in their GO term counts significantly shape the outcomes highlighted in this brief overview. Nevertheless, the findings imply a potential link between the Gene Ontology Annotation, representing their functional traits, and the likelihood of interaction among certain proteins.
