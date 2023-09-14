# Why Session-based music recommender system?
The recommender system developed in this project is session-based. Music is usually listened to, in sessions and every session could be a lot different from the last one, even for the same listener. The songs we listen to, in a session could be result of a lot of factors involved like the time of the day, the company we are in, the vibe we are feeling, and so on. Hence it is important to understand that more similar songs are listened to, in the same session. 

# Dataset information
The Last.fm 1k users dataset is used which contains 2 TSV files. One with the information of user data and the other that I have used, which contains the music listening history of 1k users with timestamps. The TSV file contains columns including- user ID, timestamp, track ID, track Name, artist ID and artist Name. The columns user ID, track ID, track Name, and timestamp are mainly used in this project. Artist information is used to understand if songs belong to the same or similar artists.
Some important properties of last.fm dataset are:
![image](https://github.com/ljain72/Music_recommender_system/assets/53551018/bf2f16f7-727c-48c3-be31-dba092e98009)


# Pre-processing Steps
The following pre-processing steps were undertaken to get session data for users:
1. Renaming column names to more understandable and readable form.
2. Removing rows with empty track ID and track Name.
3. Changing the timestamp format to '%Y − %m − %d %H : %M : %S', to obtain time difference between two consecutive timestamps.
4. Sorting out rows in ascending order of timestamp grouped by users.
5. Creating sessions by checking the timestamp between two consecutive songs listened to, by the listener. If the timestamp duration is more than 6 minutes, the session is changed.
6. Creating a tagged document containing the list of song IDs listened to, in a session, with the tag of session ID. This is done to make it inputable by the Doc2Vec algorithm.
# Test train validation split
The session data is split into train - 40%, test - 48% and validation - 12%. The reason for train data to be less is that the last.fm data is huge and to run doc2vec on the data requires a lot of time and capacity. The validation data was used in hypertuning.
# Methodology
The recommendations were generated using three approaches:
1. SessionSong2Vec embeddings recommendations: This is created by using Doc2Vec Deep learning algorithm that creates two embedding spaces, session space and song space. These embedding spaces have similar sessions and songs placed near each other. The most similar sessions and songs are recommended for a test session to predict the next song.
2. PPR on Session graph: Session graph is created using sessions created during pre-processing.
   The figure shows how the session data is transformed to graph.
   ![weighted_graph](https://github.com/ljain72/Music_recommender_system/assets/53551018/ec3b31fd-9e8d-4584-99d7-304cb1438ee0)
The weights on the graph indicates the number of times a track is played after another track. The personalized Page rank algorithm is ran on the test session to generate most important nodes to the one in the test session.
4. PPR on Similarity graph: Similarity graph is created using the Song similarity embeddings created using Doc2Vec. 10 most similar songs with similarity score more than 0.5 are obtained and connected to a song. The weight of the edge is the similarity score. The PPR is calculated on test data to obtain top 10 recommendations.
   ![graph drawio (1)](https://github.com/ljain72/Music_recommender_system/assets/53551018/acd36093-0deb-45f9-a195-d23af18d2ba4)

# Evaluation Metrics
The following evaluation metrics were used for evaluation:
1. Overall Hit Rate - This is the measure to check if the actual next song is in the list of predicted songs. The partial hit rate is also calculated to check if the artist of the actual next song is in the predicted artist list if the song is not present in the list.
2. Mean Reciprocal Rank - This is the mean of the reciprocal of the rank of the actual song in the predicted songs list.

# Results
The Overall Hit Rate and Mean Reciprocal Ranks were calculated for 150 samples for the 3 above-mentioned recommendation systems and compared against baseline models, including random recommendations and most popular recommendations. The OHR and MRR for all models is displayed in the figure.
1. OHR
  ![OHR](https://github.com/ljain72/Music_recommender_system/assets/53551018/4ae46cc1-0e09-4335-8bcc-0121a8a19d5f)

3. MRR
   ![MRR](https://github.com/ljain72/Music_recommender_system/assets/53551018/de8b217c-4861-4927-a0e3-4d2866b59f36)


# Conclusion
The embedding approach performs better than all the approaches in both MRR and OHR. The high MRR and OHR prove the importance of sessions in recommending songs to the listener.
