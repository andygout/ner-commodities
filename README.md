# ner-commodities

Experiment to create a Named Entity Recognition (NER) model to identify commodities in FT content.

It runs on [Jupyter Notebook](https://jupyter-notebook-beginner-guide.readthedocs.io/en/latest/what_is_jupyter.html) and uses modules from the [spaCy](https://spacy.io) library: _an open-source software library for advanced natural language processing, written in the programming languages Python and Cython_


## Setup
- Clone this repo and make a root-level directory called `data` that includes the folllowing files:
  - [`ft-articles-training.txt`](#dataft-articles-trainingtxt)
  - [`ft-articles-evaluation.txt`](#dataft-articles-evaluationtxt)
  - [`ft-articles-test.txt`](#dataft-articles-testtxt)
- Install the Jupyter Notebook App by downloading [Anaconda Distribution](https://www.anaconda.com/products/distribution), which is a common scientific python distribution (and which also includes scientific python packages)
- Run (from the root-level of this repo): `$ jupyter notebook` - this wil open the notebook on [http://localhost:8888/tree](http://localhost:8888/tree)


## Creating rules
- Click the `create-rules.ipynb` file to open this kernel on [http://localhost:8888/notebooks/create-rules.ipynb](http://localhost:8888/notebooks/create-rules.ipynb)
- Run each of the cells in order from top to bottom, which will create the `commodities_ner_rules` directory that is employed by subsequent kernels


## Creating entities per article dataset
- Click the `create-entities-per-article-data.ipynb` file to open this kernel on [http://localhost:8888/notebooks/create-entities-per-article-data.ipynb](http://localhost:8888/notebooks/create-entities-per-article-data.ipynb)
- Run each of the cells in order from top to bottom, which will create the following files in the `data` directory:
  - `entities_per_article_data.json`: commodities identified in each article (non-essential for training the NER model)


## Creating training datasets
- Click the `create-training-data.ipynb` file to open this kernel on [http://localhost:8888/notebooks/create-training-data.ipynb](http://localhost:8888/notebooks/create-training-data.ipynb)
- Run each of the cells in order from top to bottom, which will create the following files in the `data` directory:
  - `training_data.json` (used by spaCy v2)
  - `training_data.spacy` (used by spaCy v3)


## Creating evaluation datasets
- Click the `create-evaluation-data.ipynb` file to open this kernel on [http://localhost:8888/notebooks/create-evaluation-data.ipynb](http://localhost:8888/notebooks/create-evaluation-data.ipynb)
- Run each of the cells in order from top to bottom, which will create the following files in the `data` directory:
  - `evaluation_data.json` (used by spaCy v2)
  - `evaluation_data.spacy` (used by spaCy v3)


## Creating test dataset
- Click the `create-test-data.ipynb` file to open this kernel on [http://localhost:8888/notebooks/create-test-data.ipynb](http://localhost:8888/notebooks/create-test-data.ipynb)
- Run each of the cells in order from top to bottom, which will create the following file in the `data` directory:
  - `test_data.json`: body text segments that have not been used to train/validate the NER model that can be used to test the NER model


## Creating a spaCy NER model
- Visit [spaCy's training config quickstart](https://spacy.io/usage/training#quickstart), apply the below settings before copying the contents to your clipboard:
  - **Language:** English
  - **Components:** ner
  - **Hardware:** CPU
  - **Optimize for:** efficiency
- Paste the contents into a root-level file called `base_config.cfg` and update the `[paths]` variables to point to the corresponding spaCy format datasets:
  - `train = null` -> `train = "/data/training_data.spacy"`
  - `dev = null` -> `dev = "/data/evaluation_data.spacy"`
- Run `$ python -m spacy init fill-config base_config.cfg config.cfg` to create from `base_config.cfg` a properly formatted `config.cfg` file that will be used to train the NER model
- Run `$ python -m spacy train config.cfg --output ./output` to use the training data to create a spaCy NER model, and which will display output that looks like:
```
✔ Created output directory: output
ℹ Saving to output directory: output
ℹ Using CPU

=========================== Initializing pipeline ===========================
[2022-06-03 13:10:15,235] [INFO] Set up nlp object from config
[2022-06-03 13:10:15,241] [INFO] Pipeline: ['tok2vec', 'ner']
[2022-06-03 13:10:15,243] [INFO] Created vocabulary
[2022-06-03 13:10:15,244] [INFO] Finished initializing nlp object
[2022-06-03 13:10:16,276] [INFO] Initialized pipeline components: ['tok2vec', 'ner']
✔ Initialized pipeline

============================= Training pipeline =============================
ℹ Pipeline: ['tok2vec', 'ner']
ℹ Initial learn rate: 0.001
E    #       LOSS TOK2VEC  LOSS NER  ENTS_F  ENTS_P  ENTS_R  SCORE
---  ------  ------------  --------  ------  ------  ------  ------
  0       0          0.00     23.67    0.29    0.16    1.33    0.00
  0     200         61.09   1053.11   93.33   98.30   88.83    0.93
  0     400         40.14    103.51   97.58   98.52   96.65    0.98
  0     600         34.08     45.84   99.30   99.08   99.52    0.99
  1     800         26.39     25.22   99.42   99.16   99.68    0.99
  1    1000         28.02     19.98   99.10   98.41   99.80    0.99
  2    1200         19.63     13.10   99.78   99.64   99.92    1.00
  3    1400         19.82      6.97   99.70   99.68   99.72    1.00
  4    1600         16.01      6.17   99.72   99.52   99.92    1.00
  6    1800          7.11      4.08   99.66   99.32  100.00    1.00
  7    2000         38.36     11.99   99.72   99.48   99.96    1.00
  9    2200         66.60     20.97   99.14   98.41   99.88    0.99
 12    2400         70.13     24.79   98.61   97.25  100.00    0.99
 14    2600         29.04      8.53   99.64   99.32   99.96    1.00
 17    2800          0.00      0.00   99.60   99.20  100.00    1.00
✔ Saved pipeline to output directory
output/model-last
```

It will also create an `output` root-level directory which contains `model-best` and `model-last` sub-directories:
```
├── output
│   ├── model-best
│   ├── model-last
```


## Conducting an informal NER model test
- Click the `test-informal.ipynb` file to open this kernel on [http://localhost:8888/notebooks/test-informal.ipynb](http://localhost:8888/notebooks/test-informal.ipynb)
- Run each of the cells in order from top to bottom, which in the final cell will test the specified item from the test data against the NER model

## Conducting a formal NER model test
- Click the `test-formal.ipynb` file to open this kernel on [http://localhost:8888/notebooks/test-formal.ipynb](http://localhost:8888/notebooks/test-formal.ipynb)
- Run each of the cells in order from top to bottom, which in the final cell will test all items in the test data against the NER model and display a confusion matrix


## Sample file extracts

The sample files were sourced by searching for FT content whose body text included one of the 20 commodities:
- aluminium
- cattle
- cobalt
- cocoa
- coffee
- copper
- corn
- cotton
- crude oil
- gold
- iron ore
- lithium
- natural gas
- palm oil
- poultry
- rice
- silver
- sugar
- wheat
- zinc

#### `data/ft-articles-training.txt`
- Each line should contain an article, starting with its UUID, followed by triple pipes, followed by the body text split into segments delineated by double pipes (I chose to delineate segments based on where line breaks occurred)
- The file should not end with an empty newline
- The file includes **2,000 unique articles**: 100 articles for each of the 20 commodities (though each article may potentially mention multiple commodities)
- I chose articles that mentioned the commodities in contexts that would emphasise it as such, e.g. "aluminium traders", "the price of corn", "producers of cotton", "wheat futures contracts", etc., which is a very manual process as it requires avoiding homonyms, mentions of the commodity in the wrong sort of context, and metaphorical usage, etc. (examples below)
- The UUIDs of articles used for this file can be seen in the [FT articles training set wiki](https://github.com/andygout/ner-commodities/wiki/FT-articles-training-set)

**Homonyms:**
  - cattle -> Cattle's PLC: a British consumer finance company
  - gold -> Yamana Gold: Canadian gold mine and established producer
  - rice -> Condoleezza Rice: Former United States Secretary of State

**Undesired contexts:**
  - articles about coffee shop culture
  - rice pudding recipes

**Metaphorical usage:**
  - "…about as inviting as a bowl of cold rice pudding…"

```
1e852438-161d-4095-90f8-fccb810b4efe|||Lorem ipsum dolor sit amet…||Ut enim ad minim veniam…||Duis aute irure dolor.
3659322d-b762-437a-b345-22e3bc203e5c|||Sed ut perspiciatis unde…||Nemo enim ipsam voluptatem…||Neque porro quisquam est.
…
aa1e07d2-0a30-41cd-b146-b730ea5467ad|||At vero eos et accusamus…||Et harum quidem…||Nam libero tempore.
```

---

#### `data/ft-articles-evaluation.txt`
- This file follows the same format as used for [`ft-articles-training.txt`](#dataft-articles-trainingtxt)
- The file includes **500 unique articles**: 25 articles for each of the 20 commodities
- The UUIDs of articles used for this file can be seen in the [FT articles evaluation set wiki](https://github.com/andygout/ner-commodities/wiki/FT-articles-evaluation-set)

---

#### `data/ft-articles-test.txt`
- This file follows the same format as used for [`ft-articles-training.txt`](#dataft-articles-trainingtxt)
- The file includes **500 unique articles**: 25 articles for each of the 20 commodities
- The UUIDs of articles used for this file can be seen in the [FT articles test set wiki](https://github.com/andygout/ner-commodities/wiki/FT-articles-test-set)


## References
- [Jupyter/IPython Notebook Quick Start Guide](https://jupyter-notebook-beginner-guide.readthedocs.io/en/latest/index.html)
	- [1. What is the Jupyter Notebook?](https://jupyter-notebook-beginner-guide.readthedocs.io/en/latest/what_is_jupyter.html)
	- [2. Installation](https://jupyter-notebook-beginner-guide.readthedocs.io/en/latest/install.html)
	- [3. Running the Jupyter Nohttps://www.youtube.com/watch?v=PJZzBp6em-Qtebook](https://jupyter-notebook-beginner-guide.readthedocs.io/en/latest/execute.html)
- [Anaconda Distribution - Download](https://www.anaconda.com/products/distribution)
- [spaCy](https://spacy.io)
  - [Rule-based matching](https://spacy.io/usage/rule-based-matching)
- [YouTube: Python Tutorials for Digital Humanities by Dr William Mattingly](https://www.youtube.com/c/PythonTutorialsforDigitalHumanities)
  - [How to Use spaCy's EntityRuler (Named Entity Recognition for DH 04 | Part 01)](https://www.youtube.com/watch?v=wpyCzodvO3A) (30 Nov 2020)
  - [How to Use spaCy to Create an NER training set (Named Entity Recognition for DH 04 | Part 02)](https://www.youtube.com/watch?v=YBRF7tq1V-Q) (02 Dec 2020) - creates a training set of 2,213 segments that include entities (6-10k sentences, which is deemed a pretty good size)
  - [How to Train a spaCy NER model (Named Entity Recognition for DH 04 | Part 03)](https://www.youtube.com/watch?v=7Z1imsp6g10) (04 Dec 2020)
  - [How to Convert spaCy 2x Training Data to 3x (Named Entity Recognition in spaCy Tutorials)](https://www.youtube.com/watch?v=TKoPva69_6E) (12 Apr 2021)
  - [How to Create a Config.cfg File in spaCy 3x for Named Entity Recognition (NER)](https://www.youtube.com/watch?v=l67PXnhu0ig) (14 Apr 2021)
  - [How to Train an NER Model in spaCy 3x](https://www.youtube.com/watch?v=PJZzBp6em-Q) (07 May 2021)
  - [How to Structure an Informal NER Test with spaCy 3 (Named Entity Recognition Tutorials)](https://www.youtube.com/watch?v=SJeDvHKHN_k) (16 May 2021)
  - [How to Structure a Formal Test with Confusion Matrix in spaCy 3 for NER Models (NER for DH)](https://www.youtube.com/watch?v=k1FtpADlusE) (23 May 2021)
- [GitHub: wjbmattingly (William Mattingly)](https://github.com/wjbmattingly)
  - [spacy_tutorials_3x/02_02_formal_test.ipynb](https://github.com/wjbmattingly/spacy_tutorials_3x/blob/main/02_02_formal_test.ipynb)
  - [spacy_3_ner_tutorials](https://github.com/wjbmattingly/spacy_3_ner_tutorials)
  - [holocaust_ner_lessons](https://github.com/wjbmattingly/holocaust_ner_lessons)
- [Medium: Confusion Matrix Visualization for SpaCy NER by Dulaj Rajitha](https://dulaj.medium.com/confusion-matrix-visualization-for-spacy-ner-9e9d99120ee9) (25 Jul 2019)
