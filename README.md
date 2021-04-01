# Introduction
This repository contains code for the paper "RP-DNN: A Tweet level propagation context based deep neural networks for early rumor detection in Social Media" By J. Gao, S. Han, X. Song, etc



## Dataset

The LOO-CV and CV dataset (train set, validation set and test set) are available at our http://staffwww.dcs.shef.ac.uk/people/J.Gao/RPDNN/data/

For all the social context corpus (12 events in total) used in this paper, please download them from https://zenodo.org/record/3249977 and https://figshare.com/articles/PHEME_dataset_for_Rumour_Detection_and_Veracity_Classification/6392078


## Trained Models

Our models are developed with Allennlp framework.

The trained models reported in our paper is available at [figshare project site](https://figshare.shef.ac.uk/articles/Trained_RPDNN_LOO-CV_models_for_early_rumor_detection/11558520) (shef.data.11558520.v1)
 and http://staffwww.dcs.shef.ac.uk/people/J.Gao/RPDNN/models/

Due to limited quote of available space, we release full model and context-only model now.

If you are interested in all the models, please contact us.

## General settings

### Environment

The code and models are developed and tested in following environment with Conda:

* Python 3.6
* CUDA 9.1.85
* cudnn 7.0 (binary-cuda-9.1.85)
* gcc 4.9.4
* conda 4.3.17

Following resources were used to train our models:
* 2 x large-memory nodes, with 2x Intel E5-2630-v3 CPUs and 256GB RAM 
* 2 x NVIDIA Kepler K40M GPUs (Each K40M GPU has 12GB)
* NVIDIA K80 nodes (Each GPU unit have 24 GByes of memory)

### Dependencies
* pandas>=0.23.4
* allennlp==0.8.2
* tqdm>=4.31.1
* gensim
* h5pynltk
* overrides
* regex==2018.01.10

All setting steps can optionally be done in a virtual environment using tools such as ```conda```

### Setting of dependent resource

To use our source code either for training or for loading pre-trained RPDNN model, you need to setup two important resource.

a) ELMo model;

It is recommended to set symlink for elmo model in ```resource/elmo_model/```. Please see a template script 
```symlink_elmo_model.sh``` in the root directory. Alternatively, you can also copy latest model file into this directory

b) social context corpus; 

set symlink for social context directory (organised in 
[PHEME corpus](https://figshare.com/articles/PHEME_dataset_for_Rumour_Detection_and_Veracity_Classification/6392078) structure)
 in ```data/social_context/all-rnr-annotated-threads-retweets```. 
 A template script ```symlink_social_context_directory.sh``` is provided.

## Usage

### Training

Allennlp is used as a library in this project and its Jsonet based configuration is not developed and supported. 
An alternative trainer utility script ```rumour_dnn_trainer.py``` is developed to support training of our Allennlp based model.

Key inputs are:
1) train_set_path ("-t"): train dataset path
2) heldout_set_path("--heldout"): validation dataset path
3) evaluationset("-e"): evaluation (test) dataset
4) model_file_prefix("-p"): set model file prefix name for model weight output file
5) feature_setting("-f"): feature options that used to train or evaluate RPDNN model and have 5 options available
6) max_cxt_size: maximum social context size (default 200)
7) n_gpu ("-g"): gpu device(s) to use (-1: no gpu, 0: 1 gpu). only support int value for device no.
8) epochs: set num_epochs for training

For more settings, please find in the trainer script.

The model will be output into ```output/``` directory by default with timestamp in a new subdirectory.

Example usage:

```$ssh
$ python /RPDNN/src/rumour_dnn_trainer.py -t /data/loocv_set_20191002/sydneysiege/all_rnr_train_set_combined.csv --heldout /data/loocv_set_20191002/sydneysiege/all_rnr_heldout_set_combined.csv -e /data/loocv_set_20191002/sydneysiege/all_rnr_test_set_combined.csv -p "sydneysiege_full" -g 0 -f -1 --max_cxt_size 200 --epochs 10
```

### Evaluation

To test your model or evaluate our trained model, you need to use our evaluator script ```rumour_dnn_evaluator.py```

Key inputs are:
1) testset("-t"): test set csv file path;
2) model("-m"): pre-trained model directory to be evaluated;
3) feature_setting("-f"): feature options that used to train or evaluate RPDNN model and have 5 options available.
                       In test mode, this setting must be the same as the setting in training
4) n_gpu("-g"): gpu device(s) to use (-1: no gpu, 0: 1 gpu). only support int value for device no.

For more settings, please find in the evaluator script.

Example usage:
```$ssh
python /RPDNN/src/rumour_dnn_evaluator.py -t /data/loocv_set_20191002/ferguson/all_rnr_test_set_combined.csv -m /model/RPDNN_model_output_201910/full/ferguson_full201910121555 -g 0 -f -1 --max_cxt_size 200
```


## Contact

* [Jie Gao](http://staffwww.dcs.shef.ac.uk/people/J.Gao/)

* [Sooji Han](https://soojihan.github.io/)
