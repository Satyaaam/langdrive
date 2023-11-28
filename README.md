<div align="center">

# LangDrive

### Train, deploy and query open source LLMs using your private data, all from one library.

<p>
<img alt="GitHub Contributors" src="https://img.shields.io/github/contributors/addy-ai/langdrive" />
<img alt="GitHub Last Commit" src="https://img.shields.io/github/last-commit/addy-ai/langdrive" />
<img alt="GitHub Repo Size" src="https://img.shields.io/github/repo-size/addy-ai/langdrive" />
<img alt="GitHub Issues" src="https://img.shields.io/github/issues/addy-ai/langdrive" />
<img alt="GitHub Pull Requests" src="https://img.shields.io/github/issues-pr/addy-ai/langdrive" />
<img alt="Github License" src="https://img.shields.io/badge/License-Apache-yellow.svg" />
<img alt="Discord" src="https://img.shields.io/discord/1057844886243643532?label=Discord&logo=discord&logoColor=white&style=plastic&color=d7b023)](https://discord.gg/G8eYmcaTTd" />
</p>

</div>

-----
<p align="center">
  <a href="#-use-cases">Use cases</a> •
  <a href="#-features">Features</a> •
  <a href="https://docs.langdrive.ai" target="_blank">Docs</a> •
  <a href="#-getting-started">Getting started</a> •
  <!-- <a href="#-tutorials" target="_blank">Tutorials</a> •
  <a href="#-tutorials" target="_blank">Blog</a> • -->
  <a href="#-contributions" target="_blank">Contributions</a>
</p>

-----

LangDrive is an open-source AI library that simplifies training, deploying, and querying open-source large language models (LLMs) using private data. It supports data ingestion, fine-tuning, and deployment via a command-line interface, YAML file, or API, with a quick, easy setup.

Read the [docs](https://docs.LangDrive.ai) for more.

-----

## Use cases

LangDrive lets you builds amazing AI apps like:

- Question/Answering over internal documents
- Chatbots
- AI agents
- Content generation

-----

## Features:

- Data ingestion
    > LangDrive comes with the following built in data connectors to simplify data ingestion:
    - Firebase Firestore
    - Email Ingestion via SMTP
    - Google Drive
    - CSV
    - Website URL
    - (more coming soon, or you can build yours - LangDrive is open source)

- Fine tuning
    > Fine tune open source LLMs easily by formating your data into input:output completion pairs

- Deployment
    > Add your Hugging Face access token to deploy your model directly to hugging face hub after fine tuning
    - Hugging Face

- Inference
    > Query our supported open source models

- Data Utils
    > LangDrive comes built-in with data utils for CRUD operations for the different data connectors

- API
    > Call our support open source models from a single API
    - Completions API: https://api.langdrive.ai/v1/chat/completions
    - Fine tuning API: https://api.langdrive.ai/train
    Read the [docs](https://docs.LangDrive.ai) for more.

-----

## Docs
To see full Documentation and examples, go to [docs](https://docs.LangDrive.ai)

-----

## Getting started

Thank you for taking interest in LangDrive!

LangDrive's set of connectors and services makes training LLMs easy, and you can get started with just a CSV file. By providing a Hugging Face API key  you can train models and even host them in the cloud 😉  

Import LangDrive in your project or configure and execute LangDrive directly from the CLI. The remainder of this article will explore using both approaches for training and deploy models with LangDrive. Along the way we will explore the use of a YAML doc to help with the connecting to data and services.


#### Using the CLI

Node developers can train and deploy a model in 2 simple steps. 

1. `npm install LangDrive`
2. `LangDrive train --csv ./path/to/csvFileName.csv --hftoken apikey123 --deploy`

In this case, LangDrive will retrieve the data, train a model, host it's weights on Hugging Face, and return an inference endpoint you may use to query the LLM.  
	
The command `LangDrive train` is used to train the LLM, please see how to configure the command below.

args:

- `yaml`: Path to optional YAML config doc, default Value: './LangDrive.yaml'. This will load up any class and query for records and their values for both inputs and ouputs.
- `csv`: Path to training dataCSV*The training data should be a two-column CSV of input and output pairs.
- `hfToken`: An API key provided by Hugging Face with `write` permissions. Get one [here](https://huggingface.co/docs/hub/security-tokens).
- `baseModel`: The original model to train: This can be one of the models in our supported models shown at the bottom of this page
- `deployToHf`: true | false
- `hfModelPath`: The full path to your hugging face model repo where the model should be deployed. Format: hugging face username/model

It is assumed you do not want to deploy your model if you run `LangDrive train`. In such a case a link to where you can download the weights will be provided. Adding `--deploy` will return a link to the inferencing endpoint.

More information on how to ingest simple data using the CLI can be found in the [CLI](./cli.md) docs. For more comlex examples, read on...


#### Getting Started with YAML

Getting the data and services you need shouldn't be the hardest part about training your models! Using YAML, you can configure more advanced data retrieval and training/ deployment strategies. Once configured, these settings are available for the standalone API and also from the CLI.

Refer to the [Yaml](./yaml.md) docs for more information.

###### Step 1: Configure Your Data Connectors

Our growing list of data-connectors allow anyone to retrieve data through a simple config doc. As LangDrive grows, our set of Open-Source integrations will grow. At the moment, you can connect to your data using our `email`, `firestore`, and `gdrive` classes.  

In essense, config of these data-connectors is as straight forward as:

    firestore: 
      clientJson: "secrets/firebase_service_client.json"
      databaseURL: "env:FIREBASE_DATABASE_URL"

    drive:
      clientJson: "secrets/drive_service_client.json" 

    email:
      password: env:GMAIL_PASSWORD
      email: env:GMAIL_EMAIL

You may specify .env variables using `env:` as a prefix for your secret information.

In our example above, the `clientJson` attribute is a [Firebase service account file](https://firebase.google.com/support/guides/service-accounts).

Once this information is provided, the entire __OAuth Process__ will automatically be handled on your behalf when using any associated library, regardless if it's used in the CLI or API. Please refer to our notes on [security](./security/authentication.md) for more information on the Outh2 process when using google. 

###### Step 2: Configure Your LLM Tools

Once you have your data-connectors set up, config your training and deployment information. The last step will be to connect the two.

Training on Hugging Face and hosting the weights on Hugging Face hubs:
```
    huggingface:
        hfToken: env:HUGGINGFACE_API_KEY 
        deployToHf: false 
```

<b>NOTE</b>: To specify the model you want to train and where to host it:
```
  huggingface:
    hfToken: env:HUGGINGFACE_API_KEY
    baseModel: 
      name: "vilsonrodrigues/falcon-7b-instruct-sharded"
    trainedModel: 
      name: "karpathic/falcon-7b-instruct-tuned"
    deployToHf: true 
```

Simple enough, huh? Here comes the final step.

###### Step 3: Connecting Your Data to Your LLM

To connect data to your LLM tool, we will need to create a new YAML entry `train:`. 

Here we specify specific the specific data we want to train on. In the case of a CSV, a most simple example, we can use the `path` value to specify it's location. 

LangDrive.yaml
```
    train:
      path: ../shared.csv               - Default Path for Input and Output 
      inputValue: input                 - Attribute to extract from path
      outpuValue: output 
```

Now lets show how to query data from one of those third-party services we configured earlier.

Within the `train` entry, setting a `service` and `query` will do the trick. Set a data-connector as the `service` and one of its methods (and its args) as the `query` value. This will require exploring class documentation.

LangDrive.yaml
```
    train:
        service: 'firebase' 
        query:
          filterCollectionWithMultipleWhereClauseWithLimit:
              collection: "chat-state"                
              filterKey: []
              filterData: []
              operation: []
              limit: 5
```

In the example above we use the `filterCollectionWithMultipleWhereClauseWithLimit` method from LangDrive's Firestore class, passing arguements as specified in the LangDrive [Firestore](./api/firestore.md) docs. `collection` is the firestore collection name to retrieve data from, (limited to the first 5 entries). `filterKey` and `filterData` are not specified in this example but contain the field name/key to filter. The `operation` value specifies the firebstore query operator to use (For example, '==', '>=', '<=' ).


Note:

> If the retrieved data has two columns (or attributes) they are assumed to be in the order [input, output]. If more columns exist, LangDrive grabs the first two columns after first looking for an 'inputValue' and 'outpuValue' column. The same logic applies for information retrieved from a query and works similarly for nested Json Objects (ala: `att1.attr2`)

#### Gettings Started with API:

Our classes can be exposed in the typical manner. For more information on any one class, please refer to it's corresponding documentation.

Coming Soon: Deploy self-hosted cloud based training infrastructure on AWS, Google Cloud Platform, Heroku, or Hugging Face. Code is currently being used internally and is under development prior to general release - code avaialbe in-repo under `/src/train`.

If you would like to interact directly directly with our training endpoint you can call our hosted training image directly via the LangDrive API. 

Endpoint: `POST https://api.LangDrive.ai/train`

###### Request Body

The request accepts the following data in JSON format.
```
{
   "baseModel": "string",
   "hfToken": "string",
   "deployToHf": "Boolean",
  "trainingData": "Array",
   "hfModelPath": "string",
}
```

`baseModel`: The original model to train. This can be one of our supported models, listed below, or a Hugging Face model

- Type: String
- Required: Yes

`hfToken`: Your Hugging Face token with write permissions. Learn how to create a Hugging Face token [here](https://huggingface.co/docs/hub/security-tokens).

- Type: String
- Required: Yes


`deployToHf`: A boolean representing whether or not to deploy the model to Hugging Face after training

- Type: Boolean
- Required: Yes

`trainingData`: This is an array of objects. Each object must have two attributes: input and output. The input attribute represents the user’s input and output attribute represents the model’s output.

- Type: Array
- Required: Yes

`hfModelPath`: The hugging face model repository to deploy the model to after training is complete

- Type: String
- Required: No


###### Response Body

The request returns the following data in JSON format.

```
HTTP/1.1 200
Content-type: application/json
{
   "success": "true",
}
```

#### Model Training

We plan to expand the number of available models for training. at the moment only sharded models work as using PEFT is how these models are trained.

###### Models Support Matrix

######## Causal Language Modeling
| Model        | Supported | 
|--------------| ---- | 
| Falcon-7b-sharded | ✅    |
| GPT-2        | Comming Soon  |
| Bloom        | Comming Soon  | 
| OPT          | Comming Soon  | 
| LLaMA        | Comming Soon  |  
| ChatGLM      | Comming Soon  | 

######## Model Type Support 
| Model Type       | Support | 
|--------------| ---- | 
|Conditional Generation  |  ✅  | 
|Conditional Generation  |  ✅  | 
|Sequence Classification|  ✅  | 
|Token Classification  |  ✅  | 
|Text-to-Image Generation|    | 
|Image Classification  |    | 
|Image to text (Multi-modal models)   |    | 
|Semantic Segmentation   |    | 

-----

## Contributions

LangDrive is open source and we welcome contributions from the community. To contribute, please make a PR through the ["fork and pull request"](https://docs.github.com/en/get-started/quickstart/contributing-to-projects) process.

Join our [Discord](https://discord.gg/G8eYmcaTTd) to keep up to date with the community and roadmap. 

