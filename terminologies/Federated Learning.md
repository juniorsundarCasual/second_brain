# Definition
Federated learning is a way to train machine learning models without centralizing the data. Instead of bringing all the data to a single server, the model itself is sent out to where the data lives (like on your phone or at different hospitals).

The training happens on each device locally using its own data. Only updates to the model are shared back, not the raw data itself. This is a big advantage for preserving privacy.

The training occurs in multiple rounds, with the updated model being shared and then further refined at each device before the cycle repeats.

# Purpose 
- *Data Privacy* - It allows us to use data for improving AI models without compromising sensitive information (like medical records or your personal browsing behavior).
- *Handling Limited Data* - Even if privacy isn't a concern, FL is useful when individual devices don't have enough data on their own to train a good model. Collaboration makes better results possible.
- *Real-time Adaptation* - Models can be continuously updated based on new data patterns as they emerge, which is important for things like improving keyboard prediction on your phone.

# Methodology
- *Start with a Model* - A basic model is created on a central server.
- *Send Out the Model* - This model is distributed to a bunch of devices (smartphones, sensors, etc.).
- *Local Training* - Each device uses its own data to train the model a little bit.
- *Share the Updates* - Rather than sending the raw data, only the changes made to the model during local training are sent back to the server.
- *Aggregation* - The server combines the updates from multiple devices, improving the overall model.
- *Repeat* - The improved model is sent out again, and the whole process repeats, getting more refined over time.
