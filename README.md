## BERTopic Hippocorpus Labeling Pipeline

This repository has one purpose:

1. Configure BERTopic mode and load the full Hippocorpus.
2. Optionally use BERTopic's native LLM representation model for topic naming.
3. Fit BERTopic and export topic outputs.
4. Optionally run a post-fit LLM suggestion/review workflow for topic names.
5. Run regex rules on the full Hippocorpus to create labels.
6. Append labels to any reduced datasets by statement index.

### Project Structure

- `notebooks/topicExploration.ipynb`: simple end-to-end pipeline notebook.
- `data/hippocorpus.csv`: full source dataset.
- `data/topic_info.csv` / `data/topic_info_k_means.csv`: BERTopic topic summary for manual inspection.
- `data/topic_info_named.csv` / `data/topic_info_k_means_named.csv`: optional reviewed topic names.
- `data/topic_name_suggestions.csv`: optional post-fit LLM name suggestions.
- `data/hippocorpus_labeled.csv`: full dataset with `topic_id` and `topic_label`.
- labeled reduced outputs: generated from the manual `APPEND_TARGETS` list in the notebook.

### Miniconda Setup

```bash
conda env create -f environment.yml
conda activate bertopic-hippocorpus
```

### Run

Open and run:

- `notebooks/topicExploration.ipynb`

Run cells top-to-bottom.

### License

This project is licensed under the MIT License. See the LICENSE file for details.

### Pipeline Summary

1. Configure and load data in the notebook (`TOPIC_MODE`, paths, and `APPEND_TARGETS`).
2. Optional (BERTopic-native): enable `USE_BERTOPIC_LLM_REPRESENTATION = True` to name topics during modeling.
3. Fit BERTopic on `data/hippocorpus.csv` text, using one mode:
   - `hdbscan`: BERTopic + reduce outliers, outputs `output.csv` and `topic_info.csv`.
   - `kmeans`: BERTopic + KMeans fixed clusters, outputs `output_k_means.csv` and `topic_info_k_means.csv`.
4. Optional alternative (post-fit review loop): call an OpenAI-compatible LLM API to suggest topic names to `topic_name_suggestions.csv`, then apply reviewed suggestions to export `topic_info_named.csv` or `topic_info_k_means_named.csv`.
5. Apply regex rules to create `topic_id` and `topic_label` for the full Hippocorpus.
6. Merge labels by statement index key:
   - `hippocorpus.index` -> `<reduced_csv>.<index_col>`

For either optional LLM path, set `OPENAI_API_KEY` in your environment before running related cells.
If using the BERTopic-native representation step, install OpenAI client support (`pip install openai`) in your environment.
