# Overview
This repo contains the code for this <a href="https://ieeexplore.ieee.org/document/9464614">paper</a> which describes my contribution to <a href="https://www.just.edu.jo/icics/icics2021/mowjaz/Task%20Description.html">ICICS 2021 Mowjaz Multi-Topic Labelling Task</a>. Mowjaz is an Arabic topical content aggregation mobile application for news, sport, entertainment and other topics from top publishers that users can follow. The paper describes the approach to classify articles using Bi-directional Gated Recurrent Unit (Bi-GRU) using <a href="https://keras.io/">Keras</a> with <a href="https://github.com/bakrianoo/aravec">AraVec</a> embeddings. The F1-score of this system is 0.8344 which shows a significant improvement over the baseline models, and resulted in ranking 6th place.

# Dataset
The dataset consists of 9,590 articles split into training,development and testing sets. The following table represents some statistical properties of this dataset.
<table>
<tbody>
<tr>
<td>&nbsp;</td>
<td><strong>&nbsp;Training</strong></td>
<td><strong>Development</strong></td>
<td><strong>Testing</strong></td>
</tr>
<tr>
<td><strong>Articles No.&nbsp;</strong></td>
<td>&nbsp;7,681</td>
<td>&nbsp;956</td>
<td>&nbsp;953</td>
</tr>
<tr>
<td>
<p><strong>Max/Min&nbsp;article length&nbsp;</strong></p>
</td>
<td>&nbsp;3,384/1</td>
<td>&nbsp;2,418/1</td>
<td>&nbsp;1,602/2</td>
</tr>
<tr>
<td><strong>Avg/StdDev article length</strong></td>
<td>&nbsp;228.6/218.2</td>
<td>&nbsp;226.2/227.5</td>
<td>&nbsp;235.1/216.0</td>
</tr>
<tr>
<td><strong>Number of unique words</strong></td>
<td>228,765</td>
<td>55,256</td>
<td>57,853</td>
</tr>
</tbody>
</table>
<!-- DivTable.com -->

## Test Preprocessing
Arabic and English punctuation (including parentheses,underscores, quotes, etc.) are removed from all articles in the training, development, and testing sets. As well as, html tags, web addresses, twitter usernames. For Arabic words, tashkeel and tatweel are stripped using pyarabic [3]. Words of length less than 3, non-Arabic words (English, Unicode, etc.) and numbers are also removed. Arabic stop-words are removed to minimize sequences’ lengths. The preprocessing effect is shown in the following table, which displays a sequence before and after the preprocess.

<table style="width: 356.031px;">
<tbody>
<tr>
<td style="width: 145px;">&nbsp;</td>
<td style="width: 210.031px;">&nbsp;Article</td>
</tr>
<tr>
<td style="width: 145px;">&nbsp;Before Preprocessing</td>
<td style="width: 210.031px;">
<p>عمان 4 كانون الأول (بترا) -&lt;p&gt;<br />تكون الأجواء اليوم باردة نسبياً<br />وغائمة جزئياً في أغلب المناطق&raquo;<br />ولطيفة الحرارة في الأغوار والبحر<br />وتكون الرياح شمالية غربية<br />السرعة<br /></p>
</td>
</tr>
<tr>
<td style="width: 145px;">After Preprocessing&nbsp;</td>
<td style="width: 210.031px;">
<p>عمان الاول بترا الاجواء بارده نسبيا<br />وغائمه جزئيا اغلب المناطق ولطيفه<br />الحراره الاغوار والبحر الميت وتكون<br />الرياح شماليه السرعة<br /></p>
</td>
</tr>
</tbody>
</table>
<!-- DivTable.com -->

## Data Visualization
### Distribution of training dataset
The following figure shows the number of articles in each category. It also shows that the training dataset distribution is almost perfectly balanced. No actions are needed to make the dataset more balanced. In other words, we do not have to apply an under-sampling or over-sampling method.

![image](https://user-images.githubusercontent.com/53236311/161681394-b54e6e05-1119-453d-973a-5637bdb7a6c5.png)


### WordCloud
The following figure is a word cloud of all articles in training dataset.

![image](https://user-images.githubusercontent.com/53236311/161682245-9bda3db6-cd33-459b-ab99-88ee55c2f57d.png)

# Deep Learning Model
The Sequential model takes the embedded sentence as an input, with shape (256, 300), followed by a Dropout layer with a drop rate of 0.2. The Dropout layer is a regularization technique which randomly sets input units to 0 at each step during training time, which helps prevent overfitting. Next, one Bi-directional Gated Recurrent Unit (Bi-GRU) of 300 cells for each of the forward layer and backward layer. Finally, a Dense layer (fully connected layer) of 300 neurons. At the end, a Dense layer of 10 neurons to represent each article category as an output. Total number of parameters of the model is 1,266,910.

![image](https://user-images.githubusercontent.com/53236311/161683875-4b7afe36-15c4-4cff-b58a-528e83c0e9cd.png)

Model is compiled using Adam optimizer with a learning rate of 0.0005, training batch size of 50, 8 epochs and 300 word embedding size as the hyperparameters. Adam optimizer with a learning rate of 0.0005 was proper and slow enough to converge. It was faster and more efficient than other optimizers like SGD, RMSprop, and it achieved better results.

<table>
<tbody>
<tr>
<td><strong>&nbsp;Optimizer</strong></td>
<td><strong>&nbsp;Learning rate</strong></td>
<td><strong>Batch size&nbsp;</strong></td>
<td><strong>&nbsp;Epochs</strong></td>
<td><strong>&nbsp;Embed. Size</strong></td>
</tr>
<tr>
<td>&nbsp;Adam</td>
<td>&nbsp;0.0005</td>
<td>&nbsp;50</td>
<td>&nbsp;8</td>
<td>&nbsp;300</td>
</tr>
</tbody>
</table>
<!-- DivTable.com -->

# Results 
To measure the accuracy of our model, F1 score is used. It is a good choice to test the model as it conveys the balance between precision and recall. F1 score results for validation set with a 0.3 classification threshold:
* F1 macro: 0.865
* F1 micro: 0.861
* F1 samples: 0.872

Model’s F1-score on the test set is 0.8344 which led to ranking 6th place.
