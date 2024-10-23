# ConBias
This repository contains code for the paper titled "Visual Data Diagnosis and Debiasing with Concept Graphs", to appear in _Advances in Neural Information Processing Systems (NeurIPS), 2024._

_**Abstract**_ - 
_The widespread success of deep learning models today is owed to the curation of extensive datasets significant in size and complexity. However, such models frequently pick up inherent biases in the data during the training process, leading to unreliable predictions. Diagnosing and debiasing datasets is thus a necessity to ensure reliable model performance. In this paper, we present CONBIAS, a novel framework for diagnosing and mitigating Concept co-occurrence Biases in visual datasets. CONBIAS represents visual datasets as knowledge graphs of concepts, enabling meticulous analysis of spurious concept co-occurrences to uncover concept imbalances across the whole dataset. Moreover, we show that by employing a novel clique-based concept balancing strategy, we can mitigate these imbalances, leading to enhanced performance on downstream tasks. Extensive experiments show that data augmentation based on a balanced concept distribution augmented by CONBIAS improves generalization performance across multiple datasets compared to state-of-the-art methods._

## Datasets

We use [Waterbirds](https://github.com/anniesch/jtt/tree/master), [UrbanCars](https://github.com/facebookresearch/Whac-A-Mole), and [Coco-GB](https://github.com/datamllab/Mitigating_Gender_Bias_In_Captioning_System). Modify the paths in the scripts below to the location where you have saved these datasets. 


## Cliques

In the metadata directory we already provide the files needed to construct the imbalanced clique sets.

For each dataset, we have:

1. ```clique_dict_final_coco.pkl```: The cliques for COCO-GB 
2. ```clique_dict_final.pkl```: The cliques for Waterbirds
3. ```clique_dict_final_urbancars.pkl```: The cliques for Urbancars

To create the imbalanced clique set, we can simply run:
```
python src/concept_sampler.py --clique_file_name
```
The output of this file is a .json file containing the concept combinations to be up-sampled. 

## Concepts 

The metadata directory contains these files as well:

1. ```concepts_generation.json```: Concepts to be sampled for Waterbirds.
2. ```concepts_generation_coco.json```: Concepts to be sampled for COCO-GB.
3. ```concepts_generation_urbancars.json```: Concepts to be sampled for UrbanCars.
   
To create the co-occurrences, we use ```src/co_occurence.py``` and ```src/co_occurrence_cliques.py```. 

## Co-occurrences

The co-occurrence code for Waterbirds, COCO-GB, and UrbanCars are a bit different due to 
the different nature of the metadata, i.e. we need different ways to extract the concepts stored
in the annotations. Therefore, we also share the different csv files for each dataset, i.e. 
```co_occurrence_matrix_coco.csv```, ```co_occurrence_matrix_urbancars.csv```, ```co_occurrence_matrix_waterbirds.csv```.
For these particular datasets, the csv files and the code are not required.

## Training and evaluation

To train ConBias, run
```
python src/train.py --dataset <datname> --augmentation --method conbias --checkpoint_path <ckpt_path>
```
To evaluate CB, run
```
python src/evaluate.py --checkpoint_path <ckpt_path>
```
To evaluate OOD, run
```
python src/evaluate.py --checkpoint_path <ckpt_path> --type ood
```

NOTE: <ckpt-path> is the checkpoints saved for the base resnet model. 
NOTE: ```src/dataloaders.py``` needs to be modified with the actual dataset path on machine. 

## Citation

If you found our work useful, please consider citing it:

```
@article{Chakraborty2024VisualDD,
  title={Visual Data Diagnosis and Debiasing with Concept Graphs},
  author={Rwiddhi Chakraborty and Yinong Oliver Wang and Jialu Gao and Runkai Zheng and Cheng Zhang and Fernando De la Torre},
  journal={ArXiv},
  year={2024},
  volume={abs/2409.18055},
  url={https://api.semanticscholar.org/CorpusID:272910737}
}
```

