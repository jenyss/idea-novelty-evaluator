# idea-novelty-evaluator
How novel is your idea? Has someone else already thought of it? How do we judge whether an idea is too wild, too ambitious, or incredibly valuable? Explore all of that with the Idea Novelty Evaluator.

## What It Does
his project evaluates the novelty of a user-submitted idea by retrieving real-world examples of similar concepts from the web using Perplexity. These examples are embedded, clustered based on semantic similarity, and visualized in an interactive 3D space. Each point includes a concise label, with bubble size indicating how closely it aligns with the original idea.

* Each data point is a retrieved piece of content (e.g., snippet, article title, idea).
* The bubble size represents semantic similarity to the idea.
* The cluster/topic reflects thematic grouping (via embedding + clustering).
* X/Y/Z axes are derived from dimensionality reduction (UMAP), meaning clusters may seem close on the graphic (in order to be visualized in 3D) but might not be as close in the high-dimensional embedding space!

**Try it out:** [Google Colab :: Idea Novelty Evaluator](https://colab.research.google.com/drive/1jUIkSO9a671dizPYFAsAWTbCP8VYXi3W#scrollTo=20c40f1d-ba05-462c-bd4f-b7b4158e6650)

<img width="1106" alt="image" src="https://github.com/user-attachments/assets/8e99e41c-b0f0-45b8-b23a-6b2f9a77cfa6" />

<img width="1100" alt="image" src="https://github.com/user-attachments/assets/64569423-717f-42f1-918b-1534e770de84" />


## How It Works

* **Input:** A natural language description of an idea.
* **Retrieval:** Uses the Perplexity API (Sonar model) to return **up to 15** concise, cited examples (snippets + URLs) of how the idea has been realized or discussed. You can adjust it to return more examples if you wish.
* **Summarization:** Each snippet is summarized into a concise 12-word label using OpenAI's GPT-4o.
* **Embedding:** The original idea and all retrieved snippets are embedded using OpenAI's text-embedding-3-large.
* **Clustering:** High-dimensional embeddings are clustered using adaptive KMeans, with the number of clusters determined via silhouette score.
* **Similarity score:** Calculates cosine similarity between each item in the cluster and the original idea (to determine bubble size). The closer to the idea, the bigger the bubble size.
* **Dimensionality Reduction:** Uses UMAP to project the high-dimensional embeddings into 3D for visualization.
* **Visualization:** A 3D scatter plot is rendered using Plotly, with:
  * X/Y/Z = UMAP-projected coordinates
  * Color = Cluster assignment
  * Size = Semantic similarity to the idea (larger bubbles = more similar)
  * Hover = Shows summary and similarity
* **Heatmap:** _(uncommnet ```show_distance_heatmap(other_emb, labels)``` to view)_ A cosine distance heatmap of the embeddings is displayed to show semantic relationships.
* **Table Output:** A clickable HTML table is rendered under the plot showing:
  * Summary (label)
  * Full snippet
  * Source (URL)
  * Cluster ID
  * Similarity score


### Tools
* Perplexity API (```sonar``` | ```sonar-deep-research```): For real-time web search & citation retrieval
* OpenAI (```gpt-4o```): For converting unstructured Perplexity output into a list and generating summaries (labels)
* OpenAI Embeddings: (```text-embedding-3-large```): For semantic similarity encoding
* Scikit-learn: For cosine similarity calculation, silhouette scoring, and clustering via KMeans
* UMAP: For dimensionality reduction into 3D space
* Plotly: For interactive 3D visualization
* Seaborn / Matplotlib: For generating cosine distance heatmap
* Pandas / HTML: For structured tabular output and display

## How-To

**Option 1:** Run in Colab

- Open the provided [Colab notebook](https://colab.research.google.com/drive/1jUIkSO9a671dizPYFAsAWTbCP8VYXi3W#scrollTo=20c40f1d-ba05-462c-bd4f-b7b4158e6650)
- Add your API keys in the Colab secrets (e.g., PERPLEXITY_SONAR_API_KEY, OPENAI_API_KEY)
- Run the notebook cells

**Option 2:** Run Locally

- Clone this repo and open the uploaded notebook in Jupyter or VSCode
- Create a .env file with your API keys:
  - PERPLEXITY_SONAR_API_KEY=your_key
  - OPENAI_API_KEY=your_key
- Run the notebook. The required dependencies are in the notebook.

**Perplexity model configuration**

The default model is ```sonar-deep-research```, which provides higher-quality results but may incur increased API costs.
To use the faster, lower-cost model for testing, change the ```model``` parameter default in the ```get_perplexity_results``` function to ```model: str = "sonar"```
