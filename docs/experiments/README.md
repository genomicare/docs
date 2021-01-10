# Experiments
> This page summarize various trying of utilizing machine learning 

## Natural Language Processing
> A comprehensive Chrome extension utilize many of the NLP tech like NER, Q&A, medical translation

<iframe width="560" height="315" src="https://www.youtube.com/embed/-lxs1R08DNQ" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Then we break it down piece by piece

### **Name Entity Recognition**

> NER task: Extract meta/ structured data from a long piece of text. Actually the cases we showed are pipelines containing 2 steps:
* NER task: Finds out the region of the target(underline phrases within sentences)
* Classification: Finds out what the underlined text is.

#### NER Example
> This following example is tring to find ```drug``` with text. For now, other targets like ```Gene, Mutation, Diseases``` also works in certain accuracy

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

#### Question & Answering
For text too long, and you just want a quick answer within the text, you can just type in the equestion, and model will under line an answer. Example see SAPERE AUDE demo video above

#### Text Generation
* Text generation: solve most NLP as text generation problem (GPT, GPT2 based)

#### Machine Translation
> Currently we're using commercial API from other big tech on this subject, but translation.

## Graph Learning

## Computer Vision
### Image Data Privicy Protection
> Redacting patient information with OCR tech by key words

### MRI Image
* Brain tumor type [classifications and feature visualization](tumor_type_classification_and_its_implications.html) using MRI images (T1 enhanced open dataset)
* Brain tumor [region prediction](region_prediction.html) using MRI images (T1 enhanced open dataset)

<img src="imgs/mri_region.jpeg" width=200>
<img src="imgs/mri_region2.jpeg" width=200>
<img src="imgs/mri_region3.jpeg" width=360>

## Organic Compond Property Prediction
> We can perform multiple target prediction on a single organic compond mostly chemicals with Mol mass below 1000.

* [SMILES string](https://en.wikipedia.org/wiki/Simplified_molecular-input_line-entry_system) precict mutagenecity (one of the toxity), [prediction on CKB drugs](toxity_prediction_ames.html)
![smiles string example](imgs/SMILES.png)
