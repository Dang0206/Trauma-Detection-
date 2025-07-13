TRAUMA DETECTION METHODS

To evaluate the complexity of the dataset, several baseline and Vietnamese-optimized models were tested. The selected approaches include Logistic Regression and Gemini-2.0-Flash (a large language model, LLM).

 Logistic Regression 
 
For Logistic Regression, effective Vietnamese text processing was achieved using the PyVi library (specifically, ViTokenizer) for word segmentation. Word segmentation is essential in Vietnamese due to its compound word structure. Without accurate segmentation, the model may fail to interpret the correct meaning of the text. Proper segmentation helps the model better capture semantic information, especially when applying n-gram features.
After segmentation, the textual data was converted into a numerical format to be fed into the machine learning model. The Bag-of-Words approach was applied using CountVectorizer from the scikit-learn library:
-	The vectorizer was configured with ngram_range=(1, 2) to include both unigrams (single words) and bigrams (two consecutive words).
-	CountVectorizer was fit on the training set to learn the vocabulary and then applied to both the training and test sets to ensure consistency.
After vectorization, the Logistic Regression model is train in a 10-fold cross-validation framework with random_state=42 to ensure reproducibility. The model was then trained on the vectorized training data.
 Gemini-2.0-Flash

During experimentation, Gemini-2.0-Flash was accessed via the API provided by Google, connected through the google.generativeai library using the free-tier access.A custom prompt was designed to guide the model, containing detailed label descriptions, contextual information, and specific instructions on expected outputs. 
The prompt also included a generation_config to control model behavior:
-	temperature = 1.0: This parameter controls the degree of randomness in the model’s responses. Higher values (close to 1) make the output more creative and diverse, while lower values (close to 0) produce more deterministic and repetitive responses. A value of 1.0 was chosen to provide the model with sufficient flexibility to handle varied and ambiguous textual contexts.
-	top_p = 0.2: This nucleus sampling parameter ensures that the model selects the next word from the top 20% of the probability distribution. This restricts the model from choosing low-probability words, resulting in more coherent and focused outputs.

Additionally, the model was fine-tuned using a few-shot prompting strategy. A representative example with an accurately labeled paragraph—formatted to reflect the real dataset—was embedded in the prompt. This few-shot approach improves accuracy when working with Vietnamese texts, which may not be a high-priority language in Gemini’s original training data.
