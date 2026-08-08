## Installation

```javascript
npm install chromadb@1.9.1
npm install chromadb-default-embed
```

###  Docker

```bash
sudo apt update 
sudo apt install ca-certificates 
sudo update-ca-certificate 
docker system prune -a
docker pull chromadb/chroma:0.4.24
```
---
# Comparison with traditional DBs

| Feature | Traditional DB | Vector DB |
|---------|----------------|-----------|
| Data Representation | Tables, rows, columns | Multi-dimensional vectors |
| Search | Exact matching (SQL) | Similarity & nearest-neighbor search |
| Indexing | B-tree | Metric trees, LSH, ANN |
| Best For | Structured data | AI, embeddings, unstructured data |
| Scalability | Transaction-focused | Large-scale similarity search |

---
# Characteristics

## Specialized Data Structures
- Reversed Indexes
- Product Quantization (PQ)
- Locality-Sensitive Hashing (LSH)

## Vector Operations
- Nearest Neighbor Search (NN)
- Similarity Search
- Distance Calculations

---

# Disk-Based Vector Databases

## Features
- Store vectors on disk
- Suitable for very large datasets
- Use indexing & compression techniques

## Examples
- Annoy
- Milvus
- FAISS

---

# In-Memory Vector Databases

## Features
- Store vectors directly in RAM
- Very fast read/write operations
- Ideal for real-time analytics & recommendation systems

## Examples
- RedisAI
- TorchServe

---
# Applications

- **Biology** → Climate & biological data analysis
- **Healthcare** → Patient outcome prediction
- **E-commerce** → Product recommendations
- **Social Media** → Friend/content suggestions
- **Traffic Planning** → Traffic analysis & optimization

---
## Collection
A container (like table in dbms) that:
- Stores vector embeddings
- Holds documents
- Manages metadata

---
## Embedding
Numerical representation of data

```text
Cat: [0.23,0.55,0.89,0.10...]
Dog: [0.24,0.53,0.86,0.12...]
```

Since they both have similar numbers or embeddings, it means they are similar.

---
## Resolving LLM limitations
- The DB displays data objects as vectors
- Each data object contains features represented as dimensions
- The DB views the object's dimension numbers
- The DB determines the closeness, or similarity, of the objects
---
## Semantic Searching
Very important in LLMs, searches for similarities like categories so if prompt is I need smth to drink, it knows how everything is related

--- 
## Working of VectorDBs

```text
Data -> Convert into vectors -> Vector has num dims -> DB comps vector vals and finds closest vector -> Return similar objects
```

---
## Program 

```javascript
const {
    ChromaClient,
    DefaultEmbeddingFunction
} = require("chromadb");

const client = new ChromaClient(); //Create an instance of ChromaClient.

const defaultEmbedding =
    new DefaultEmbeddingFunction(); // Converts text into vectors (embeddings).

const COLLECTION_NAME =
    "my_basic_collection"; //like SQL table


const ids = [
    "1",
    "2",
    "3",
    "4"
];

const documents = [

    "Coffee is a beverage.",

    "Tea is another popular drink.",

    "Apple is a healthy fruit.",

    "Laptop is an electronic device."

];

const metadatas = [

    {
        category: "Drink"
    },

    {
        category: "Drink"
    },

    {
        category: "Fruit"
    },

    {
        category: "Electronics"
    }

];
// getOrCreateCollection(): Create a new collection or open if exists

async function createCollection() {
    console.log("\nCreating / Opening Collection...\n");

    const collection =
        await client.getOrCreateCollection({
            name: COLLECTION_NAME
        });
    console.log("Collection Ready.");
    return collection;
}

//add stuff
async function addDocuments(collection) {
    console.log("\nAdding Documents...\n");
    await collection.add({ ids, documents, metadatas });
    console.log("Documents Added Successfully.");
}


async function getDocuments(collection) {
    console.log("\nRetrieving Documents...\n");
    const data = await collection.get();
    console.log(data);
    return data;
}


async function generateEmbeddings() {
    console.log("\nGenerating Embeddings...\n");
    const embeddings =
        await defaultEmbedding.generate(documents);
    console.log(embeddings);
    return embeddings;

}

async function searchDocuments(collection) {
    console.log("\nPerforming Similarity Search...\n");
    const queryText = "Coffee"; //conv query to vectors
    const results = await collection.query({
        queryTexts: [queryText],
        n: 3 //no of nearest docs
    });
    console.log("Similarity Search Results\n");
    console.log(results);
    return results;
}

async function main() {
    try {
        console.log("\nHi");
        const collection = await createCollection(); //Create/Open Collection
        await addDocuments(collection); // Insert Documents
        await getDocuments(collection);// Retrieve Documents
        await generateEmbeddings();// Generate Embeddings 
        await searchDocuments(collection);// Perform Similarity Search
        console.log("\nFinished");
    }
    catch (error) {
        console.error(error);
    }
}

main();

```

---
## ChromaDB APIs

|API|Purpose|
|---|---|
|`new ChromaClient()`|Connect to ChromaDB|
|`getOrCreateCollection()`|Create or open a collection|
|`collection.add()`|Insert documents|
|`collection.get()`|Retrieve documents|
|`collection.query()`|Perform similarity search|
|`DefaultEmbeddingFunction()`|Create embeddings|
|`generate()`|Convert text into vectors|


---

