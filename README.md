# Generating academic paper titles using a neural network: Project overview
### Introduction
To a layperson, the titles of academic papers often seem arcane and incomprehensible. I thought this would be the perfect content to use as the basis for a text generation project. 

This project showcases:
- **LSTM network**: As the basis of the text generator is a long short-term memory network, a type of recurrent neural network (RNN).
- **Deep Learning with PyTorch**: PyTorch is used to implement the model creation and training.
- **Regular expressions**: As part of the data cleaning and preparation, regular expressions are used in order to ensure the validity of input strings.

### Website blog post
https://www.nolaneley.com/portfolio/title-generation

### Code and Resources Used
**Python Version**: 3.11.4

**Packages**: PyTorch, pandas, numpy

### Files
1. data_wrangling - loads raw json metadata files, filters titles, concatenates into single string with stop characters, and saves
2. model_training - loads clean data and encodes it, creates and trains LSTM model, saves best model
3. text_gen - uses random prompts to generate full titles from trained model

### Data sources and cleaning
A huge corpus of metadata of academic publications has been made available on crossref.org. I downloaded the 2020 release since it was a modest 65 GB (compared to the 2022 release which was 160 GB). That includes metadata on over 112 million publications. Since training a model on that much data requires significant computational time, I aimed for the minimum number of paper titles I could train a model on that would still yield somewhat convincing results. After some trial and error I landed on about 8,600 paper titles.

Only titles which included a set of accepted characters were chosen for training (A-Z, 0-9, : ; ' .). All titles were transformed into uppercase before training to reduce the number of unique characters included in the corpus. A stop character (+) was added at the end of each title then the titles were concatenated into one long string of over 400,000 characters.

### Model training and parameters
My model was based in large part on the model in this blog post: https://machinelearningmastery.com/text-generation-with-lstm-in-pytorch/

An LSTM network was used (torch.nn.LSTM) with:
- 256 hidden units
- 2 layers
- Dropout rate of 0.2
- Cross-entropy loss function
- Adam optimization

The model was trained for 40 epochs. Because training took so long (>50 hours) I didn't do much experimenting with optimizing these parameters.


### Text generation
In order to generate a title, we need a few characters as a starting point. These were chosen by taking the first n characters of a randomly chosen title. The text was generated until it reached a maximum length (180 characters) or the stop character (+).

### Example output
Here are some example titles with various values for n (the prompt length).

**n = 11**
- OPTIMIZED SESUM ALBUMIN AND COMPUTER SIMULATION OF THE STRUCTURE
- INNOVATION IN THE CONPLEX OF THE STRUCTURE

**n = 13**
- AESTHETIC SURGICAL PROCESSES
- THERMOREGULATION OF THE STRUCTURE

**n = 15**
- THE TREATMENT OF AN INTERACTION OF THE SURGICAL PATHOLOGY
- LIPOPROTEIN ANA COMPUTER SYSTEMS

**n = 17**
- SYPHILIS OF THE NERROUP OF THE PROTEINS OF THE SURGICAL PROBESSES
- ADVANCES IN CARDIAC CELLS IN THE PRESENCE OF AN INTEGRATED PROTEINS OF THE SELATIONSHIP

**n = 19**
- THE GEOMETRY OF NONMINEAR SYSTEMS
- INTEGRATED INTELLIGENT CONTROL OF A COMPUTER SYSTEM FOR THE METABOLISM OF THE SELATIONSHIP OF THE PROTEINS OF THE SURGICAL PROBESSES

**n = 21**
- NAD(P)H AND F420 FLUORIDE AND COMPUTER SYSTEMS
- ULTRASONOGRAPHY IN OBTIENTS WITH PERTING PROTEINS

### Impressions
The generated titles tend to overuse certain terms (e.g. "surgical probesses" and "of the structure"). It's a little difficult to say for certain what is causing this problem but it could be improved by one or more of the following:
- Having more/better data - Including more titles or filtering out certain titles to have a more coherent dataset.
- Longer training (more epochs) - After 40 epochs, the cross-entropy loss was, for the most part, still decreasing, indicating the model had not yet converged and may benefit from additional training
- Higher dropout rate - The model might be overfitting to some patterns in the dataset. Increasing the dropout rate could help with this, though it might slow convergence.

Overall, I'm pretty happy with the results. I would like to experiment more to improve the results but as training took over 50 hours on my 2017 MacBook Pro and I don't have access to a GPU, it's a big time commitment to test out the suggestions above.
