# Experiments

## Natural Language Processing
> **Name Entity Recognition** (Extract meta/ structured data from a long piece of text). Actually the cases we showed are pipelines containing 2 steps:
* NER task: Finds out the region of the text
* Classification: Finds out what the underlined text is.

### Example
> This following example is tring to find the drug. For now, other targets like Gene, Mutation, Diseases also works in certain accuracy

```python
from gc_lab.drug_norm import DrugNorm
dn = DrugNorm.from_db()
print(dn.find_drug("The drugs like Bicalutamide, famitinib and Palbociclib."))
```

this will output:
```python
["Bicalutamide", "Famitinib", "Palbociclib"]
```

Which matches the knowledge base records from Genomicare
### Text Generation
* Text generation: solve most NLP as text generation problem (GPT, GPT2 based)

## MRI Image
* Brain tumor type [classifications and feature visualization](tumor_type_classification_and_its_implications.html) using MRI images (T1 enhanced open dataset)
* Brain tumor [region prediction](region_prediction.html) using MRI images (T1 enhanced open dataset)
![mri_img1](imgs/mri_region.jpeg =100x) ![mri_img2](imgs/mri_region2.jpeg =100x) ![mri_img3](imgs/mri_region3.jpeg =200x)
## Chemicals
* [SMILES string](https://en.wikipedia.org/wiki/Simplified_molecular-input_line-entry_system) precict mutagenecity (one of the toxity), [prediction on CKB drugs](toxity_prediction_ames.html)
![smiles string example](imgs/SMILES.png)
