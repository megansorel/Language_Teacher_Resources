# Executive Summary

**Abstract**

When it comes to inspiring students to learn another language, teachers try to be creative but it can be hard to research and then build lessons using limited resources.

Not only by using relevant topics to what students want to learn but also finding nuances in communication techniques or following slang is a lot to ask of teachers.

It is an increasingly large problem to keep students, particularly in the US, motivated to learn foreign languages. Additionally, teachers are incredibly busy and don’t have time to research the patterns of communication and how it may change.

How can we support teachers and show students that language is ubiquitous and therefore worth learning?

**Solution**

Reddit is a popular website on a global scale, offering a majority of users, many of whom are under 30 years old, an opportunity to follow their curiosities and ask questions to people from different cultures. The vast sharing network allows conversation and language to flourish naturally. It is a great way to keep up with relevant topics and language use and can be a great resource for lesson material.

Teachers don't have a lot on their plate, they have time to browse submissions to find trends in language and topics though right?

Of course not!

That is why we are building a Reddit Web Scraper that will browse reddit's `Ask an American` and `Ask Europe` subreddit submissions to find trends in language use and topics. We can quickly scan over 2,000 submissions from selected timeframes to find patterns in conversations and build a report of suggestions for teachers lessons.

The report would include:

- recommendations on topics

- top used words for lessons

- unique words to a subreddit

- exploration of communication styles in target languages

Teachers may use the report to build lessons, prepare for study abroad trips, or help students understand communication differences across cultures.

**Method**

AskEurope was selected due to the comparable popularity to AskAnAmerican, as well as its inclusion of countries whose languages are among the most popular taught in American high school. AskAnAmerican was included for ESL or TEFL teachers.

After getting top_comments and questions from each subreddit, data was cleaned to maintain our goal of searching topics that were popular by culture as well as linguistic differences among Americans commenting in ‘AskAnAmerican’ and Europeans commenting in ‘AskEurope.’

Frequencies of parts of speech used by commenters, the subjectivity and polarity of commenters by culture, as well as the punctuation use by commenters were calculated. Against the original hypothesis, there was little difference among commenters. While this needs further analysis, it may be that reading and writing are more commonly the first part of a foreign language that learners master and they can be exposed to native use at the extent that they wish.

Most frequent words in each subreddit, determined using the spacy lemmatizer and TfidfVectorizer as well as the topics that were central to each subreddit were found. Topics on language and culture had a higher similarity to AskEurope subreddit topics than AskAnAmerican. Food and politics were more similar to AskAnAmerican than AskEurope. Further analysis will be needed to see how the US 2020 Election impacted this topic.

**Evidence**

When modeling, Logistic Regression, a Decision Tree, KNN, ExtraTrees, RandomForest, Bagging, Gradient Boost and AdaBoost Classifiers were used.

The baseline score was 50.7% with ‘AskAnAmerican’ in the majority class and labeled as 0.

After standard scaling, the Knn was barely above baseline in its predictions and was not further considered for the production model.

After grid searching for the best parameters,the Logistic Regression model accurately scored 73.8% on training data and 67.8% on testing data. The model performed 3% better at predicting the positive class (AskEurope) than the negative class(AskAnAmerican). The model’s predictions used mostly words to determine which subreddit a submission belonged to, for example, a subreddit that includes the word ‘amendment’ is 60% more likely to be from ‘AskAnAmerican’ whereas a model with the word ‘language’ is 30% more likely to be from ‘AskEurope.’

The Decision Tree Classifier originally was extremely overfit with over 20% variance between the test and training data, the latter getting 83% of the predictions.Introducing bias by limiting leaf size, max depth and minimum sample split reduced bias greatly. The features used differentiated from the logistic regression model. The most important features included the use of verbs and auxiliary verbs, the period in punctuation and the comment polarity.

The final model for production is Logistic Regression due to its easy interoperability and understandable coefficients. Furthermore, the variance between test and training data was the lowest without needing to use computer resources to grid search for the best cross val score with expenses classifiers.

The goal of the model is to quickly and accurately classify submissions and extract important features from the topics so teachers do not have to wait for lesson materials. Therefore, logistic regression is the used model as it could classify 67.8% of testing data.

**Conclusion**

In order to support teachers and language learners, it is advised that reddit apis are set up to scrape submissions and classify them on a weekly or monthly basis depending on where the model is being implemented.

Teachers can ask for specific topics or use summarized recommendations as they see fit. The current model can support them to use topics and vocab words to teach lessons illustrating cultural differences or prepare students for study abroad. Teachers can use the linguistic differences, such as adpositions use in a writer meaning the comment is 33.7% more likely to be from an 'AskEurope' commenter.

Future plans for the model include looking into using the model for other languages, topics over time using datetimes, slang, which topics and communication styles are the most popular and discussion inducing.

**Data Dictionary**
<table>
  <tr>
   <td><strong>Data</strong>
   </td>
   <td><strong>Data Type</strong>
   </td>
   <td><strong>Description</strong>
   </td>
  </tr>
  <tr>
   <td>num_comments
   </td>
   <td>int
   </td>
   <td>Number of comments of the subreddit
   </td>
  </tr>
  <tr>
   <td>num_upvotes
   </td>
   <td>int
   </td>
   <td>Number of upvotes of the subreddit
   </td>
  </tr>
  <tr>
   <td>subreddit
   </td>
   <td>0 or 1
   </td>
   <td>Subreddit to which the data belongs, 0: AskAnAmerican, 1:AskEurope
   </td>
  </tr>
  <tr>
   <td>comment_length
   </td>
   <td>int
   </td>
   <td>Length of the comment by characters
   </td>
  </tr>
  <tr>
   <td>question_length
   </td>
   <td>int
   </td>
   <td>Length of the question (title and body of the post) by characters
   </td>
  </tr>
  <tr>
   <td>POS_comment
   </td>
   <td>float
   </td>
   <td>Ratio of one Part of Speech used over others in the comment section
   </td>
  </tr>
  <tr>
   <td>emojis
   </td>
   <td>float
   </td>
   <td>Use of emojis compared to other punctuation in the comments
   </td>
  </tr>
  <tr>
   <td>Punctuation
   </td>
   <td>float
   </td>
   <td>Use of different types of punctuation compared to others in the comment section
   </td>
  </tr>
  <tr>
   <td>Topics
   </td>
   <td>float
   </td>
   <td>Topics(0-4) measured by frequency of occurrence in the post. Determined using lemmatized words from the title, body and comment combined.  
   </td>
  </tr>
  <tr>
   <td>Words
   </td>
   <td>float
   </td>
   <td>Words measured by frequency of occurrence in the post, calculated by lemmatized words from the title, body and comment combined.
   </td>
  </tr>
  <tr>
   <td>comment_subjectivity
   </td>
   <td>float
   </td>
   <td>How subjective the comments were, calculated using lemmatized text from comments
   </td>
  </tr>
  <tr>
   <td>comment_polarity
   </td>
   <td>float
   </td>
   <td>How polarizing the comments were, calculated using lemmatized text from comments
   </td>
  </tr>
</table>


**Sources**

[https://stackabuse.com/python-for-nlp-tokenization-stemming-and-lemmatization-with-spacy-library/](https://stackabuse.com/python-for-nlp-tokenization-stemming-and-lemmatization-with-spacy-library/)

[https://swatimeena989.medium.com/beginners-guide-for-preprocessing-text-data-f3156bec85ca#f46e](https://swatimeena989.medium.com/beginners-guide-for-preprocessing-text-data-f3156bec85ca#f46e)

[https://medium.com/@yanlinc/how-to-build-a-lda-topic-model-using-from-text-601cdcbfd3a6](https://medium.com/@yanlinc/how-to-build-a-lda-topic-model-using-from-text-601cdcbfd3a6)

[https://stackoverflow.com/questions/14984119/python-pandas-remove-duplicate-columns](https://stackoverflow.com/questions/14984119/python-pandas-remove-duplicate-columns)

[https://towardsdatascience.com/is-random-forest-better-than-logistic-regression-a-comparison-7a0f068963e4](https://towardsdatascience.com/is-random-forest-better-than-logistic-regression-a-comparison-7a0f068963e4)

[https://stackoverflow.com/questions/24399820/expression-to-remove-url-links-from-twitter-tweet](https://stackoverflow.com/questions/24399820/expression-to-remove-url-links-from-twitter-tweet)

[https://www.geeksforgeeks.org/generating-word-cloud-python/](https://www.geeksforgeeks.org/generating-word-cloud-python/)

[https://matplotlib.org/stable/gallery/color/colormap_reference.html](https://matplotlib.org/stable/gallery/color/colormap_reference.html)

[https://www.geeksforgeeks.org/generating-word-cloud-python/](https://www.geeksforgeeks.org/generating-word-cloud-python/)

[https://amueller.github.io/word_cloud/generated/wordcloud.WordCloud.html](https://amueller.github.io/word_cloud/generated/wordcloud.WordCloud.html)

[https://matplotlib.org/stable/gallery/color/colormap_reference.html](https://matplotlib.org/stable/gallery/color/colormap_reference.html)

Class notes from [DSI at GA](https://git.generalassemb.ly/DSIR-412/course-info)
