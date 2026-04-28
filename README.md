# G2PTransformer

This repository is a submission for LELA60332 course. It contains code and results for a G2P (grapheme-to-phoneme) Sanskrit-Devanagari and Hindi-Devanagari transformer. The model is trained to be able to generate the correct IPA representation for a given input word in the original Sanskrit-Devanagari or Hindi-Devanagari. 

### Repository Structure 
The repo is divided into (almost) ready-to-run environments, with slurm scripts to test on the CSF if applicable (although training and evaluating is easily reproducible locally due to low memory requirements of around 1-2GB RAM). Terminal/slurm logs are anonymised, are situated in the results section, and detail the training process and results. Relevant datasets are downloaded from the SIGMORPHON G2PST repository when running ```train.py``` or evaluation scripts for OOD testing. 

### Requirements 
Please note that to run the code, the following requirements are needed:
```
python>=3.10
torch>=2.0
tqdm>=4.60
panphon>=0.20
```
### How To Run 
- On CSF: Download or clone. Make sure that the dependencies are in place. Use ```jobscript.slurm``` to run ```train.py```, use ```jobscripteval.slurm``` to run the ```eval.py``` script. 
- Locally: Download or clone. Make sure the dependencies are in place. Run in terminal via ```uv run python train.py``` to begin training, perform evaluation by ```uv run python eval.py```. Default evaluation happens on the ```test.tsv``` dataset, which depending on the environment used is either Sanskrit or Hindi.

Other evaluation scripts such as ```evalnep.py```/```evalhin.py``` download Nepali or Hindi data if you'd wish to recreate evaluation on the OOD dataset.  
### Model 
The model is a lightweight modernised transformer using scaled dot-product multi-head attention with rotary positional embeddings (RoPE) and grouped key/value heads and weight-tying implemented. 
### Training 
Training script includes all utilities for downloading, preprocessing, and tokenising the data. Training is default set at 50 epochs and early stopping with patience of 15. 

Evaluation happens during training to be able to save the best model, with greedy decoding used for every epoch and beam search decoding used every five epochs to save computation. Every epoch, the model is evaluated on greedy PER and tested for exact match on 10 randomly picked samples from the valuation set. 
### Evaluation 
Evaluation scripts include Levenshtein, weighted PER (WPER), Hamming distance, and exact match. These are calculated based on PanPhon's feature tables for phonemes, to be able to differentiate the severity of mistakes. During evaluation, only beam search decoding is used to get the best possible result. The script prints an overview, including top 10 mistakes (deletion, substitution, hallucination). 
### Results
The model is overall more accurate for Hindi (87% exact match) than Sanskrit (46% exact match), although PER metrics for both indicate mistakes are non-severe. OOD generalisation for Nepali is poor regardless of whether the model was pre-trained on Hindi or Sanskrit. 
