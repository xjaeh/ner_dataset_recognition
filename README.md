# ner_dataset_recognition

## Dataset information

#### The datasets are provided in the datasets file.
- The once with \_sentences contain the sentences and the conference information
- The once without it are in IOB2 format

#### Datasets explained
_Only information about the datasets without \_sentences are provided, \_sentences contain the same sentences as there counterpart_
- Dataset.csv - The whole dataset

- Zero-shot.csv - Contains 200 sentences with datasets that do not occur in the train or test set

- Train_set.csv - The 80% of the 80/20 split of the remaining sentences of the whole dataset (so without the zero-shot set)

- Test_set.csv - The 20% of the 80/20 split of the remaining sentences of the whole dataset (so without the zero-shot set)

- Test_set_real_ratio - A test set where only 1% of the sentences contain dataset names

- Test_set_easy - 444 sentences from the test set that only contain dataset names with three or fewer words

- Test_set_hard - 444 sentences from the test set that only contain dataset names with four or more words

- SSC - A dataset containing only weak supervised examples

- SSC_positives - Only the potive weak supervised examples

## Code information

#### The code is providid in the code file
- The files with \_cross-validation are the code for cross-validation
- The other ones are the regular methods.

#### Getting the right data
In the code the standard situation is shown, so training on the train set and testing on the test set. 

For the more specialised cases, code is provided below

#### Domain adaptability 

DATAset = pd.read_csv('Dataset_sentences.csv')

BIOset = pd.read_csv('Dataset.csv')

ids = DATAset[DATAset.conference == 'VISION'].id.to_list()

sentences = ['Sentence: ' + str(id) for id in ids]

data = BIOset[~BIOset['Sentence #'].isin(sentences)]

test = BIOset[BIOset['Sentence #'].isin(sentences)]

#### Amount of training data

*This is done via slicing, slices of the stratisfied 20 fold are used and each time added to the previous slice*

Settings:

skf = StratifiedKFold(20, shuffle=True, random_state=42)

#### Positive/negative ratio
*The right data is selected using slicing, example: np.array(X_tr)[:2168]*

TRAINset = pd.read_csv('Train_set_sentences.csv')

ids = TRAINset[~TRAINset.labels.str.contains('Geen')].id.to_list()

sentences = ['Sentence: ' + str(id) for id in ids]

ds = data[data['Sentence #'].isin(sentences)]

nds = data[~data['Sentence #'].isin(sentences)]

data = ds.append(nds)

#### Other

*This is done by appending, example:*

data = pd.read_csv('Train_set.csv')
data2 = pd.read_csv('SSC.csv')
data = data.append(data2)
