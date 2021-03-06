Assignment URL: http://www.cs.columbia.edu/~cs4705/hw1/h1_fall14.pdf

#PART 1: HOW TO RUN CODE

Question 4:
Given code to produce counts from training data
python count_freqs.py ner_train.dat > ner.counts
--> Outputs into ner.counts

to replace words with rare and to produce new training data with rare
python add_rare.py ner.counts ner_train.dat
--> Produces file ner_train_rare.dat

to produce counts after replacing data info with rare
python count_freqs.py ner_train_rare.dat > ner_rare.counts
--> Outputs into ner_rare.counts

to produce emission counts 
python named_entity_tagger.py ner_rare.counts ner_dev.dat ner.counts
--> Produces file ner_dev_emissions.dat

to check performance of model 
python eval_ne_tagger.py ner_dev.key ner_dev_emissions.dat


Question 5:
to prduce trigram probabilities - given sample_trigram data
python named_entity_tagger.py ner_rare.counts ner_dev.dat ner.counts sample_trigram.dat
--> produces file ner_dev_trigram.dat

implements viterbi algorith
python viterbi_algo.py ner_rare.counts ner_dev.dat ner_dev_viterbi.dat 
--> outputs probability of tagged sequence of word in ner_dev_viterbi.dat

evaluates performance
python eval_ne_tagger.py ner_dev.key ner_dev_viterbi.dat


Question 6: 
to replace words new set of classes - not just RARE
python add_class.py ner.counts ner_train.dat
--> Produces file ner_train_classes.dat

to prodce counts after replacing data info with new classes
python count_freqs.py ner_train_classes.dat > ner_classes.counts
--> output int ner_classes.counts

to produce updated emission counts
python named_entity_tagger.py ner_classes.counts ner_dev.dat ner.counts
--> produces file ner_dev_emissions.dat (overwrites previous data)

evaluates performance
python eval_ne_tagger.py ner_dev.key ner_dev_emissions.dat

to implement viterbi algorihm after replacing words in the class
python viterbi_classes.py ner_classes.counts ner_dev.dat ner_dev_viterbi_classes.dat 
--> outputs probability into ner_dev_viterbi_classes.dat

evaluates performance
python eval_ne_tagger.py ner_dev.key ner_dev_viterbi_classes.dat


#PART 2: PERFORMANCE OF ALGORITHM
4. Emission count performance
Found 14043 NEs. Expected 5931 NEs; Correct: 3117.

	 precision 	recall 		F1-Score
Total:	 0.221961	0.525544	0.312106
PER:	 0.435451	0.231230	0.302061
ORG:	 0.475936	0.399103	0.434146
LOC:	 0.147750	0.870229	0.252612
MISC:	 0.491689	0.610206	0.544574

5. Viterbi Algorithm Performance
Found 4704 NEs. Expected 5931 NEs; Correct: 3647.

	 precision 	recall 		F1-Score
Total:	 0.775298	0.614905	0.685849
PER:	 0.762535	0.595756	0.668907
ORG:	 0.611855	0.478326	0.536913
LOC:	 0.876458	0.696292	0.776056
MISC:	 0.830065	0.689468	0.753262


6. HMM Improvement Performance

Found 11677 NEs. Expected 5931 NEs; Correct: 3867.

	 precision 	recall 		F1-Score
Total:	 0.331164	0.651998	0.439232
PER:	 0.192650	0.824266	0.312307
ORG:	 0.475936	0.399103	0.434146
LOC:	 0.811370	0.684842	0.742756
MISC:	 0.491689	0.610206	0.544574

Found 4920 NEs. Expected 5931 NEs; Correct: 3862.

	 precision 	recall 		F1-Score
Total:	 0.784959	0.651155	0.711824
PER:	 0.789084	0.637106	0.704997
ORG:	 0.639545	0.546338	0.589279
LOC:	 0.874346	0.728462	0.794765
MISC:	 0.815686	0.677524	0.740214




#PART 3: OBSERVATION/COMMENTS
4. Emission Count Performance
The total precision for this section was 0.221961 - which is a pretty low precision
This is most likely due to the fact that a lot of words were categorized under _RARE_ (a total of 30260 words)
There were a lot more named entities found than expected.


5. Viterbi Algorithm Performance
Using the Viterbi algorithm dramatically increased my total precision to be 0.775298
There were a lot fewer named entities found - with a higher number of those being correct.
This is most likely due to the fact that we are considering the maximum likelihood estimates for transmissions as well as the emissions estimates

6. HMM Improvement Performance
The additional classes I created for this section were:
_UPPER_: for words that are in all uppercase
_DIGIT_: words that only contain digits
_NOTALPHA_: contains words that contain punctuation in them

These were chosen after looking at the set of infrequent words. 
The emssion precision for this increased from 0.22 to 0.33164
There were fewer number of named entities found with a higher number of those being correct - which shows that this increased performance
However, the precision for the viterbi algorithm increased - but only from 0.7752 to 0.7849
This increased the number of found named intities from 4704 to 4920 and also increased the number off correct tags from 3627 to 3862

The performance overall increased - this is most likely due to the fact that less words were categorized under _RARE_ to make a more precise
estimatation. The number words categorized under RARE is now: 21640 
Categorization results:
_RARE_: 21640 words
_UPPER_: 2508
_DIGIT_: 906
_NOTALPHA_: 5206

In initially chose classes that contained very few words overall - which did not increase performance as much as these classes.


PART 4: ADDITIONAL INFORMATION
Edited given count_freqs.py - adjusted read_counts
