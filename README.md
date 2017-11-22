# Introduction

There is a readme.html which is converted from this markdown file for demonstration.

`CTCLoss.py`

- Pytorch implementation of Connectionist Temporal Classification (CTC) loss function by extending `torch.autograd`

`CNN_LSTM_FC_model.py`

- A Pytoch based CNN+LSTM+CTC model

`train.py`

- Trainig script

# Code

For the sake of convenience, a test shell script test.sh is provided for testing the pretrained model on test set. Simply run the following, and it will help set up the environment and fit the model automatically:

```shell
sh test.sh
```

## Environment Set Up
The running environment is set up in Python 2.7.10.
By running `virtualenv`, it could help set up the environment based on the `requirements.txt` easily:

```shell
# Create and activate new virtual environment
virtualenv venv
source venv/bin/activate

# Install requirements
pip install -r requirements.txt
```

## Code Description

## Network Architecture

The images are first processed by a CNN to extract features, then these extracted features are fed into a LSTM for character recognition.

The architecture of CNN is just Convolution + Batch Normalization + Leaky Relu + Max Pooling for simplicity, and the LSTM is a 2 layers stacked LSTM, you can also try out Bidirectional LSTM.

## Connectionist Temporal Classification (CTC)

### Forward & Backward

### Decoding

Given the probability distribution $P(l|x)$, we can compute a label $l$ for an input sequence $x$ by taking the most likely label. Thus, given that $L^{\leqslant T}$ is the set of sequences of length less than or equal to $T$ with letters drawn from the alphabet $L$, we can express our desired classifier h(x) as follows:

$$ h(x) = \arg \max_{\ell \in L^{\le T}} P(\ell | x) $$

Computing the most likely $l$ from the probability distribution $P(l|x)$ is known as decoding. However, given that the alphabet $L$ and the maximum sequence length $T$
 may be quite large, it is computationally intractable to examing every possible $L^{\leqslant T}$. There is no known algorithm to efficiently compute this h(x)
 precisely; however, there are several ways to approximate decoding which work well enough in practice.
Traditionally, decoding is done in one of two ways, which we discuss now. However, note that Graves et. al. do not end up using either of these ways, as they augment the CTC-style network in several ways, and find that a different decoding strategy works better in that context.