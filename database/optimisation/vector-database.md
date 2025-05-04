# Vector Database

**Structured vs. Unstructured Data**

* **Structured data** is highly organized, stored in tables with predefined schemas (e.g., numbers, dates, and categories).
* **Unstructured data** lacks a fixed format and includes **text, images, audio, and video**, making it harder to store and analyze efficiently.

**What Are Vectors?**

* In **linear algebra**, a vector is a numerical list with a defined direction.
* In **AI & ML**, vectors (also called **embedding vectors**) transform raw data into a numerical format, allowing for similarity searches across different types of content.

**Why Store Vectors?**

* Storing and searching unstructured data is challenging due to **high dimensionality and large volumes**.
* **Vectorization** improves **storage, indexing, and search efficiency**, enabling **faster similarity searches** for applications like recommendation systems, NLP, and image retrieval.

**How ClickHouse Stores Vectors**

* ClickHouse supports vector storage using **tuple** or **array** data types.
* **Tuple-based storage** is useful for fixed-length vectors.
* **Array-based storage** allows **variable-length vectors**, providing flexibility in handling different models and embeddings.

This enables ClickHouse to efficiently store and process vector data for high-performance analytics and search applications

## Indexing Vector Fields

There are two primary approaches to searching for similar vectors:

1. **Brute-force search using distance functions** – Highly accurate but slower.
2. **Indexing vector fields** – Faster but less precise.

ClickHouse experimentally supports **Approximate Nearest Neighbor (ANN) Search Indexes** in **MergeTree** table engines.&#x20;

Now, let’s explore a real-world example of storing and searching for vectors in ClickHouse.

### Storing and Searching Similar Vectors in ClickHouse

ClickHouse is known for its lightning-fast analytics on structured data, but with the rise of AI and machine learning, the demand for efficient **vector search** has grown. ClickHouse now supports **vector similarity indexing** using the **HNSW (Hierarchical Navigable Small World) algorithm**, enabling high-performance approximate nearest-neighbor (ANN) searches.

## **Why Vector Indexing Matters**

Handling unstructured data like images, audio, and text often involves converting it into **embedding vectors**. Searching for similar vectors efficiently requires specialized indexing structures rather than brute-force comparisons.

### **1. Enabling Vector Indexing**

Before creating a vector similarity index, you need to enable the experimental feature:

```
SET allow_experimental_vector_similarity_index = 1;
```

### **2. Creating a Table with Vector Index**

This example creates a table to store vectors and applies an HNSW-based similarity index.

```sql
CREATE TABLE image_embeddings (
  id Int64,
  embedding Array(Float32),
  INDEX vector_idx embedding TYPE vector_similarity('hnsw', 'L2Distance')
)
ENGINE = MergeTree
ORDER BY id;
```

### **3. Inserting Vector Data**

Insert some sample data representing vectors

```sql
INSERT INTO image_embeddings (id, embedding) VALUES
(1, [0.12, 0.34, 0.56, 0.78]),
(2, [0.98, 0.76, 0.54, 0.32]),
(3, [0.23, 0.45, 0.67, 0.89]);
```

### **4. Performing a Vector Similarity Search**

Find the top 5 most similar vectors to a given reference vector.

```sql
WITH [0.15, 0.35, 0.55, 0.75] AS reference_vector
SELECT id, embedding
FROM image_embeddings
ORDER BY Distance(embedding, reference_vector)
LIMIT 5;
```

### **5. Adjusting Search Performance**

To improve recall and search speed, adjust the `hnsw_candidate_list_size_for_search` parameter.

```sql
WITH [0.15, 0.35, 0.55, 0.75] AS reference_vector
SELECT id, embedding
FROM image_embeddings
ORDER BY Distance(embedding, reference_vector)
LIMIT 5
SETTINGS hnsw_candidate_list_size_for_search = 512;
```

#### **Performance Considerations**

* **Indexing Speed:** HNSW indexing can slow down `INSERT` and `OPTIMIZE` operations.
* **Parallelization:** Use `max_build_vector_similarity_index_thread_pool_size`to speed up index creation.
* **Managing Merges:** Suppressing merges (`SYSTEM STOP MERGES`) before bulk inserts can prevent redundant index builds.
* **Granularity:** Setting `GRANULARITY` determines how many sub-indexes are created, impacting search efficiency.

#### **Why ClickHouse for Vector Search?**

* **Scalability:** Supports massive datasets with efficient ANN search.
* **SIMD Optimization:** Leverages CPU acceleration for faster distance computations.
* **Seamless Integration:** Works alongside ClickHouse’s analytical capabilities.

