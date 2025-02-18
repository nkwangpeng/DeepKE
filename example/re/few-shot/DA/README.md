# Data Augmentation for Few-shot RE
## 1 Prepare the environment

Following the instructions from [nlpaug](https://github.com/makcedward/nlpaug#installation) (Thanks a lot!).



## 2 Try different DA methods
We provide many data augmentation methods: DA based word2vec, TF-IDF, contextual word embedding (BERT and RoBERTa), random swapping and WordNet' Synonym). And all DA methods can be implimented on contexts, entites and both of them. 
- Generate augmented data
    
  ```shell
  >> python DA.py -h
  usage: DA.py [-h] --input_file INPUT_FILE --output_dir OUTPUT_DIR --DAmethod
             {word2vec,TF-IDF,word_embedding_bert,word_embedding_roberta,random_swap,synonym} --model_dir MODEL_DIR
             [--model_name MODEL_NAME] [--locations {sent1,sent2,sent3,ent1,ent2} [{sent1,sent2,sent3,ent1,ent2} ...]]

    optional arguments:
    -h, --help            show this help message and exit
    --input_file INPUT_FILE, -i INPUT_FILE
                            the training set file
    --output_dir OUTPUT_DIR, -o OUTPUT_DIR
                            The directory of the sampled files.
    --DAmethod {word2vec,TF-IDF,word_embedding_bert,word_embedding_roberta,random_swap,synonym}, -d {word2vec,TF-IDF,word_embedding_bert,word_embedding_roberta,random_swap,synonym}
                            Data augmentation method
    --model_dir MODEL_DIR, -m MODEL_DIR
                            the path of pretrained models used in DA methods
    --model_name MODEL_NAME, -mn MODEL_NAME
                            model from huggingface
    --locations {sent1,sent2,sent3,ent1,ent2} [{sent1,sent2,sent3,ent1,ent2} ...], -l {sent1,sent2,sent3,ent1,ent2} [{sent1,sent2,sent3,ent1,ent2} ...]
                            List of positions that you want to manipulate
  ```

  Take context-level DA based on contextual word embedding at the context-level on SemEval for example:

  ```bash
  python DA.py \
      -i train.txt \
      -o aug \
      -d word_embedding_bert \
      -mn bert-base-uncased \
      -l sent1 sent2 sent3
  ```

- Delete repeated instances and get the final augmented data

  ```shell
  >> python merge_dataset.py -h
  usage: merge_dataset.py [-h] [--input_files INPUT_FILES [INPUT_FILES ...]] [--output_file OUTPUT_FILE]

  optional arguments:
    -h, --help            show this help message and exit
    --input_files INPUT_FILES [INPUT_FILES ...], -i INPUT_FILES [INPUT_FILES ...]
                          List of input files containing datasets to merge
    --output_file OUTPUT_FILE, -o OUTPUT_FILE
                          Output file containing merged dataset
  ```

  For example:

  ```bash
  python merge_dataset.py \
      -i train.txt aug/aug.txt \
      -o aug/merge.txt
