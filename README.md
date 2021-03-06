# Bitcoin-Price-Prediction-LSTM


Being a data scientist and a part-time crypto trader, I have been very interested in creating a Deep Learning model that could help me predict Bitcoin price. So, this article is based on my experimenting with LSTM to create a reliable and accurate 
A link to my Git Project is here: https://github.com/msaleem18/Bitcoin-Price-Prediction-LSTM


## LSTM's
Long short term memory, or more popularly known as LSTM's, are a type of Recurrent Neural networks that help the model learn long term sequences in the data set. Since my focus here is more on their usage, if you are interested in knowing more details about what LSTM's are and how they work, you can check out this great article that goes in depth to explain all that.
Dataset
For this project, I used Binance BTC/USDT hourly price dataset that can be found here: http://www.cryptodatadownload.com/data/binance/

## Binance BTC/USDT DatasetPre-processing
The dataset provided by Binance needs to cleaned up:
- They have been collecting data since 2017 and since then the format of some of the fields in the dataset has changed.
- The "date" field in the dataset is pretty useless - we need to create our own
= Add a few additional fields that'll help us in the modelling process

For this model I created fields to track the hour of the day and the weekday.

## Normalization
Normalization is pretty important for LSTM modelling. The price of Bitcoin, since its inception, has varied a great deal starting from almost $0 reaching an all time high of $57,000 in recent months; this is not good for the model. The higher numebrs will end up gaining more weightage and we'll end up with a biased model.
In order to avoid this skewness we have to normalize our data. There are quiet a few normaliation methods availble in the sklearn library, however i decided to go with MinMax Scalar. I experimented with a few other methods such as Robust Scalar, however my model performed the best with MinMax Scalar.
One more thing to note, the best practice is to only normalize your training data and then use that same method to normalize your test data. However, since the price of Bitcoin has recently increased at a lot higher rate than in previous years this causes an issue with the scalar method. Since we are doing time-series prediction and the data is sorted in ascending order, the earlier lower prices are picked for the training dataset and the new higher prices are only available in the test dataset. Just for this project i took the shortcut and to avoid excessive coding i normalized the whole dataset and only split it into train and test after the normalization step. The best practice would be to use window scaling as explained here https://www.datacamp.com/community/tutorials/lstm-python-stock-market

## Lagging Dataset
Once we have our cleaned and normalized data we need to create a lagging dataset for LSTM time-series prediction. What do i mean by a lagging dataset?
Basically how the LSTM time-series prediction work is that it looks back at x hours of data and tries to predict what will be the price at x+1 hour. The following image tries to explain this concept.

## Modelling
Now comes the exciting part, creating the actual LSTM model. For me at least this turned out to be pretty simple and quick. I ended up using Keras's Bidirectional LSTM's with a Batch Normalization layer in my architecture.
I exprimented with a a bunch of other complex architectures, to a name a few:
- LSTM's with different activation functions (tanh vs relu)
- Stacked LSTM layers (2 and 3 LSTM layers)
- CNN + LSTM model

However, my model performed the best using a single LSTM layer only; my mean-squared-error was the lowest for this architecture.

## Conclusion
Even though the model is performing pretty well I still beleive there is a lot of room for improvement. The crypto market is very volatile and this model is not using any information from current news or trends. An important enhancement would be to use Natural Language Processing (NLP) and feed in news data into this model.
I'm going to use this model in my trading in the upcoming weeks and see how well it performs.
