# Banking Transactions Analysis

## Running Neo4j Enterprise on Docker

Use the following command to run Neo4j Enterprise on Docker. Update the volume paths for Neo4j's data and logs folders accordingly.

```
docker run \
    --env=NEO4J_ACCEPT_LICENSE_AGREEMENT=yes \
    --restart always \
    --publish=7474:7474 --publish=7687:7687 \
    --volume=/path/to/data/folder:/data \
    --volume=/path/to/log/folder:/logs \
    neo4j:5.8.0-enterprise
```

Once the container is running, open the following link on your browser to see the Neo4j Browser:

```
http://localhost:7474
```
## Datasets used

This project uses datasets from [banking - Customer & Transaction data](https://gist.github.com/maruthiprithivi/f11bf40b558879aca0c30ce76e7dec98). For simplicity, the datasets are also included in this repo:

  - [customers.csv](./datasets/customers.csv)
  - [purchases.csv](./datasets/purchases.csv)
  - [transfers.csv](./datasets/transfers.csv)


## Data Model

The data is modeled using [arrows.app](https://arrows.app/), which is available in JSON format([Bank_transaction_purchase.json](./Bank_transaction_purchase.json)).

The following image illustrates it:

![title](./images/Bank_transaction_purchase.png)

## Loading the data into Neo4j

This project uses Python to cleanse and load the data. The script is written as a Jupyter Notebook ([banking_ingestion.ipynb](./banking_ingestion.ipynb)) to make it easier to follow. Please use [Nbviewer](https://nbviewer.org/github/wicaksana/neo4j-banking-transactions-analysis/blob/main/banking_ingestion.ipynb) to open the notebook in case Github fails to render it.

Environment specification:

- Python version: 3.10.9
- Package `neo4j` version: 5.9.0
- Pandas version: 1.5.3

## Exploratory Cypher queries

[queries.ipynb](./queries.ipynb)