# AmiGO Dataset

AmiGO is a friendly dataset of high-quality human-curated samples for protein function prediction. It is derived from the UniProt database and contains amino acid sequences annotated with their corresponding gene ontology (GO) terms. The samples are divided into three subsets each containing a set of GO terms that are associated with one of the three subgraphs of the gene ontology - `Molecular Function`, `Biological Process`, and `Cellular Component`. In addition, we provide a stratified `train`/`test` split that utilizes latent subgraph embeddings to distribute GO term annotations equally.

## Processing Steps

- Filter high-quality empirical evidence codes.
- Remove duplicate GO term annotations.
- Expand annotations to include the entire GO subgraph.
- Embed subgraphs and assign stratum IDs to the samples.
- Generate stratified train/test split.

## Subsets

The [AmiGO](https://huggingface.co/datasets/andrewdalpino/AmiGO) dataset is available on HuggingFace Hub and can be loaded using the HuggingFace [Datasets](https://huggingface.co/docs/datasets) library. 

The dataset is divided into three subsets according to the GO terms that the sequences are annotated with.

- `all` - All annotations
- `mf` - Only molecular function terms
- `cc` - Only celluar component terms
- `bp` - Only biological process terms

To load the default AmiGO dataset with all function annotations you can use the example below.

```python
from datasets import load_dataset

dataset = load_dataset("andrewdalpino/AmiGO")
```

To load a subset of the AmiGO dataset use the example below.

```python
dataset = load_dataset("andrewdalpino/AmiGO", "mf")
```

## Splits

We provide a 90/10 `train` and `test` split for your convenience. The subsets were determined using a stratified approach which assigns cluster numbers to sequences based on their terms embeddings. We've included the stratum IDs so that you can generate additional custom stratified splits as shown in the example below.

```python
from datasets import load_dataset

dataset = load_dataset("andrewdalpino/AmiGO", split="train")

dataset = dataset.class_encode_column("stratum_id")

dataset = dataset.train_test_split(test_size=0.2, stratify_by_column="stratum_id")
```

## Filtering

You can also filter the samples of the dataset like in the example below.

```python
dataset = dataset.filter(lambda sample: len(sample["sequence"]) <= 2048)
```

## Tokenizing

Some tasks may require you to tokenize the amino acid sequences. In this example, we loop through the samples and add a `tokens` column to store the tokenized sequences.

```python
def tokenize(sample: dict): list[int]:
    tokens = tokenizer.tokenize(sample["sequence"])

    sample["tokens"] = tokens

    return sample

dataset = dataset.map(tokenize, remove_columns="sequence")
```

## References

>- The UniProt Consortium, UniProt: the Universal Protein Knowledgebase in 2025, Nucleic Acids Research, 2025, 53, D609–D617.