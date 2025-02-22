![image](https://user-images.githubusercontent.com/113465005/226238596-cc76039e-67c2-46b6-b0bb-35d037ae66e1.png)

# 3-day POC VBD powered by: Azure Cognitive Search + Azure OpenAI + Bot Framework + Langchain
Your organization requires a Multi-Channel Smart Chatbot and a search engine capable of comprehending diverse types of data scattered across various locations. Additionally, the conversational chatbot should be able to provide answers to inquiries, along with the source and an explanation of how and where the answer was obtained. In other words, you want **private and secured ChatGPT for your organization that can interpret, comprehend, and answer questions about your business data**.

The goal of the MVP POC is to show/prove the value of a GPT Virtual Assistant built with Azure Services, with your own data in your own environment. The deliverables are:

1. Backend Bot API built with Bot Framework and exposed to multiple channels (Web Chat, MS Teams, SMS, Email, Slack, etc)
2. Frontend web application with a Search and a Bot UI.

The repo is made to teach customers step-by-step on how to build a OpenAI based Smart Search Engine. Each Notebook builds on top of each other and ends in building the two applications.

**For Microsoft FTEs:** This is a customer funded VBD, below the assets for the delivery.

| **Item**                   | **Description**                                                                                                     | **Link**                                                                                                                                                |
|----------------------------|---------------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------|
| VBD SKU Info and Datasheet                   | CSAM must dispatch it as "Customer Invested" against credits/hours of Unified Support Contract                                      | [ESXP SKU page](https://esxp.microsoft.com/#/omexplanding/services/14205/geo/USA/details/1)                                                                                              |
| VBD Accreditation for CSAs     | Link for CSAs to get the Accreditation needed to deliver the workshop                                                                      | [Accreditation Link 1](https://learningplayer.microsoft.com/activity/s9239623/launch/), [Accreditation Link 2](https://learningplayer.microsoft.com/activity/s9239638/launch/)  |
| VBD 3-day POC Asset (IP)  | The MVP to be delivered  (this GitHub repo)                                     | [Azure-Cognitive-Search-Azure-OpenAI-Accelerator](https://github.com/MSUSAzureAccelerators/Azure-Cognitive-Search-Azure-OpenAI-Accelerator)                |
| VBD Workshop Deck          | The deck introducing and explaining the workshop                                                                    | [Azure OpenAI Accelerator - GPT Smart Search Pitch Deck.pdf](https://github.com/MSUSAzureAccelerators/Azure-Cognitive-Search-Azure-OpenAI-Accelerator/blob/main/Azure%20OpenAI%20Accelerator%20-%20GPT%20Smart%20Search%20Pitch%20Deck.pdf) |


---
**Prerequisites Client 3-Day POC**
* Azure subscription
* Accepted Application to Azure Open AI
* Microsoft members need to be added as Guests in clients Azure AD
* A Resource Group (RG)  needs to be set for this Workshop POC, in the customer Azure tenant
* The customer team and the Microsoft team must have Contributor permissions to this resource group so they can set everything up 2 weeks prior to the workshop
* A storage account must be set in place in the RG. <b>Disable firewalls and enable public network access from all networks</b>
* Data/Documents must be uploaded to the blob storage account, at least two weeks prior to the workshop date
* For IDE collaboration during workshop, Jupyper Lab will be used, for this, Azure Machine Learning Workspace must be deployed in the RG
   * Note: Please ensure you have enough core compute quota in your Azure Machine Learning workspace 

---
# Architecture 
![Architecture](./images/GPT-Smart-Search-Architecture.jpg "Architecture")

## Flow
1. The user asks a question.
2. In the app, an OpenAI LLM uses a clever prompt to determine which source contains the answer to the question.
3. Four types of sources are available:
   * 3a. Azure SQL Database - contains COVID-related statistics in the US.
   * 3b. Azure Bing Search API - provides online web search for current information.
   * 3c. Azure Cognitive Search - contains AI-enriched documents from Blob Storage (10k PDFs and 52k articles).
      * 3c.1. Uses an LLM (OpenAI) to vectorize the top K document chunks from 3c.
      * 3c.2. Uses in-memory cosine similarity to get the top N chunks.
      * 3c.3. Uses an OpenAI GPT model to craft the response from the Cog Search Engine (3c) by combining the question and the top N chunks.
   * 3d. CSV Tabular File - contains COVID-related statistics in the US.
4. The app retrieves the result from the source and crafts the answer.
5. The tuple (Question and Answer) is saved to CosmosDB to keep a record of the interaction.
6. The answer is delivered to the user.

---
## Demo

https://gptsmartsearch.azurewebsites.net/

---

## 🔧**Features**

   - Uses [Bot Framework](https://dev.botframework.com/) and [Bot Service](https://azure.microsoft.com/en-us/products/bot-services/) to Host the Bot API Backend and to expose it to multiple channels including MS Teams.
   - 100% Python.
   - Uses [Azure Cognitive Services](https://azure.microsoft.com/en-us/products/cognitive-services/) to index and enrich unstructured documents: Detect Language, OCR images, Key-phrases extraction, entity recognition (persons, emails, addresses, organizations, urls).
   - Uses [LangChain](https://langchain.readthedocs.io/en/latest/) as a wrapper for interacting with Azure OpenAI , vector stores, constructing prompts and creating agents.
   - Multi-Lingual (ingests, indexes and understand any language)
   - Multi-Index -> multiple search indexes
   - Tabular Data Q&A with CSV files and SQL Databases
   - Uses [Bing Search API](https://www.microsoft.com/en-us/bing/apis) to power internet searches in the bot
   - Uses CosmosDB as persistent memory to save user's conversations.
   - Uses [Streamlit](https://streamlit.io/) to build the Frontend web application in python.
   

---

## **Steps to Run the POC/Accelerator**

Note: (Pre-requisite) You need to have an Azure OpenAI service already created

1. Fork this repo to your Github account.
2. In Azure OpenAI studio, deploy these two models: **Make sure that the deployment name is the same as the model name.**
   - "gpt-35-turbo" for the model "gpt-35-turbo (0301)". If you have "gpt-4", use it (it is definitely better)
   - "text-embedding-ada-002"
3. Create a Resource Group where all the assets of this accelerator are going to be. Azure OpenAI can be in different RG or a different Subscription.
4. ClICK BELOW to create all the Azure Infrastructure needed to run the Notebooks (Azure Cognitive Search, Cognitive Services, SQL Database, CosmosDB, Bing Search API):

[![Deploy To Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fpablomarin%2FGPT-Azure-Search-Engine%2Fmain%2Fazuredeploy.json) 

_Note: If you have never created a cognitive multi-service account before, please create one manually in the azure portal to read and accept the Responsible AI terms. Once this is deployed, delete this and then use the above deployment button._

5. Enable Semantic Search on your Azure Cognitive Search Service:
   - On the left-nav pane, select Semantic Search (Preview).
   - Select either the Free plan or the Standard plan. You can switch between the free plan and the standard plan at any time.

6. Clone your Forked repo to your local machine or AML Compute Instance. If your repo is private, see below in Troubleshooting section how to clone a private repo.

7. Make sure you run the notebooks on a **Python 3.10 conda enviroment**
8. Install the dependencies on your machine (make sure you do the below comand on the same conda environment that you are going to run the notebooks:
```
pip install -r ./common/requirements.txt
```
8. Run 01-Load-Data-ACogSearch.ipynb:
  - Loads 9.8k PDFs into your Search Engine and create the first index with AI skills
9. Run 02-LoadCSVOneToMany-ACogSearch.ipynb and:
  - Ingests 52k documents into your Search Engine coming from 1 CSV file
10. **Run the remaining Notebooks in order**. They build up on top of each other.
11. **After you ran all the notebooks**, Go to the apps/ folder and follow the instructions to deploy the Backend Bot and the Frontend UI web applications.

---

## **FAQs**

1. **Why the vector similarity is done in memory using FAISS versus having a separate vector database like RedisSearch or Pinecone?**

A: True, doing the embeddings of the documents pages everytime that there is a query is not efficient. The ideal scenario is to vectorize the docs chunks once (first time they are needed) and then retrieve them from a database the next time they are needed. For this a special vector database is necessary. The ideal scenario though, is Azure Search to save and retreive the vectors as part of the search results, along with the document chunks. Azure Search will soon allow this in a few months, let's wait for it. As of right now the embedding process doesn't take that much time or money, so it is worth the wait versus using another database just for vectors. Once Azure Cog Search gets vector capabilities, the search/retrieval/answer process will be a lot faster.

2. **Why use the MAP_REDUCE type in LangChaing sometimes versus STUFF type everytime?**

A: Because using STUFF type with all the content of the pages as context, in many ocoasions, uses too many tokens. So the best way to deal with large documents is to find the answer by going trough all of the search results and doing many calls to the LLM looking for summarized chunks, then combine this summaries and put them all in the call as context. However as time goes by, the tokens will not be a limitation anymore, GPT-4-32k models is a lot of tokens. Imagine GPT-5 or 6. For more information of the difference between STUFF and MAP_REDUCE, see [HERE](https://github.com/hwchase17/langchain/tree/master/langchain/chains/question_answering)

3. **Why use Azure Cognitive Search engine to provide the context for the LLM and not fine tune the LLM instead?**

A: Quoting the [OpenAI documentation](https://platform.openai.com/docs/guides/fine-tuning): "GPT-3 has been pre-trained on a vast amount of text from the open internet. When given a prompt with just a few examples, it can often intuit what task you are trying to perform and generate a plausible completion. This is often called "few-shot learning.
Fine-tuning improves on few-shot learning by training on many more examples than can fit in the prompt, letting you achieve better results on a wide number of tasks. Once a model has been fine-tuned, you won't need to provide examples in the prompt anymore. This **saves costs and enables lower-latency requests**"

However, fine-tuning the model requires providing hundreds or thousands of Prompt and Completion tuples, which are essentially query-response samples. The purpose of fine-tuning is not to give the LLM knowledge of the company's data but to provide it with examples so it can perform tasks really well without requiring examples on every prompt.

There are cases where fine-tuning is necessary, such as when the examples contain proprietary data that should not be exposed in prompts or when the language used is highly specialized, as in healthcare, pharmacy, or other industries or use cases where the language used is not commonly found on the internet.

## Troubleshooting

Steps to clone a private repo:
- On your Terminal, Paste the text below, substituting in your GitHub email address. [Generate a new SSH key](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent#generating-a-new-ssh-key).
```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```
- Copy the SSH public key to your clipboard. [Add a new SSH key](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent#generating-a-new-ssh-key).
```bash
cat ~/.ssh/id_ed25519.pub
# Then select and copy the contents of the id_ed25519.pub file
# displayed in the terminal to your clipboard
```
- On GitHub, go to **Settings-> SSH and GPG Keys-> New SSH Key**
- In the "Title" field, add a descriptive label for the new key. "AML Compute". In the "Key" field, paste your public key.
- Clone your private repo
```bash
git clone git@github.com:YOUR-USERNAME/YOUR-REPOSITORY.git
```


