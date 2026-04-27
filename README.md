# G2PTransformer

This repository is a submission for LELA60332 course. It contains code and results for a G2P (grapheme-to-phoneme) Sanskrit-Devanagari and Hindi-Devanagari transformer. 

## Repository Structure 
The code is structured into (almost) ready-to-run environments, with slurm scripts to test on the CSF if applicable (although training and evaluating is easily reproducible locally due to low memory requirements, ~1-2GB RAM). Terminal/slurm logs are anonymised and are situated in the results section. Relevant datasets are downloaded from the SIGMORPHON G2PST repository when running ```train.py```. 

## Requirements 
Please note that to run the code, the following requirements are needed:
```
python>=3.10
torch>=2.0
tqdm>=4.60
panphon>=0.20
```
## How To Run 
- On CSF: Download or clone. Change directory. Make sure that the dependencies are in place. Use ```jobscript.slurm``` to run ```train.py```, use relevant ```jobscripteval.slurm```/```jobscriptevalnep.slurm``` to run the ```eval.py``` script.
- Locally: Download or clone. Change directory. Make sure the dependencies are in place. Run in terminal via ```uv run python train.py``` to begin training, perform evaluation by ```uv run python eval.py```. Default evaluation happens on the ```test.tsv``` dataset.

Other evaluation scripts such as ```evalnep.py```/```evalhin.py``` download Nepali or Hindi data if you'd wish to recreate evaluation on the OOD dataset.  
## Model 
The model is a lightweight modernised transformer using scaled dot-product multi-head attention with rotary positional embeddings (RoPE) and grouped key/value heads and weight-tying implemented. 
## Training 

## Evaluation 
Evaluation happens during training, with greedy decoding used for every epoch and beam search decoding used every five epochs to save computation. 
