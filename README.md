# Generating academic paper titles using an LSTM network: Project overview
### Introduction
To a layperson, the titles of academic papers often seem arcane and incomprehensible. I thought this would be a perfect content to use as the basis for a text generation project. 

This project showcases:
- **LSTM network**: As the basis of the text generator is a long short-term memory network, a type of recurrent neural network (RNN).
- **Deep Learning with PyTorch**: PyTorch is used to implement the model creation and training.
- **Regular expressions**: As part of the data cleaning and preparation, regular expressions are used in order to ensure the validity of input strings.

### Website blog post
https://www.nolaneley.com/portfolio/title-generation

### Code and Resources Used
**Python Version**: 3.11.4

**Packages**: PyTorch, pandas, numpy

### Data sources and cleaning
crossref.org makes available a huge corpus of metadata of academic publications. I downloaded the 2020 release since it was a modest 65 GB (compared to the 2022 release which was 160 GB). That includes metadata on over x number of publications. Since training a model on that much data requires significant computational time, I aimed for the minimum number of paper titles I could train a model on that would still yield convincing results. After some trial and error I landed on about 9,000 paper titles.

Only titles which included a set of accepted characters were chosen for training (A-Z, 0-9, : ; ' .). All titles were transformed into uppercase before training to reduce the number of unique characters included in the corpus. A stop character (+) was added at the end of each title then the titles were concatenated into one long string of over 600,000 characters.

### Model training and parameters
A LSTM network was used...
- 2 layers
- cross-entropy loss, etc.

Because training took so long (several days) it was difficult to optimize the parameters so I mostly modeled the parameters on the ones used in the model in this blog post: https://machinelearningmastery.com/text-generation-with-lstm-in-pytorch/

### Example output

