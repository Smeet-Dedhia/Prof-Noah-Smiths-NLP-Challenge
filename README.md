# My Submission for Prof. Noah Smith's NLP Challenge
## The Challenge
### Part1 - Is it English?: 
Given a pair of sentences—one original and one corrupted—build a classifier to identify which sentence is the original. The training data provides labeled pairs, while the test data contains pairs in random order. Your goal is to classify the test data with maximum accuracy.

### Part 2 - Ruin English: 
Create a dataset where each original sentence from the training data is paired with a corruption you generate. The corruptions should be challenging for classifiers like the one built in the first task, ensuring no sentence remains identical to its original.

## Summary of my Approach  

### Data Preparation & Embedding  
- Used **BERT-base-uncased** model to transform `sentence_a` and `sentence_b` into embeddings.  
- Computed the difference in embeddings (`embedding_a - embedding_b`) as input for Part 1.  
- Applied the same model to generate embeddings for test data in Part 1 and for corrupting sentences in Part 2.  

---

### Part 1: Classification  
- **Training Data**:  
  - Sampled 50K rows from training data, assigned them a label of `0`.  
  - Swapped `sentence_a` and `sentence_b` in these rows and labeled them as `1`.  
  - Resulting dataset: **100K instances**.  
- **Approach**:  
  - Tried two inputs:  
    1. Concatenation of `embedding_a`, `embedding_b`, and their difference.  
    2. Signed difference (`embedding_a - embedding_b`) only.  
  - Approach 2 performed better, leveraging the subtle differences in embeddings to detect corruption.  
- **Model**:  
  - Implemented a **Bidirectional LSTM** in PyTorch.  
  - Order of corrupted sentences was preserved by using signed differences (no absolute values).  
- **Performance**:  
  - Validation Accuracy: **96.4%**  
  - Precision: **96.7%**, Recall: **96%**, F1 Score: **0.96**  

---

### Part 2: Sentence Corruption  
- Planned to train a **seq2seq encoder-decoder** model for generating corrupted sentences.  
- Current Solution:  
  - Implemented a Python script for corrupting sentences by:  
    - Randomly shifting punctuation.  
    - Swapping words with a lower probability.  


