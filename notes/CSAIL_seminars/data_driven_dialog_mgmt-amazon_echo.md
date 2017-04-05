# Dialog Action and Response at Amazon

+ Alborz Geramifard
+ alborzg@amazon.com
+ With Amazon since 2013, lot of work on RL for amazon, Aeronautics and Astronautics PhD MIT, Postdoc MIT Information and Systems Multimodal MDP

## Existing gaps
+ manual dialog: hand written rules
  + time consuming
  + hard to debug
  + non adaptive
+ goals 
  + adaptive

## Markov Decision Process

+ (S,A,Pass,Rass,gamma)
+ pi(s): S -> A
  + st+1, rt
+ Markovian assumption: "don't need ot know how we got here, sufficient to know we are here"
+ Example dimensinos:
  + NLU Output: Intent, Slot, Confidence, etc
  + ASR Output: Transcription, Lattie, etc
  + confirmation history
+ Possible actions A
  + confirm
  + play something
  + say idk
+ Pass
  + transition probability: not known!
  + distribution oer the next state (s') given the current state and action (s,a)
+ Dependent on the user ASR
+ Rass
  + reward function: user satisfaction
  + evaluated manually or automatically (ideal) manually = amazon turk looking at stuff
  + examples of automatic: 
    + sentiment analysis: good job! or Alexa u suck
    + voice cues: turn of the lamp vs. tuuurn oooof the laaaamp
+ gamma:
  + discount factor: future reward vs. immediate reward

## Reinformcement Learning

+ reinforcement learning is a way to solve markov decision process without a model.
+ [pigeon pushes the block to under the food - BF Skinner Foundation](https://www.youtube.com/watch?v=ymkT_C_NWXw) 
+ "feed the echo infinite data and and have it figure out how to best interact with users"
![the single RL slide lol](https://github.com/markostam/sandbox/blob/master/photos/IMG_3851.JPG?raw=true)

### DATA

+ you need data man

## MovieBot

+ conversational movie bot within alexa - "alexa enable movie bot"
+ pivoting on similar movies, movies by actor, movies by director
+ q&a about year, lead actor, director, rating , plot
+ personalized response avriability
+ anaphora including indent carry over "how tall is brad pitt?" -> how about matt daemon
+ global search: what is the rating for inception?
+ currently fully *rule-based*
![moviebot](https://github.com/markostam/sandbox/blob/master/photos/IMG_3852.JPG?raw=true)

### chicken and egg problem

+ need a system to create data
+ used this bot using the Alexa SDK to create moviebot
+ DSL for interaction
+ very important to have a high-quality system end to end or the user will not come back. thus you will have no data.
+ users don't care how it works, they want it to work.

## Potential Feedback Signals

+ sentiment: "alexa that was RAD!"
+ prosody: "ALEXA TURN OFF THE BED!!!!"
+ explicit: thumbs up thumbs down (no one does this)

### Sentiment analysis

+ used amazon produc reviews as proxy
+ target mapping (pos, neg, neutral)
+ 80% train, 20% de, 20% test
+ tried 13 ML methods (shpagett)
  + LSTM: simple lstm on word vectors on google word embeddings
  + sropout 0.4
  + l1: 0.001
  + 128 dim word vecs
+ looked at the two best ones to see if they were using the same data, to see if it's worth ensembling
  + looked at sentence distribution
  + looked at T-test between them LSTM vs. SVC
+ turns out T-test was similar between LSTM vs. SVC for positive intent and neutral
+ LSTM ends up doing better on negative intent. maybe because LSTM takes into account long-range connections i.e. blah blah blah BUT blah blah. LSTM can look at the BUT.

## Creating a Reward Function

+  skipped over this :(

## Questions:

+ try to penalize the system by one turn

+ one-shot vs. conversational.
  + users get biased towards the one-shot question apprach due to the current SOTA
  + "stocahstic based helping" tells you not everything has to be one-shot. i.e. you acn just say "who's the director" rather than "who is the director of the movie the martian"
