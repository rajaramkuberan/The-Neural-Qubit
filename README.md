# The-Neural-Qubit

## Overview

This is the related code for the research paper & Youtube video by Siraj Raval titled "The Neural Qubit". This model is a hybrid classical-quantum classifier, with a number of input classical layers that control the parameters of an input layer in a two-mode continous-variable quantum neural network. The model is trained so that it outputs a photon in one mode for a genuine credit card transaction, and outputs a photon in the other mode for a fraudulent transaction. The classifier has
an area under the ROC curve of 0.953, compared to the optimal value of 1. This is on a simulated quantum environment and classical algorithms can outperform it. I intuit that on a physical quantum computer, the ROC could outperform classical algorithms, but more research is needed to confirm that. XanaduAI had a great proof of concept in this direction, I built off of their variational circuit code by modifying the fock basis truncation, adding an extra photonic quantum layer, and increasing the intial gate parameter seed. Next, i want to try to re-do this code but on a real quantum computer, using [D-Wave's quantum API](https://www.dwavesys.com/take-leap)

Full paper [here](https://sirajraval.com/wp-content/uploads/2019/09/The-Neural-Qubit.pdf)

## Dependencies
- Tensorflow 1.3
- Strawberry Fields
- numpy

## Installation

Download the [Strawberry Fields](https://github.com/XanaduAI/strawberryfields) library. This code uses the Strawberry Fields Tensorflow backend. Right now, Strawberry Fields on supports Tensorflow 1.3. And Tensorflow 1.3 only supports versions of Python less than 3.7. So make sure you have those two, and the code will run fine. More specific installation details are on the Strawberry fields README page. 

## Usage

#### Step 1 - Acquire data

Download the [Credit Card Fraud Detection](https://www.kaggle.com/mlg-ulb/creditcardfraud) dataset from Kaggle. Extract the `creditcard.csv` file & place it in this folder. 

#### Step 2 - Data preprocessing

Run the data processor script to split the data into training & testing sets

```bash
python3 data_processor.py
```

#### Step 3 - Train the model & monitor progress

Run the training loop using the fraud detection script. This script can take several hours to complete on a typical PC. It's quite computationally expensive to simulate quantum dynamics on a classical computer. This is why we need quantum computers. 

```bash
python3 fraud_detection.py
```

Since the model is periodically saved during training, you can monitor progress by launching TensorBoard in terminal. `simulation_label` is the name used to refer to a particular run of the script `fraud_detection.py` (this is specified within the file itself; the default is `1`).

```bash
tensorboard --logdir=outputs/tensorboard/simulation_label
```

<img align="center" src="https://github.com/XanaduAI/quantum-neural-networks/blob/master/static/fraud_detection.png" width=300px>

#### Step 4 - Test the model

First, edit `testing.py` to point to the simulation label and checkpoint of the model which is to be tested. These are specified under the variables `simulation_label` and `ckpt_val` in `testing.py`.

Next, test the model by running the testing script

```bash
python3 testing.py
```
The output of testing is a confusion table, which can be found as a numpy array in `outputs/confusion/simulation_label`. The confusion table is given for multiple threshold probabilities for a transaction to be considered as genuine.

#### Step 5 - Visualize the results

The performance of the trained model can be investigated with:
```bash
python3 roc.py
```
which outputs the receiver operating characteristic (ROC) curve and confusion matrix for the optimal threshold probability.


## Credits

XanaduAI! Keep pushing neural network theory into the thrilling world of quantum information processing. Also D-Wave, for believing in me. Dr. Matthew Fisher thank you for your work on quantum cogniton as well. Max Tegmark, thank you for helping me learn about the arguments against quantum cognition. And Guillame Verdon, thank you for being my Quantum advisor & friend at Google X. 
