# Code repository accompanying NAACL 2019 tutorial on "Transfer Learning in Natural Language Processing"

The tutorial will be given on June 2 at NAACL 2019 in Minneapolis, MN, USA by Sebastian Ruder, Matthew Peters, Swabha Swayamdipta and Thomas Wolf.

## Abstract

The classic supervised machine learning paradigm is based on learning in isolation, a single predictive model for a task using a single dataset. This approach requires a large number of training examples and performs best for well-defined and narrow tasks. Transfer learning refers to a set of methods that extend this approach by leveraging data from additional domains or tasks to train a model with better generalization properties.

Over the last two years, the field of Natural Language Processing (NLP) has witnessed the emergence of several transfer learning methods and architectures which significantly improved upon the state-of-the-art on a wide range of NLP tasks.

These improvements together with the wide availability and ease of integration of these methods are reminiscent of the factors that led to the success of pretrained word embeddings and ImageNet pretraining in computer vision, and indicate that these methods will likely become a common tool in the NLP landscape as well as an important research direction.

We will present an overview of modern transfer learning methods in NLP, how models are pre-trained, what information the representations they learn capture, and review examples and case studies on how these models can be integrated and adapted in downstream NLP tasks.

Here is the [webpage](https://naacl2019.org/program/tutorials/) of NAACL tutorials.

## Codebase

This codebase try to present in the simplest and shortest possible way a few of the major Transfer Learning technics which have emerged over the past years. The code in this repository doesn't pretend to be state-of-the-art even though effort has been made to ensure reasonnable performances and possible upgrade to the current state-of-the-art level.

Special effort has been made to

- ensure the present code can be use as straightforwardly as possible, in particular by hosting pretrained models and datasets,
- keep the present clodebase as compact and self-contained as possible to make it easy to manipulate and understand.

Currently the codebase comprises:

- a transformer model with a GPT-2-like architecture as the basic pretrained model,
- a pretraining script to train this model with a language modeling objective on a selection of large datasets (wikitext-103, simplebooks-92) using distributed training if available,
- several architectures based on the transformer model for fine-tuning (with a classification head on top, with adapters),
- a fine-tuning script to fine-tune these architectures on a classification task (imdb).

## Installation

To be able to use this codebase, simply clone the Github repository and install the requirements like this:

```bash
git clone https://github.com/huggingface/naacl_transfer_learning_tutorial
cd https://github.com/huggingface/naacl_transfer_learning_tutorial
pip install -r requirements.txt
```

## Pre-training

To pre-train the transformer, run the `pretraining_train.py` script like this:

```bash
python ./pretraining_train.py
```

or using distributed training like this (for a 8 GPU server):

```bash
python - torch.distributed.launch --nproc_per_node 8 ./pretraining_train.py
```

The pre-training script will:

- download `wikitext-103` for pre-training (default),
- log the experiements in Tensorboard and in a folder under `./runs`,
- save checkpoints in the log folder.

## Fine-tuning

To fine-tune a pre-trained the transformer, run the `finetuning_train.py` script like this:

```bash
python ./finetuning_train.py --model_checkpoint PATH-TO-YOUR-PRETRAINED-MODEL-FOLDER
```

`PATH-TO-YOUR-PRETRAINED-MODEL-FOLDER` can be for instance `./runs/May17_17-47-12_my_big_server`

or using distributed training like this (for a 8 GPU server):

```bash
python - torch.distributed.launch --nproc_per_node 8 ./finetuning_train.py  --model_checkpoint PATH-TO-YOUR-PRETRAINED-MODEL-FOLDER
```
