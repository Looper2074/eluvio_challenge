# Eluvio Challenge
### Introduction 

We are being asked to "find a problem" to solve in a given Dataset from Eluvio. It contains small 
titles of users which mainly seem to be about conflicts,geopolitical news,wars etc. The dataset  also gives us other information too(if they are adults,their username,upvotes,downvotes,etc). I decided to use spaCy at first but I also used gensim for unsupervised clustering.
**Github repository where I used gensim(ipynb notebook):**
https://github.com/Looper2074/eluvio_gensim

### spaCy
Considering Eluvio's very large datasets I had to first  find the best performing open-source library in order to maximize speed and efficiency at which we process our data.Spacy is a very popular library that provide us with what we need.As we can see from its benchmarks it perfoms very well.

![spacy-benchmark](https://user-images.githubusercontent.com/58263228/114309621-758dfc00-9af0-11eb-911e-f75281d2e5ba.png)





Spacy provides its users with pre-trained "pipelines" that can be installed as Python packages. 

![selected trained pipeline](https://user-images.githubusercontent.com/58263228/114309637-80489100-9af0-11eb-9bf4-721885a0c019.png)





Below is a summary of the features spaCy is providing:

![features overview](https://user-images.githubusercontent.com/58263228/114309647-863e7200-9af0-11eb-83b0-c8be00f20d89.png)



**Pretrained model used:** en_core_web_lg (lg indicating the large volume of text it was pretrained on)

**Pipeline**: ['tok2vec', 'tagger', 'parser', 'ner', 'attribute_ruler', 'lemmatizer']


* **Tokenization**: Segments text into words,punctuations marks,etc
* **tok2vec**: Transforms tokens into vectors and saves them in Token.vector. The library generates word vectors using an algorithm called word2vec under the hood.
* **tagger**: Assigning word types to tokens,like verb or noun.
* **parser**: Assigns syntantic dependency labels,describing the relations between individual tokens,like subject or object.
* **ner**: labelling named "real-world" objects, like persons,companies or locations.
* **attribute_ruler**: manages rule-based mappings and exceptions for all token-level attributes
* **lemmatizer**: Assigns the base forms of words. For example, the lemma of “was” is “be”, and the lemma of “rats” is “rat”.


This project also uses spaCy "projects".SpaCy projects let you manage and share end-to-end spaCy workflows for different use cases and domains, and orchestrate training, packaging and serving your custom pipelines. It also makes it easy to integrate many other tools. SpaCy projects also make it easy to integrate with many other tools.https://spacy.io/usage/projects


<!-- SPACY PROJECT: AUTO-GENERATED DOCS START (do not remove) -->

# 🪐 spaCy Project: Eluvio

 Eluvio's challenge in predictive modelling or analytical insights for business use cases  

## 📋 project.yml

The [`project.yml`](project.yml) defines the data assets required by the
project, as well as the available commands and workflows. For details, see the
[spaCy projects documentation](https://spacy.io/usage/projects).

### ⏯ Commands

The following commands are defined by the project. They
can be executed using [`spacy project run [name]`](https://spacy.io/api/cli#project-run).
Commands are only re-run if their inputs have changed.

| Command | Description |
| --- | --- |
| `preprocess` | Convert the data to spaCy's binary format and output the result in the corpus directory  |
| `phrasematch` | Runs some phrase matching examples from a json file(assets/phrasematches.json) |
| `dependencymatch` | Runs some dependency matching examples from a json file(assets/dependencymatches.json) |
| `VisualizePhraseResults` | It visualizes phrase matched results  |
| `VisualizeDependencyResults` | It visualizes dependency matched results  |
| `ClusterEntities` | For some named entity types we are displaying the number of times it has been mentioned in posts and also what number of them are of positive/negative semtiment using TextBlob |

### ⏭ Workflows

The following workflows are defined by the project. They
can be executed using [`spacy project run [name]`](https://spacy.io/api/cli#project-run)
and will run the specified commands in order. Commands are only re-run if their
inputs have changed.

| Workflow | Steps |
| --- | --- |
| `matcher` | `preprocess` &rarr; `phrasematch` &rarr; `dependencymatch` &rarr; `VisualizePhraseResults` &rarr; `VisualizeDependencyResults` |
| `cluster` | `preprocess` &rarr; `ClusterEntities` |

### 🗂 Assets

The following assets are defined by the project. They can
be fetched by running [`spacy project assets`](https://spacy.io/api/cli#project-assets)
in the project directory.

| File | Source | Description |
| --- | --- | --- |
| [`assets/Eluvio_DS_Challenge.csv`](assets/Eluvio_DS_Challenge.csv) | Local | CSV data given from Eluvio |
| [`assets/phrasematches.json`](assets/phrasematches.json) | Local | JSON-formatted input phrase matches for testing |
| [`assets/dependencymatches.json`](assets/dependencymatches.json) | Local | JSON-formatted input dependency matches for testing |

<!-- SPACY PROJECT: AUTO-GENERATED DOCS END (do not remove) -->
### Rule-based matching

Since, we are interested in extracting information from the dataset it makes sense to use spaCy's **rule-matching**. Apart from being able to find keywords in the corpus file that gets produced during the preprocessing state. It also gives us the ability to **find relationships between tokens**, expand tokens to extract useful information. I will be testing some queries ,first some phrase matching rules then dependency matching rules. In the project I am saving the produced outputs in a json file, filtered based on upvotes where a hypothetical front-end developer could use. 

However, spaCy has integration with **visual tools** (displaCy) that allows you to see important information in the results. It can display **entity tags** ,for example it can tell you if a keyword is a country or an organization and it can also display the dependencies of a sentence. With that in mind, if someone for example wants to find what countries might be going into war ,we can query the data about war and then display entity tags only for countries. A dependency example that I tried was to find sentences that include the verb "sue" and its direct objective. If someone wants to go a step further we can even save these tokens (for example all the companies that are getting sued) by reading the value of the dictionary of the patterns with the  name of the dependency that corresponds to it.

#### Phrase-Matcher queries:
```json
{
  "1": [
    "Barack Obama"
  ],
  "2": [
    "free trade"
  ],
  "3": [
    "war",
    "missiles"
  ],
  "4": [
    "warship"
  ],
  "5": [
    "CEO"
  ],
  "6": [
    "investors"
  ],
  "7": [
    "protestors"
  ],
  "8": [
    "activists"
  ]
}
```

#### Dependency-Matcher queries:
```json
{
  "1":
  [{
    "RIGHT_ID": "poses",
    "RIGHT_ATTRS": {"ORTH": "poses"}
  },
  {
    "LEFT_ID": "poses",
    "REL_OP": ">",
    "RIGHT_ID": "threat",
    "RIGHT_ATTRS": {"ORTH": "threat"}
  }],
   "2":
  [{
    "RIGHT_ID": "conflict",
    "RIGHT_ATTRS": {"ORTH": "conflict"}
  },
    {
      "LEFT_ID": "conflict",
      "REL_OP": ">",
      "RIGHT_ID": "about",
      "RIGHT_ATTRS": {
        "DEP": "prep"
      }
    }],
   "3":
  [{
        "RIGHT_ID": "anchor_sue",
        "RIGHT_ATTRS": {"ORTH": "sue"}
    },
    {
        "LEFT_ID": "anchor_sue",
        "REL_OP": ">",
        "RIGHT_ID": "founded_subject",
        "RIGHT_ATTRS": {"DEP": "nsubj"}
    },
    {
        "LEFT_ID": "anchor_sue",
        "REL_OP": ">",
        "RIGHT_ID": "founded_object",
        "RIGHT_ATTRS": {"DEP": "dobj"}
    }]

}
```


#### Phrase matcher results(html renders):
![1](https://user-images.githubusercontent.com/58263228/114451111-8a958880-9bdf-11eb-8bc9-cc3b885b5828.png)
![2](https://user-images.githubusercontent.com/58263228/114451116-8b2e1f00-9bdf-11eb-8ff8-ac05f5b81f55.png)
![3](https://user-images.githubusercontent.com/58263228/114451120-8c5f4c00-9bdf-11eb-8d1a-e7d7cf8160d0.png)
![4](https://user-images.githubusercontent.com/58263228/114451122-8d907900-9bdf-11eb-9b4f-bde1f87c79fa.png)
![5](https://user-images.githubusercontent.com/58263228/114451127-8ec1a600-9bdf-11eb-8137-2e3480422727.png)
![6](https://user-images.githubusercontent.com/58263228/114451133-8ff2d300-9bdf-11eb-97c7-dabf5228a697.png)
![7](https://user-images.githubusercontent.com/58263228/114451144-941ef080-9bdf-11eb-95fc-84a088954d1f.png)


These renders above use this configuration in scripts/visualize.py(they don't exclude any entity tags):
![dependency option render](https://user-images.githubusercontent.com/58263228/114312942-374b0980-9afd-11eb-80b8-1364a113a90c.png)

The "sue" dependency example(we are only displaying organizations):

![dependency3render](https://user-images.githubusercontent.com/58263228/114312907-0b2f8880-9afd-11eb-905f-a7e16781e867.png)

It uses this configuration to display the results:

![1](https://user-images.githubusercontent.com/58263228/114313106-d6700100-9afd-11eb-8c26-4b7c88127621.png)

We can also,as mentioned before display the dependencies(test case of finding conflict and a preposition along with it):

![dependency-with-dep-render2](https://user-images.githubusercontent.com/58263228/114313156-03241880-9afe-11eb-9ea5-600f50798bdb.png)

![dependency option render](https://user-images.githubusercontent.com/58263228/114313147-facbdd80-9afd-11eb-811e-062fd0000652.png)

#### Some other ideas include:
* find all mentions of "ministry" and determine which country's is mentioned for and its type (ex.the "French Ministry of Defense"), and then we add up the number of times it appears for each ministry.(ex.French Ministry of Defense appeared 10 times)
* protestors (british protestors,violent protestors etc) and then again find their frequencies
* find information that relates to money using the money tag of spacy
* find news about elections so then you predict future outcomes in that country. If a dictatorship falls in a region then it might be prosperous to open a business there
* CEO Names mentions
* We can also exclude results based on age
* Track news about someone or about a company for example throughout time.For example find all news related to Facebook and display them in a linear order.

However,there might be a better way to find related information like this. Writing queries like that requires us to monitor the dataset to come up with queries and we have to constantly write new queries if we want to find new information on new data.What better way is there? Unsupervised Clustering as you will see in notebook of Gensim.Spacy is still a very useful tool though if we wanted to write very specific queries based on a business' request. Also,unfortunately spacy doesn't always recognize tags correctly as in the example of the Obama Camp that gets recognized as a person instead of an organization. For these kind of problems we would have to train the dataset in order to recognize tags better.Prodigy is a tool that can used for that to easily annotate the dataset and then train using spaCy with pytorch or tensorflow. spaCy recently added to train with the state of the art transformers.

https://github.com/explosion/spacy-transformers

#### Testing the project:
I included the auto-generated requirements from pycharm, in the end I queried the whole dataset but the corpus dataset was too large to upload in github.However,the output files are the results from using the whole dataset so you open them up.If you want to test on a smaller part of the dataset just change this line in the preprocess script:
texts = pd.read_csv(input_path)[:desired_number_of_lines]
Also don't forget to download the pretrained model:
python -m spacy download en_core_web_lg

#### Performance

* The matchers allow you to check a token for a match through an assigned id instead of quering the whole dataset for every different phrase match.Also,I think spaCy allows you to define custom user data in DocBin.store_user_data so you don't have to read from csv but instead read from the serialized object.
* spaCy gives you also the ability to stream the input data: docs = nlp.pipe(texts, n_process=2, batch_size=2000). Gensim does not give you the ability to stream data like this but it would be easy to combine spaCy's pipeline with gensim.So if I were to continue this project I would fix the issues mentioned and combine spaCy with gensim.However, I did try some interesting things in gensim so please make sure to check it out in the linked repository.

# Update : Version 0.0.1

I am now adding the ability to "cluster" any entity types that the language model supports and the ability to do sentiment analysis on a post title based on its polarity with TextBlob.You can read more about spaCyTextBlob here:
* https://github.com/SamEdwardes/spaCyTextBlob

I clustered the following named entities:

* EVENT: An event
* FAC: Faculty mention
* GPE: Country
* LAW: Law mention
* LOC: Location(can be city,state, etc)
* ORG: Any Organization
* PERSON: Person mentions
* PRODUCT: Product mentions

You can find more information/customize in the cluster_entities.py script .I have to remind you again that spaCy allows us to train any named entities we want with the help of Prodigy .What do I mean by clustering?I am quering the whole dataset and for each entity type,for each unique term in this entity type I am aggregating the times we are seeing this unique term and I am also doing sentiment analysis on the doc it belongs to.If polarity=0 we define the doc as neutral,if polarity <0 the doc is defined as negative else the doc is defined as positive.We are saving all these results in an html file for each named entity. You can find all these files in outputs/entity_clusters or again you can run data you want by running "spacy project run cluster" as mentioned in the autogenerated spaCy project text.We are also sorting based on the number of mentions. So this clustering script allows you to extract the public's opinion on a desired named entity type.For example you might want to see what names or organizations are mentioned often .If we had used the transformer model we would probably have gotten even better results.Below I am attaching screenshots from the results.

* ### EVENT
![EVENT](https://user-images.githubusercontent.com/58263228/115154439-91ab1380-a083-11eb-9699-49085e5f413d.png)

* ### FAC
![FACULTY](https://user-images.githubusercontent.com/58263228/115154441-9374d700-a083-11eb-94c6-c53b1e88322d.png)
* ### GPE
![GPE](https://user-images.githubusercontent.com/58263228/115154443-966fc780-a083-11eb-83d9-3b9c9e869313.png)
* ### LAW
![LAW](https://user-images.githubusercontent.com/58263228/115154446-98d22180-a083-11eb-8548-4ffd76bf8769.png)

* ### LOC
![LOC](https://user-images.githubusercontent.com/58263228/115154449-9a034e80-a083-11eb-8beb-c379b80d4823.png)

* ### ORG
![ORG](https://user-images.githubusercontent.com/58263228/115154450-9b347b80-a083-11eb-928c-9bd3981b28c4.png)

* ### PERSON
![PERSON](https://user-images.githubusercontent.com/58263228/115154451-9bcd1200-a083-11eb-8c23-1aa89552c0aa.png)

* ### PRODUCT
![PRODUCT](https://user-images.githubusercontent.com/58263228/115154452-9c65a880-a083-11eb-91b5-099cac4d89d5.png)

One visualization idea concerning geographical regions(since this dataset heavily mentions geopolitical events/news) was to pinpoint locations in a Global Map during  a certain time range using Streamlit(similar project):
https://github.com/nithishr/streamlit-data-viz-demo

So we could for example select a month in 2013 and a map would pinpoint locations that were mentioned in posts.We could also give a color gradient to a certain location depending on the percentage of positive/negative sentiment,green for positive sentiment,red for negative sentiment.If someone hovered on a location he would see the percentage of positive/negative posts and also a hyperlink to the positive or negative,neutral posts where the user would be able to see all of the posts listed.However,that's beyond the scope for a job audition but it would be very interesting to implement in case the hiring proccess continues or/and if it was a desired feature.One simpler/more general thing to do would be to color gradient the percentage of positive/negative posts.

# Other interesting NLP applications for Eluvio's content

**Hugginface has transformer models that have numerous applications that would help Eluvio in:**
* Question Answering with distil-bert,bert.For example "Brad Pitt in the movie x portrays what character?Answer: a box fighter"
* Summarization of movie scripts with distil-bert,bert.
* Translation of movie scripts/subtitle creation with t5
* Using models such gpt2/distil gpt-2 to help with content generation for artists/creators or creating queries for gpt3's paid api which is not open-sourced unfortunately.
