# Amino-a-Go-Go

Amino-a-Go-Go is a dataset of high-quality samples for protein function prediction. It is derived from the SwissProt dataset and contains amino acid sequences annotated with their corresponding gene ontology (GO) terms. The samples are divided into three subsets each containing a set of GO terms that are associated with one of the three subgraphs of the gene ontology - `Molecular Function`, `Biological Process`, and `Cellular Component`. In addition, we provide a stratified `train`/`test` split that utilizes  latent subgraph embeddings to distribute GO term annotations equally.

## Processing Steps

- Filter samples for high-quality empirical evidence codes.
- Remove duplicate GO term annotations.
- Expand GO term annotations to include the entire GO subgraph.
- Filter samples annotated with only root terms (i.e "biological process").
- Create embeddings of each sample's GO subgraph.
- Assign stratum IDs to the samples in the dataset.

## References

>- The UniProt Consortium, UniProt: the Universal Protein Knowledgebase in 2025, Nucleic Acids Research, 2025, 53, D609–D617.