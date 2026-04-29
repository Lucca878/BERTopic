## BERTopic Hippocorpus Labeling Pipeline

This repository has one purpose:

1. Run BERTopic on the full Hippocorpus.
2. Reduce outliers.
3. Inspect resulting topics and define regex rules.
4. Run regex rules on the full Hippocorpus to create labels.
5. Append labels to any reduced datasets by statement index.

### Project Structure

- `notebooks/topicExploration.ipynb`: simple end-to-end pipeline notebook.
- `data/hippocorpus.csv`: full source dataset.
- `data/topic_info.csv`: BERTopic topic summary for manual inspection.
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

### Pipeline Summary

1. Fit BERTopic on `data/hippocorpus.csv` text.
2. Choose fit mode in the notebook:
	- `hdbscan`: BERTopic + reduce outliers, outputs `output.csv` and `topic_info.csv`.
	- `kmeans`: BERTopic + KMeans fixed clusters, outputs `output_k_means.csv` and `topic_info_k_means.csv`.
3. Apply regex rules to create `topic_id` and `topic_label` for the full Hippocorpus.
4. In the notebook, set `APPEND_TARGETS` with the files you want to label.
5. Merge labels by statement index key:
	- `hippocorpus.index` -> `<reduced_csv>.<index_col>`
