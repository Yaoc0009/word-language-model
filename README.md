# NLP Assignment 2 Q1

To train the FNN model for 6 epochs run the first command\
To train the FNN 8 gram model run the second command\
To train the FNN model without weight sharing specify tie_weights=false in the FNNmodel constructor\
To generate text with the saved model use the last command

```bash 
python main.py --cuda --lr 2 --epochs 6 --model FNN
python main.py --cuda --lr 2 --epochs 6 --model FNN --bptt 8
python generate.py --cuda --checkpoint model.pt
```

# Word-level language modeling RNN

This example trains a multi-layer RNN (Elman, GRU, or LSTM) on a language modeling task.
By default, the training script uses the Wikitext-2 dataset, provided.
The trained model can then be used by the generate script to generate new text.

```bash 
python main.py --cuda --epochs 6           # Train a LSTM on Wikitext-2 with CUDA
python main.py --cuda --epochs 6 --tied    # Train a tied LSTM on Wikitext-2 with CUDA
python main.py --cuda --epochs 6 --model Transformer --lr 5   
                                           # Train a Transformer model on Wikitext-2 with CUDA
python main.py --cuda --tied               # Train a tied LSTM on Wikitext-2 with CUDA for 40 epochs
python generate.py                         # Generate samples from the trained LSTM model.
python generate.py --cuda --model Transformer
                                           # Generate samples from the trained Transformer model.
```

The model uses the `nn.RNN` module (and its sister modules `nn.GRU` and `nn.LSTM`)
which will automatically use the cuDNN backend if run on CUDA with cuDNN installed.

During training, if a keyboard interrupt (Ctrl-C) is received,
training is stopped and the current model is evaluated against the test dataset.

The `main.py` script accepts the following arguments:

```bash
optional arguments:
  -h, --help            show this help message and exit
  --data DATA           location of the data corpus
  --model MODEL         type of recurrent net (RNN_TANH, RNN_RELU, LSTM, GRU,
                        Transformer)
  --emsize EMSIZE       size of word embeddings
  --nhid NHID           number of hidden units per layer
  --nlayers NLAYERS     number of layers
  --lr LR               initial learning rate
  --clip CLIP           gradient clipping
  --epochs EPOCHS       upper epoch limit
  --batch_size N        batch size
  --bptt BPTT           sequence length
  --dropout DROPOUT     dropout applied to layers (0 = no dropout)
  --tied                tie the word embedding and softmax weights
  --seed SEED           random seed
  --cuda                use CUDA
  --log-interval N      report interval
  --save SAVE           path to save the final model
  --onnx-export ONNX_EXPORT
                        path to export the final model in onnx format
  --nhead NHEAD         the number of heads in the encoder/decoder of the
                        transformer model
```

With these arguments, a variety of models can be tested.
As an example, the following arguments produce slower but better models:

```bash
python main.py --cuda --emsize 650 --nhid 650 --dropout 0.5 --epochs 40           
python main.py --cuda --emsize 650 --nhid 650 --dropout 0.5 --epochs 40 --tied    
python main.py --cuda --emsize 1500 --nhid 1500 --dropout 0.65 --epochs 40        
python main.py --cuda --emsize 1500 --nhid 1500 --dropout 0.65 --epochs 40 --tied 
