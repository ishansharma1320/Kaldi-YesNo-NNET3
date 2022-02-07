Training A NNET-3 Model using Kaldi on YesNo Dataset
================

In Order to set this up, clone this repository in the egs directory of your locally installed kaldi repository
For successful training of a NNET-3 model, following pre-requisites must be met:
  
  1. GMM model with triphone alignments
  2. A Config file that defines the architecture of Neural Network
  


Creating the `input_task` dir
------------------------------------

In order to run `easy-kaldi`, you need to make a new `input_dir` directory. This is the only place you need to make changes for your own corpus.

This directory contains information about the location of your data, lexicon, language model.

Here is an example of the structure of my `input_dir` directory for the corpus called `corpus`. As you can see from the `->` arrows, all of these files are softlinks. Using softlinks helps you keep your code and data separate, which becomes important if you're using cloud computing.

```
input_corpus/
├── lexicon_nosil.txt -> /data/corpus/lexicon/lexicon_nosil.txt (present in the input directory for yesno)
├── lexicon.txt -> /data/corpus/lexicon/lexicon.txt (present in the input directory for yesno)
├── task.arpabo -> /data/corpus/lm/corpus.arpabo (present in the input directory for yesno)
├── test_audio_path -> /data/corpus/audio/test_audio_path (this should correspond to the directory which contain the test audio files only)
├── train_audio_path -> /data/corpus/audio/train_audio_path (this should correspond to the directory which contain the test audio files only)
├── transcripts.test -> /data/corpus/audio/transcripts.test (present in the data/test_yesno directory, file named as text)
└── transcripts.train -> /data/corpus/audio/transcripts.train (present in the data/test_yesno directory, file named as train)

0 directories, 7 files
```

Most of these files are standard Kaldi format, and more detailed descriptions of them can be found on [the official docs](http://kaldi-asr.org/doc/data_prep.html).


- `lexicon_nosil.txt` // Standard Kaldi // phonetic dictionary without silence phonemes
- `lexicon.txt` // Standard Kaldi // phonetic dictionary with silence phonemes
- `task.arpabo` // Standard Kaldi // language model in ARPA back-off format
- `test_audio_path` // Custom file! // one-line text file containing absolute path to dir of audio files (eg. WAV) for testing
- `train_audio_path` // Custom file! // one-line text file containing absolute path to dir of audio files (eg. WAV) for training
- `transcripts.test` // Custom file! // A typical Kaldi transcript file, but with only the test utterances
- `transcripts.train` // Custom file! // A typical Kaldi transcript file, but with only the train utterances




Running the scripts
------------------------------------



The scripts will name files and directories dynamically. You will define the name of your input data (e.g. `corpus`) in the initial `input_` dir, and then the rest of the generated dirs and files will be named accordingly. For instance, if you have `input_corpus`, then the GMM alignment stage will create `data_corpus`, `plp_corpus` and `exp_corpus`.




### Force Align Training Data (GMM)

`$ ./run_gmm.sh corpus 001`

- `corpus` should correspond exactly to `input_corpus`.

- `001` is any character string, and is written to the name of the WER file: `WER_nnet3_corpus_001.txt`




### Neural Net Acoustic Modeling (DNN)

`$ ./run_nnet3.sh "corpus" $hidden-dim $num-epochs`

  For yesno, set hidden-dim=39 num-epochs=10 for accurate results
  
- first argument is a character string of the corpus name (must correspond to `input_corpus`)

- `hidden-dim` is the number of nodes in your hidden layer

- `num-epochs` is num epochs for DNN training
