### Week 3
### Basic Models
#### Sequence to Sequence Model
- This is good for French to English translations
- There is a encoder network and decoder network for these translations.
#### Image Captioning
- There were multiple models coming out about the same time for this kind of task.
### Picking the Most Likely Sentence
#### Machine Translation as Building a Conditional Language Model
- Machine translation model is very similar to the Language Model except instead of starting out with a vector of zeros it instead has an encoded network that figures out some representation for the input sentence.
  - And it takes that input sentence instead of the representation of all zeros.
- ![alt text](image.png)
- And it is called the "Conditional Language Model" by Andrew Ng since the input sentence is fed into the network as the input?
#### Why not a greedy search?
- Greedy search is a computer science algorithm that says to generate the first word pick whatever is the most likely first word according to your conditional language model. Then pick the second best word and so on..
- going is a common french word so you may get a translation like "Jane is going to be visiting Africa in September" instead of "Jane is visiting Africa is September". This is kind of a hand wavy explanation.
- Its not always optimal to pick one word at a time.
- So we go with Beam search.
- The set of all english sentences is too large to enumerate through. Beam search will have a solution!
### Beam Search Algorithm
- Whereas Greedy Search will pick only one most likely wordd and move on, Beam Search can consider multiple alternatives.
  - Beam Search has a parameter called B which is called the beam width and it means that Beam search will consider three possibilities at the same time.
  - This still does not deal with the problem of searching all possible English sentences.
  - say you have a input French sentence, you run that input through the encoder network.
  - Then in the first step you decode the network and this is a softmax probability of 10,000 (where does this number come from?)
- ![alt text](image-2.png)
    - There are three copies of the network with different starting words from **step 1**
- ![alt text](image-3.png)
### Refinements to Beam Search
#### Length Normalization
- ![alt text](image-4.png)
  - I think that first formula is multiplying all of the numbers and the second one is summation.
  - Use log to avoid numerical underflow issues.
  - You can do the sum of logs of the probabilities instead of the product of probabilities
- Because the log is a strictly monotonically increasing function, we know that maximizing log p of y given x should give you the same result as maximizing p of y given x.
- A heuristic or a hack is what people found works best through trial and error.
#### Beam search discussion
- How do you set the Beam width B?
  - large B: better result, slower
  - small B: worse result, faster
- Unlike exact search algorithms, Beam Search runs faster but is not guaranteed to find exact maximum for arg max P (Y | x).
### Error Analysis in Beam Search
- Figure out whether its your Beam Search that is causing problems or the RNN that is causing problems.
  - I guess we are using both an RNN and Beam Search algorithm somehow. I wonder what this architecture is called.
  - The RNN model is really a encoder and decoder.
  - Compute the P( y_goal | x) and P(y_pred | x) you can compare the probalities here and decide whether its the RNN or Beam Search that is causing issues. So what are the steps behind this?
    - If the y_goal example has a higher prob then Beam Search is at fault. If y_pred has a bigger probabilty then the RNN is at fault.
#### Error Analysis Process
- Go through the Human vs the algorithm predicitons. Figure out what fraction of errors are "due to" beam search vs. RNN model.
### Bleu Score (Optional)
- Stopped here