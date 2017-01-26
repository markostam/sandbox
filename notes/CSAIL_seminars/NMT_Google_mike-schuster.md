# Neural Machine Translation and other AI Projects at Google

+ mike schuster - google brain
+ schuster@google.com
+ g.co/brain

## Quick History

### various people tried to improve translation with NN's
+ brain team
+ translate team

### seq to seq models (NIPS 2014)
+ SOTA on WMT en>FR 
+ learned without explicit alignment
+ drawback: all information needs to be carried in internal state
  + breaks fown for long sentences

### attention based models (2014)
+ removes drawback by giving accesss to all encoder states
+ tranlsation quality now independent of sentences length

![](https://github.com/markostam/sandbox/blob/master/photos/IMG_3368.jpg?raw=true)

### Timeline of Google Brain work on NMT
#### 9/2015
+ tried to replicate results of these papers, couldn't do it

#### 1/2016
+ first SOTA results on WMT database

#### 2/2016
+ first production data results
+ more people from brain and translate got involved to productionize NMT

#### 9/2016
+ first language chinese->en launched
+ paper with full description GNMT on arxic

#### 11/15/2016
+ 16 language paries launched on translate
+ multilingual GNMT on arXiv

## Production Quality

### Production traffic
+ billions of sentences per day (and growing)
+ biggest user of TPUs at google

### NMT give significant improvements
+ human side-by-side (SxS) improvement of +>0.1 is significant
  + +>0.1 => definitely launchable
  + +>0.1 => improvement would take months to achieve before
+ first language pair launched
  + chinese -> english got SxS for +>0.6
  + improvement as big as all improvements in the last 10 years
+ now almost all languagees +>0.5 and some up to +>1.0

## What do you do with sequence models (RNNs)

### speech recognition 
+ estimate state posterior probabbilities per 10ms frame

### video recommendations
+ with hierarchicahl softmax and MaxEnt model for top 500k youtube videos
  + based on history:
    + battlestar galactica,battlestar galactica, nail vid, nail vid nail vid -> ?
      + battlesstar galactica

### image captioning
+ combine with CNN as shown by andrej karpathy
+ they tested this internally at google and learned that the system was limited: it just started spitting the same stuff eventually.

## deep sequence to sequence (old model)
+ 4 layer deep LSTM encoder/decoder
+ just gets deeper and deeper and uses LSTM instead of vanilla RNN

### attention mechanism
+ addresses the information bottleneck problem
+ all encoder states accessible instead of only final one

## Google BNMT Architecture
+ 8-layer encoder/decoder with atention and residual skip-connections

![](https://github.com/markostam/sandbox/blob/master/photos/IMG_3369.jpg?raw=true)

+ each layer is on one gpu (8x GPUSs/ machine).
  + they chose 8 layers based on the number of GPU's per machine
  + encoder and decoder are on separate machines

### model training
  + runs on ~100 GPUS (12 replicas (for ensembling) 8 gpus each)
  + only 32k words
  + because softmax size only 32k can be fully calculated ( no sampling))
+ optimization
  + combination of Adam an dSGD wiht delayed exponential decay
  + 128/256 sentence pairs combined into one batch
+ training time
  + ~1 week for 2.5m steps = ~300m sentence pairs
  + for example, on english->french we use only 15% of available data
  + they have more than enough data. this is infinite data to them.

### Wordpiece Model (WPM)
+ Data-driven bottom-up segmenter (trained once on example data)
  + produces predetermined number of units to represent any word possible
  + no UNK (unkonwn word) anymore
  + frequenc words become full units, rare words split up
### Training criterion
+ max liklihood on training data given evolving word definition
+ simplest case very similar to byte pair encodeing (BPE)
### WPM example
+ segmentation
  + add underscore before words then segment using trained wpm model
    + this is a house => _Th is_is a house
+ *TODO: look up WPM example
+ particularly useful for morphologically rich languages (Ru, De, Ja, Ko, etc)
+ they now use WPM model for 32k words
+ use of WPM improves machine translation measure (BLEU) and lowers latency

## Speed-up with TPU's

### decoding speed used to be a major launch blocker
+ TPU is the only viable option (~300M model parameters total)
+ quantization error
+ some bugs (quantization errors and a lot more)
+ many restrictions (memory, etc

### speedup (TPU, algorithms, etc)
+ 10sec/sentence -> 200ms/sentence

### Quantization range boundaries simulated during training
+ model can be quantized without loss of translation quality
+ no other training restrictions necessary

### Quantized inference using TPU
+ matmul is in 8 bits all other in 16 bits to work
+ attention is on CPU
+ Quantization actually helps quality (maybe regularization)

### Latency
+ latency increased by roughly a factor of 3x
+ still was ok - people dont care as long as its within 300ms

## Multilingual model and Zero-Shot Translation
+ they start to get better results as they add more languages to the model
+ due to transfer learning
+ model trained on both can be fed a sentence mixture of two languages and have it translated to a third language

### Interlingua?
+ sentences with same meaning mapped to similar regions regardless of language!

## Challenges

### early cutoff
+ cuts off or drops some words in source sentence
#### broken dates/numbers
+ 5->6
+ but on average significantly better than pbmt on  numbers
### short rare queries
+ "deoxyribonucleic acid" in japaneses?
+ should be easy but isn't
### transliteration of names
+ eichelbergertown => ?
### JUNK
+ xxx -> (oxford dictionary)
+ the cat is a good computer -> (of the English language?)
+ BAD DATA!

## Open research problems

### Use of context
+ full doc translation
+ infinite streaming translation
+ contexts (10yo translation different than 30yo)

### Better automatic measure and objective functions
+ current BLEU weighss all words the same
  + "president" more important than "the"
+ discriminative training
  + maximum liklihood produces mismatched training/test procedure
    + no decoding errors for max liklihood training
+ RL (and similar) already running but no significant gains at moment

### BNMT for other projects
+ other projects using same codebase for diifferent problems (search, google assistant, etc etc)
+ at least 10k gpu's running BNMT every day

