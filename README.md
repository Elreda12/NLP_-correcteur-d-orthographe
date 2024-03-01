# NLP_-correcteur-d-orthographe
You use autocorrect every day on your cell phone and computer. In this assignment, you will explore what really goes on behind the scenes. Of course, the model you are about to implement is not identical to the one used in your phone, but it is still quite good.

By completing this assignment you will learn how to:

Get a word count given a corpus
Get a word probability in the corpus
Manipulate strings
Filter strings
Implement Minimum edit distance to compare strings and to help find the optimal path for the edits.
Understand how dynamic programming works
Similar systems are used everywhere.

For example, if you type in the word "I am lerningg", chances are very high that you meant to write "learning", as shown in Figure 1.
<img width="263" alt="auto-correct" src="https://github.com/Elreda12/NLP_-correcteur-d-orthographe/assets/141457934/13326b4c-6b28-401a-ae9f-711b3f91697d">
# Part 1: Data Preprocessing

As in any other machine learning task, the first thing you have to do is process your data set.

Many courses load in pre-processed data for you.
However, in the real world, when you build these NLP systems, you load the datasets and process them.
So let's get some real world practice in pre-processing the data!
Your first task is to read in a file called 'shakespeare.txt' which is found in your file directory. To look at this file you can go to File ==> Open.

# Implement the function process_data which

1) Reads in a corpus (text file)

2) Changes everything to lowercase

3) Returns a list of words.

Expected Output : 
The first ten words in the text are: 
['o', 'for', 'a', 'muse', 'of', 'fire', 'that', 'would', 'ascend', 'the']
There are 6116 unique words in the vocabulary.

# Implement a get_count function that returns a dictionary

-- The dictionary's keys are words
-- The value for each word is the number of times that word appears in the corpus.

For example, given the following sentence: "I am happy because I am learning", your dictionary should return the following:
![ccc](https://github.com/Elreda12/NLP_-correcteur-d-orthographe/assets/141457934/3d1c579e-e3ce-4c04-8824-822090cd771e)


Instructions: Implement a get_count which returns a dictionary where the key is a word and the value is the number of times the word appears in the list.

# Implement get_probs

Given the dictionary of word counts, compute the probability that each word will appear if randomly selected from the corpus of words.

𝑃(𝑤𝑖)=𝐶(𝑤𝑖)𝑀(Eqn-2)
where

𝐶(𝑤𝑖)
  is the total number of times  𝑤𝑖
  appears in the corpus.

𝑀
  is the total number of words in the corpus.

For example, the probability of the word 'am' in the sentence 'I am happy because I am learning' is:

𝑃(𝑎𝑚)=𝐶(𝑤𝑖)𝑀=27.(Eqn-3)
Instructions: Implement get_probs function which gives you the probability that a word occurs in a sample. This returns a dictionary where the keys are words, and the value for each word is its probability in the corpus of words.

# Part 2: String Manipulations
Now, that you have computed  𝑃(𝑤𝑖)
  for all the words in the corpus, you will write a few functions to manipulate strings so that you can edit the erroneous strings and return the right spellings of the words. In this section, you will implement four functions:

delete_letter: given a word, it returns all the possible strings that have one character removed.
switch_letter: given a word, it returns all the possible strings that have two adjacent letters switched.
replace_letter: given a word, it returns all the possible strings that have one character replaced by another different letter.
insert_letter: given a word, it returns all the possible strings that have an additional character inserted.

# Part 3: Combining the edits
Now that you have implemented the string manipulations, you will create two functions that, given a string, will return all the possible single and double edits on that string. These will be edit_one_letter() and edit_two_letters().
# 3.1 Edit one letter
Instructions: Implement the edit_one_letter function to get all the possible edits that are one edit away from a word. The edits consist of the replace, insert, delete, and optionally the switch operation. You should use the previous functions you have already implemented to complete this function. The 'switch' function is a less common edit function, so its use will be selected by an "allow_switches" input argument.

Note that those functions return lists while this function should return a python set. Utilizing a set eliminates any duplicate entries.

# 3.2 Edit two letters
Now you can generalize this to implement to get two edits on a word. To do so, you would have to get all the possible edits on a single word and then for each modified word, you would have to modify it again.

Instructions: Implement the edit_two_letters function that returns a set of words that are two edits away. Note that creating additional edits based on the edit_one_letter function may 'restore' some one_edits to zero or one edits. That is allowed here. This accounted for in get_corrections.

# 3-3: suggest spelling suggestions
Now you will use your edit_two_letters function to get a set of all the possible 2 edits on your word. You will then use those strings to get the most probable word you meant to type aka your typing suggestion.

Instructions: Implement get_corrections, which returns a list of zero to n possible suggestion tuples of the form (word, probability_of_word).

Step 1: Generate suggestions for a supplied word: You'll use the edit functions you have developed. The 'suggestion algorithm' should follow this logic:

If the word is in the vocabulary, suggest the word.
Otherwise, if there are suggestions from edit_one_letter that are in the vocabulary, use those.
Otherwise, if there are suggestions from edit_two_letters that are in the vocabulary, use those.
Otherwise, suggest the input word.*
The idea is that words generated from fewer edits are more likely than words with more edits.
Note:

Edits of one or two letters may 'restore' strings to either zero or one edit. This algorithm accounts for this by preferentially selecting lower distance edits first.

The logical or could be used to implement the suggestion algorithm very compactly. Alternately, if/then constructs could be used.

Step 2: Create a 'best_words' dictionary where the 'key' is a suggestion and the 'value' is the probability of that word in your vocabulary. If the word is not in the vocabulary, assign it a probability of 0.

Step 3: Select the n best suggestions. There may be fewer than n.

# Part 4: Minimum Edit distance

Now that you have implemented your auto-correct, how do you evaluate the similarity between two strings? For example: 'waht' and 'what'

Also how do you efficiently find the shortest path to go from the word, 'waht' to the word 'what'?

You will implement a dynamic programming system that will tell you the minimum number of edits required to convert a string into another string.

# Part 4.1 Dynamic Programming
Dynamic Programming breaks a problem down into subproblems which can be combined to form the final solution. Here, given a string source[0..i] and a string target[0..j], we will compute all the combinations of substrings[i, j] and calculate their edit distance. To do this efficiently, we will use a table to maintain the previously computed substrings and use those to calculate larger substrings.

