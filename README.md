# Amazon-ML-Challenge-2021 #
Python Notebook which was used to solve the Amazon ML Challenge 2021. The problem was essentially product classification. Link to the challenge can be found below:
 [Link of the Challenge :](https://www.hackerearth.com/challenges/competitive/amazon-ml-challenge/)
 We were able to achieve an accuracy of 64.89. The accuracy achieved by the top team was 70.41.
 # Toolboxes/Libraries Used #
 - Numpy
 - Pandas
 - NTKL
 - FastText
 # Approach #
As we thought product classification would essentially be a keyword centric task, as a first step, Linear SVMs were tried out as a baseline.
 ### Data Cleaning ###
The data cleaning process involved:
 - Dealing with NaNs: All rows with more than 1 attribute as NaN was removed.
 - All characters were made to lower case.
 - HTML tags, punctuation and stop words were removed.
 - The WordNetLemmatizer from the NTLK package was used to lemmatize all text.
 - Concatenation: The Input to the models were formed by concatenating across all columns(attributes) for each row(datapoint).
 ### Model ###
The TF-IDF matrix was generated and then fed to the Linear SVM. 
The model's performance was less than satisfactory. We believe this is because the vocabulary of the dataset was much larger than that of wordnet. Taking the example of label=60054, the WordNetLemmatizer was not able to lemmatize 'hinduism' to 'hindu', and the embedding used dosen't convey the similarity between such words and words like 'Hanuman' or 'Chalisa'.
We also found some label bias which could've critically affected the performance of the SVM.
We thought using a model which used character level representations might be able to mitigate this problem, thus we tried using FastText next. Running FastText with the default parameters alone gave us an accuracy of ~63%. Tweaking parameter such as minn failed to increase accuracy.
### Resampling ###
As the data was highly imbalanced, we tried performing a combination of random undersampling and random oversampling on the data. However, this reduced accuracy.
### Ensemble ###
Finally, an Ensemble scheme was used to increase the performance of the model. The data was divided into two sets, one containing the Majority Labels, and another containing the Minority Labels with some overlap. These two different sets was used to train 2 different FastText modesls, whoose predicitions were combined with that of the original model(trained on the entire dataset) usung an Ensemble scheme utilizing the predict probabilities provided by FastText models. This increased our accuracy to 64.89%
