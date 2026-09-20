# Wordpiece_tokenization

## Overview

This project demonstrates the basic working of WordPiece Tokenization using Python.

WordPiece is a subword tokenization method commonly used in Natural Language Processing and Transformer-based language models. Instead of treating every complete word as a separate token, WordPiece can divide words into smaller meaningful pieces.

In this project, a small corpus is used to understand how WordPiece tokenization works step by step.

## Corpus

The input corpus is stored in `corpus.txt`.

The corpus contains the following words:

​```text
play
playing
played
​```

These words are selected because they share the common part `play` and have different endings.

## Project Files

​```text
WordPiece-Tokenization/
│
├── Word_piece.ipynb
├── corpus.txt
└── README.md
​```

### Word_piece.ipynb

This Jupyter Notebook contains the complete Python implementation of the WordPiece tokenization process.

### corpus.txt

This file contains the input words used for building and testing the tokenizer.

### README.md

This file explains the project and how it works.

## How the Program Works

The project follows these main steps:

### 1. Read the Corpus

The program reads the words from `corpus.txt` and stores them as a list.

​```text
play
playing
played
​```

### 2. Create Initial Tokens

Initially, each word is split into individual characters.

​```text
play     -> p l a y
playing  -> p l a y i n g
played   -> p l a y e d
​```

### 3. Calculate Token Frequencies

The program counts how many times each character appears in the complete corpus.

For example:

​```text
p : 3
l : 3
a : 3
y : 3
i : 1
n : 1
g : 1
e : 1
d : 1
​```

### 4. Calculate Pair Frequencies

The program finds consecutive token pairs and counts their occurrences.

Example:

​```text
(p, l) : 3
(l, a) : 3
(a, y) : 3
(y, i) : 1
(i, n) : 1
(n, g) : 1
(y, e) : 1
(e, d) : 1
​```

### 5. Calculate WordPiece Scores

A score is calculated for each pair using the frequency of the pair and the frequencies of its individual tokens.

$$ \text{Score}(x,y) = \frac{\text{Frequency}(x,y)}{\text{Frequency}(x) \times \text{Frequency}(y)} $$

The calculated scores are:

​```text
(p, l) : 0.3333
(l, a) : 0.3333
(a, y) : 0.3333
(y, i) : 0.3333
(i, n) : 1.0
(n, g) : 1.0
(y, e) : 0.3333
(e, d) : 1.0
​```

The pair with the highest score is selected for merging.

In this example, three pairs tie for the highest score — the first one encountered is chosen:

​```text
Best pair: ('i', 'n')
Best score: 1.0
​```

Successive merge rounds continue in the same way — combining the highest-scoring remaining pairs — until every word is reduced to its shared prefix plus a suffix piece.

### 6. Build the Vocabulary

The final vocabulary used for tokenization contains:

​```text
play
##ing
##ed
​```

The `##` notation indicates that the token is a continuation of a previous token.

For example:

​```text
playing -> play + ##ing
played  -> play + ##ed
​```

### 7. WordPiece Tokenization

The tokenizer searches for the longest matching token from the vocabulary.

The resulting tokenization is:

​```text
play     -> ['play']
playing  -> ['play', '##ing']
played   -> ['play', '##ed']
​```

This shows how the common word part `play` can be reused for different words.

### 8. Assign Token IDs

Special tokens and vocabulary tokens are assigned numerical IDs.

​```text
[PAD]  -> 0
[UNK]  -> 1
[CLS]  -> 2
[SEP]  -> 3
play   -> 4
##ing  -> 5
##ed   -> 6
​```

The tokenizer can therefore convert the words into numerical token IDs.

​```text
play
Tokens: ['play']
IDs: [4]

playing
Tokens: ['play', '##ing']
IDs: [4, 5]

played
Tokens: ['play', '##ed']
IDs: [4, 6]
​```

## Special Tokens

The project uses four special tokens:

| Token   | Purpose                                        |
| ------- | ----------------------------------------------- |
| `[PAD]` | Used for padding sequences                     |
| `[UNK]` | Represents an unknown token                    |
| `[CLS]` | Represents the beginning of a sequence         |
| `[SEP]` | Represents the end or separation of a sequence |

## Final Encoding

The `encode()` function adds `[CLS]` at the beginning and `[SEP]` at the end of the token sequence.

The final results are:

​```text
play
Tokens: ['[CLS]', 'play', '[SEP]']
IDs: [2, 4, 3]

playing
Tokens: ['[CLS]', 'play', '##ing', '[SEP]']
IDs: [2, 4, 5, 3]

played
Tokens: ['[CLS]', 'play', '##ed', '[SEP]']
IDs: [2, 4, 6, 3]
​```

## Technologies Used

* Python
* Jupyter Notebook
* NumPy
* Python Collections (`Counter`)

## Requirements

Install the required Python libraries using:

​```bash
pip install numpy
​```

Jupyter Notebook or JupyterLab can be used to run the `.ipynb` file.

## How to Run

1. Download or clone this repository.
2. Make sure `corpus.txt` and `Word_piece.ipynb` are in the same folder.
3. Open `Word_piece.ipynb` using Jupyter Notebook or JupyterLab.
4. Run the cells in order.
5. The notebook will display the token frequencies, pair frequencies, WordPiece scores, vocabulary, token IDs, and final encoded representation.

## Example

For the input:

​```text
playing
​```

The tokenizer produces:

​```text
Tokens: ['[CLS]', 'play', '##ing', '[SEP]']
IDs: [2, 4, 5, 3]
​```

For the input:

​```text
played
​```

The tokenizer produces:

​```text
Tokens: ['[CLS]', 'play', '##ed', '[SEP]']
IDs: [2, 4, 6, 3]
​```

## Objective

The main objective of this project is to understand the basic concept and implementation of WordPiece tokenization.

The project demonstrates how words can be converted into subword tokens and then into numerical IDs that can be used as input for Natural Language Processing models.

## Conclusion

This project provides a simple implementation of WordPiece tokenization using a small corpus. It demonstrates the complete flow from reading the corpus and creating character-level tokens to calculating pair scores, creating a vocabulary, generating subword tokens, and converting them into token IDs.

The example with `play`, `playing`, and `played` shows how a common word part can be represented as one token while the different endings are represented as separate subword tokens.

## Author

**Doddi Hasini**
