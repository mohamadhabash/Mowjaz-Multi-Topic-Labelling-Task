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

## Data Visualization
### Distribution of training dataset
The following figure shows the number of articles in each category. It also shows that the training dataset distribution is almost perfectly balanced. No actions are needed to make the dataset more balanced. In other words, we do not have to apply an under-sampling or over-sampling method.

![image](https://user-images.githubusercontent.com/53236311/161681394-b54e6e05-1119-453d-973a-5637bdb7a6c5.png)


### WordCloud
The following figure is a word cloud of all articles in training dataset.

![image](https://user-images.githubusercontent.com/53236311/161682245-9bda3db6-cd33-459b-ab99-88ee55c2f57d.png)

# Test Preprocessing
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
