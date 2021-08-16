# Forecasting-COVID-19-cases
Forecasting of COVID-19 cases using deep learning


## Introduction
The goal of this project is to give a fair estimate of covid cases in India. I found a published article on [Forecasting COVID-19 cases](https://www.sciencedirect.com/science/article/pii/S2211379721000048). They were able to make predictions with an error of less than 2%. Here I have tried to implement their learnings and try to make predictions for the next few days.

You can check out the [article version](https://realnihal.github.io/2021/08/15/COVID19-forecasting.html).

## Importing data
I have imported the covid-19 data from [this source](https://documenter.getpostman.com/view/10724784/SzYXXKmA). Many Volunteers have pre-cleaned and collected the data. We get access to various metrics but are only interested in the “Daily Case” counts.

We have to normalize the data to increase the accuracy of the model.

The time-series data that we have must be converted into windows. It defines the no of days the model looks into the past to predict the future. I have chosen to have the window size as 30 and the predicting horizon of 1 day. You can check out the [complete code](https://github.com/realnihal/Forecasting-COVID-19-cases/blob/main/COVID19_forecasting_using__deeplearning.ipynb) to understand how I did it. Training and testing data is created by splitting the windowed data that we have. I have used a split ratio of 0.2. We are creating a model checkpointing callback using the [tensorflow callback function](https://www.tensorflow.org/api_docs/python/tf/keras/callbacks/ModelCheckpoint). This allows us to save only the best model that is trained across many epochs.

## About the model
The main reason for stacking LSTM is to allow for greater model complexity. In a simple feedforward net, we stack layers to create a hierarchical feature representation of the input data for some machine learning task. The same applies to stacked LSTM’s. At every time step, an LSTM, besides the recurrent input. If the information is already the result from an LSTM layer (or a feedforward layer), then the current LSTM can create a more complex feature representation of the current input. This model and its parameter were derived from the extensive testing done in this [paper](https://reader.elsevier.com/reader/sd/pii/S2211379721000048?token=96B6C9E2813943F5D2FE4882F66A79AFA5E8779BC525996AA7E6F9EE1B924E254C50FC4994A800B07CE92EADF065D17B&originRegion=eu-west-1&originCreation=20210815022455).

## Results
We can see that we have achieved an error of 2.7%, which is slightly higher than the original [paper](https://reader.elsevier.com/reader/sd/pii/S2211379721000048?token=96B6C9E2813943F5D2FE4882F66A79AFA5E8779BC525996AA7E6F9EE1B924E254C50FC4994A800B07CE92EADF065D17B&originRegion=eu-west-1&originCreation=20210815022455). Let’s try to use this model to predict future cases.

## Conclusions

We can see the rising trend of an upcoming third wave in the country. We have to consider a lot of things before we take this model seriously, such as:

We are using a single feature (univariate) to make the prediction. This may not be accurate as the actual trends could be more correlated to other factors.

The further in the future we want to predict, the less accurate the model becomes. This means that the actual slope may not be exact. The peak or duration of the third wave might by varying a lot.

Nevertheless, this is an alarming sign that the public should be prepared. I really wish this doesn’t happen and the model is wrong, but it’s still a good idea to increase precautions and save yourself.
