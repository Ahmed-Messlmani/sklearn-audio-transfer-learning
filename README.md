# Audio Transfer Learning with Scikit-learn and Tensorflow
We use the [VGGish pre-trained Tensorflow model](https://github.com/tensorflow/models/tree/master/research/audioset) as an audio feature extractor. This deep convolutional neural network was trained with the [Audioset dataset](https://research.google.com/audioset/), that is conformed by 2M YouTube audios for the task of general audio tagging. Scikit-learn classifiers are employed to rapidly prototype competent audio classifiers that can be trained on a CPU.

See the pipeline:
<p align="center"><img src="scikit-learn-transfer-learning.png"></p>

This material is prepared to learn about Tensorflow, Scikit-learn, and deep learning in general. Besides, due to the simplicity of scikit-learn, this toolkit can be employed to easily build proof-of-concept models from small training sets. As small (or big!) as your hardware allows! See this [article](https://arxiv.org/abs/1810.10274) to know more on using audio transfer learning in low-data regimes. 

## Installation:
Create a python3 virtual environment `python3 -m venv env`, activate it `source ./env/bin/activate`, and install the dependencies `pip install -r requirements.txt`.

#### Download the the VGGish model:
Download the pre-trained model `wget https://storage.googleapis.com/audioset/vggish_model.ckpt` in `sklearn-audio-transfer-learning/src/`.

## Music genre classification: a toy example

#### Setup the task
As an example, let's download the GTZAN dataset `wget http://opihi.cs.uvic.ca/sound/genres.tar.gz` and extract the audio files `tar -zxvf genres.tar.gz` in `sklearn-audio-transfer-learning/data/audio/GTZAN/`. Approximated download time: between 30 min and an hour. We already provide (fault-filtered) train/test partitions in `sklearn-audio-transfer-learning/data/index/GTZAN/`.

`sklearn_audioset.py` is the main python file. Note that on its top-20 lines you can configure it. It is important that you set your data folder, in my case: `DATA_FOLDER = '/home/jordipons/transfer-learning-tutorial/data/'`.

You can also set some parameters. For example, you can select which sklearn classifier to employ. Sklearn has tones of possibilities! You can easily set your favourite sklearn classifier in `define_classification_model()`. To start, let's select 'linearSVM'. We set it as follows: `LinearSVC(C=1)`.

#### Run your classifier
Open the `sklearn-audio-transfer-learning/src/` directory in a terminal and run `python sklearn_audioset.py`. Approximated run time in your laptop: 15 min.

Congrats, you have build a music genre classifier! The model we developed achieved 77.24% accuracy in our test set. Interestingly, this basic model can achieve better results than a standard [MFCCs + SVM classifier (53.44%)](https://arxiv.org/abs/1805.00237), and is quite competent when compared to the best result we are aware of [(82.1%)](https://www.mdpi.com/2076-3417/8/1/150).

#### Can you improve this result? 

Feel free to modify our scripts, they are meant for that! 
Keep us updated if you break the state-of-the-art ;)

## I want to build my own audio classifier

In the following, we give some tips on how to build another classifier that is not based on the GTZAN dataset.

As an example on how to do it, let's download the ASC-TUT dataset ([dev-set](https://zenodo.org/record/400515#.W9n2UtGdZhE) / [eval-set](https://zenodo.org/record/1040168#.W9n2jNGdZhE)). With this data, we will build an acoustic scene classifier. Are we in a park or in a bus?

Copy the audio into a new directory in `sklearn-audio-transfer-learning/data/audio/ASC-TUT/`. Now create your train/test partitions. These are just lists of the files that belong to this partition: see the GTZAN example above. For example, access to the your audio directory `sklearn-audio-transfer-learning/data/audio/ASC-TUT/dev-set/` and run `ls > train_set.txt`. Do the same for creating the test partition. Afterwords, remember to configure the variables `'audio_paths_train'` and `'audio_paths_test'` in `sklearn_audioset.py` with the new dataset/partitions.

The last step is to define which is the label (or ground truth) for each of the audios. You can define the correspondence between the audio path and its label in the following function `path2gt_datasets(path, dataset)` in `sklearn-audio-transfer-learning/src/utils.py`.

You are ready to go.

## Additional tips

Dive into it! This code is readable and well documented.
    
Change the model?
    'model_type': 'linearSVM', # 'linearSVM', 'SVM', 'perceptron', 'MLP', 'kNN'
    
Go faster?
    'load_training_data': 'training_data_GTZAN_8643.npz' # False or load a model: 'training_data_GTZAN_839.npz'
    
Does not fit into memory?
    'train_batch': 8,
    'test_batch': 8,

## Scripts directory
- `sklearn_audioset.py`: main script where we build the audio classifiers with Audioset features and Scikit-learn.
- `utils.py`: auxiliar script with util functions that are used by `sklearn_audioset.py`.
- `vggish_input.py`,`vggish_params.py`,`vggish_slim.py`,`mel_features.py`,`vggish_model.ckpt`: auxiliar scripts to employ the VGGish pre-trained model that delivers the Audioset features.

## Folders structure
- `/src`: folder containing previous scripts.
- `/data`: where all intermediate files (Audioset features, audio, results, etc.) will be stored. 
- `/data/index/`: indexed files containing ground truth annotations and partitions.
- `/data/audio/`: folder where to store the audio datasets.

When running previous scripts, the following folders will be created:
- `./data/audio_representation/`: where the training Audioset features are stored.
- `./data/experiments/`: where the results of the experiments are stored.
