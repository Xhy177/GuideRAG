# GuideRAG

**Planning-Guided, Intent-Conditioned Graph Navigation for Retrieval-Augmented Generation**

Haoyang Xu, Yanhua Yu, Feng Luo, Zhiyong Huang, Shuai Luo
*Beijing University of Posts and Telecommunications · China Mobile Group Design Institute · National University of Singapore*

GuideRAG is a graph-based RAG framework for multi-hop question answering. Rather than expanding a query-agnostic evidence graph, it first extracts a lightweight **retrieval plan** — task decomposition, anchor entities, and an expected type chain — and uses that plan to steer multi-hop evidence discovery, so expansion follows the query's intent instead of drifting into noisy regions.

## How it works

1. **Query planning** — decompose the query into sub-queries, each with anchors and a hop-wise expected type chain.
2. **Type-guided, evidence-mediated expansion** — primary (path-defining) and supplementary (recall-oriented) anchors start the walk; query-relevant segments in an auxiliary entity–segment index mediate evidence transfer to new entities under soft type guidance.
3. **Consolidation** — dense passage relevance is merged with accumulated entity evidence via Personalized PageRank on a relation-free passage–entity graph; per-sub-query rankings are normalized and combined.

No relation extraction is required, which keeps the pipeline cheap and robust on graphs built from raw text.

## Results

Five benchmarks (NQ, PopQA, HotpotQA, 2WikiMultiHopQA, MuSiQue), BGE-M3 embeddings, GPT-4o-mini, top-5 passages:

| Method | Avg F1 | Avg Acc |
| --- | --- | --- |
| HippoRAG2 | 58.1 | 61.2 |
| **GuideRAG** | **60.3** | **63.0** |

On MuSiQue, GuideRAG reaches 40.9 F1 with 1.52M prompt tokens, 1,414 s indexing and 269 s retrieval — roughly 10× faster retrieval and 7× fewer tokens than HippoRAG2.

## Dataset

`dataset/` contains the fixed 1,000-question evaluation subset and its retrieval corpus for each benchmark used in the paper:

| Benchmark | Questions | Corpus chunks | Hops |
| --- | --- | --- | --- |
| Natural Questions (NQ) | 1,000 | 1,636 | single |
| PopQA | 1,000 | 1,322 | single |
| HotpotQA | 1,000 | 1,525 | multi |
| 2WikiMultiHopQA | 1,000 | 765 | multi |
| MuSiQue | 1,000 | 1,579 | multi |

Each folder holds `questions.json` (questions with gold answers and evidence) and `chunks.json` (the retrieval corpus, as `"<id>:<text>"` strings). The data is derived from the public benchmarks above — please respect their original licenses when redistributing.

## Code

The implementation is being cleaned up and **will be open-sourced in this repository**. Until then, the dataset above is fully usable on its own; watch/star the repo to be notified.

## Citation

If you find this work useful, please cite the paper. A BibTeX entry will be added once the official version is available.

## Acknowledgement

Supported by the BUPT–China Mobile Joint Institute, and by the National Natural Science Foundation of China (Grant No. U22B2019).
