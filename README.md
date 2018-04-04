# Capstone Project - The Mood of Music
The Capstone Project is my final project from my Data Science Immersive program experience at General Assembly. 
Key element in the project had to be:
- Develop an interesting question
- Collect the data required to model that data
- Developing the strongest model(s) for prediction
- Communicat the findings to other data scientists and non-technical individuals. 


## Business case
In what of the top 10 music countries should a record label release a new artist?<BR />
Based on the mood of the music and what music that is popular in the countries.
(On the top 10 music countries 2 of them are asian countries. Since it is challenging to work with asian signs compares to roman signs I have decided that it is only going to be the to 8 music countries. I will exclude the 
2 asian countries.)


## Description and target

**TARGET / Y**
Happy, Sad, Neutral comes from the column Valence in the dataframe. Originaly commes from the Spotify API.
The variable goes from 0 to 1. It describing the musical positiveness of the track. Tracks with high valence sound more positive (e.g. happy, cheerful, euphoric), while tracks with low valence sound more negative (e.g. sad, depressed, angry). I need to decide upon what interval levels I will use for happy, neutral and sad (for example 0 <= sad < 0.40, 0.40 <= neutral < 0.60, 0.60 <= happy < 1.0). Use mood of a song to get mood of artist and mood of country.

**Phase 1**
MVP: I will try to predict if a song is happy, neutral or sad. 
More improved models with more data.

**Phase 2**
Determin the mood of an artist based on the songs made of the artist.
Determin the mood of a country based on the songs popular in that country.



## Limitations
- Top 10 music countries (exclude 2 Asian countries due to other signs in written language)
- Only songs with roman characters (not asian signs for example)
- Assumption: All songs in toplist can be considered popular. If a song is not in toplist, it is not popular.
- Only look at data for 2017.


## The Data
**Initial data**<BR />
- Spotify world wide daily song ranking:<BR /> 
https://www.kaggle.com/edumucelli/spotifys-worldwide-daily-song-ranking/<BR />
- Song lyrics:<BR /> 
https://www.kaggle.com/artimous/every-song-you-have-heard-almost/<BR />

**More data**<BR />
- Additional song features from Spotify API (energy, mode, tempo, uri etc.)<BR />
https://developer.spotify.com/web-api
- Additional lyrics from Genius. Scraping through the API.<BR />
https://docs.genius.com/
- Lyrics manually added.


## Tools (needs to be completed)
- TextBlob
- NLP
- Linear Regressor
- Lasso Regressor
- Random Forest Regressor
- Ada Boosting Regressor
- Gradient Boosting Regressor
- GridSearchCV
- RandomizedSearchCV


## Technical Report (separat md-file)
Status: In progress

How to replicate the modeling process.
1. How did I acquired my data.
2. How the data should be transformed.
3. How I operationalized my outcome variable.
4. How I choose my models and hyperparameters (also including what metric to use to determine if a model 
successful or not).
4. Any future deployment strategies, additions of data, or modeling techniques I have yet to try. 


## Jupyter Notebook 1 - cleaning_top_songs
Status: first version done

This notebook include: 
- Loading the data from csv-file
- Get to know the data & some initial fix
- Save data to new csv-file<BR />

### Get to know the data & some initial fix
- Looked at entire df<BR />
	Shape: (3441197, 7)<BR />
	Columns: Position, Track Name, Artist, Streams, URL, Date, Region

		Position
		- No missing values
		- Data type integer64

		Track Name
		- 657 missing values (rows was removed)
		- Data type string
		- Comment: I might want to decide not to use data from the Asian region since they have Artists 
		and Track Names in foreign signs. I did choose to remove the rows with missing values bc I came 
		to the conclusion that it would be impossible to fill them in with other values. 657 rows out of 
		3441197 are also very few and they seemes to be distributed over different regions.

		Artist
		- 657 missing values (rows was removed, same rowes as for Track Name)
		- Data type string
		- Comment: I might want to decide not to use data from the Asian region since they have Artists 
		and Track Names in foreign signs. I did choose to remove the rows with missing values bc I came 
		to the conclusion that it would be impossible to fill them in with other values. 657 rows out of 
		3441197 are also very few and they seemes to be distributed over different regions.

		Streams
		- No missing values
		- Data type integer64

		Date
		- No missing values
		- Data type string (was converted into datetime)
		- 3 new columns created for Year, Month, Day
		- There was data from 1 jan 2017 to 9 jan 2018. I removed the 9 days from 2018. I only want data 
		for 2017.

		Region
		- Column renamed to Country
		- No missing values
		- Data type string
		- A new column (Region) was created featuring groups of countries, based on what continent they 
		belong to.
		- Comment: I did find a dictionary for what country each letter combination was exual to. (Is saves 
		in the file contry_region_explanation). In the same file you can also see what countries go into 
		what region. There is also a country that equals to global, which is for the world (my 
		interpretation; all countries together). That column can be used to see if a specific region or 
		country deviates a lot from what the rest of the world thinks. 

	New shape: (3355829, 11)<BR />
	Columns: Position, Track Name, Artist, Streams, URL, Date, Year, Month, Day, Country, Region<BR /> 
	(the order of the columns also was changed)


## Jupyter Notebook 2 - cleaning_lyrics
Status: first version done

This notebook include: 
- Loading the data
- Get to know the data
- The data set were actually saved as 2 csv-files. I had to combined the 2 into one data frame to make 
the work easier.
- Drop rows were Song or Lyrics were missing. Extremely few. 
- Clean the data from '\r\n', '\n\n', '\r', '\n'.
- Make all the Lyrics into lower casing.
- Save data to new csv-file<BR />

### Get to know the data
- Looked at data from 3 csv-files
	ArtistURL (not using)<BR /> 
	Lyrics1 
	- Shape: 250000,3)<BR /> 
	- Columns: Band, Lyrics, Song<BR /> 
	- 1 missing value in Song
	Lyrics2 
	- Shape: 266174,3<BR /> 
	- Column: Band, Lyrics, Song<BR /> 
	- 7 missing values in Lyrics


## Jupyter Notebook 3 - combined_data
Status: first version done

This notebook include: 
- Loading the data
- Some light data munging
- Combinding songs and lyrics into one data frame
- Save data to new csv-file<BR />


## Jupyter Notebook 4 - spotify_api
Status: first version done

This notebook include: 
- Get data from Spotify API
- Use song URL from the data frame with songs and lyrics in
- Get to know the variables available in the API https://developer.spotify.com/web-api/get-audio-features/
- Save data to new csv-file<BR />


## Jupyter Notebook 5 - combined_data2
Status: first version done

This notebook include: 
- Loading the data (songs_lyrics + spotify_api_data)
- Combinding songs and lyrics into one data frame
- Drop columns that are not going to be used
- Drop rows that are duplicates
- See for how many unique songs I have lyrics.
- Save data to new csv-file<BR />


## Jupyter Notebook 6 - get_more_lyrics
Status: first version done

This notebook include: 
- Getting data from Genius using API and then web scraping
- Save all data to new csv-file
- Save unique song and lyrics to csv-file<BR />


## Jupyter Notebook 7 - combined_data3
Status: first version done

This notebook include: 
- Loading the data (songs_lyrics_api_data + genius_unique)
- Combinding new lyrics into one data frame (consisting rows of the top 8 music countries 
(US, UK, Germany,France, Canada,)
- Save data to new csv-file<BR />


## Jupyter Notebook 8 - top_8_countries
Status: first version done

This notebook include: 
- Loading the data (latest version of combined data)
- Looking at missing lyrics for all countries
- Looking at missing lyrics for top 10 countries (top 8 without asia)
- Looking at which songs appear most frequently
- Manually add lyrics for the songs that appear more than 1000 times
- Save data to new csv-file<BR />


## Jupyter Notebook 9 - mvp_mood_of_song
Status: first version done

This notebook include: 
- Remove duplicated songs (removes rows)
- Remove columns that might change per song (ex. position, streams, country etc.)
- Looking at missing values - focus on the Lyrics column (the one with missing values are dropped later in this notebook)
- Use TextBlob for the Lyrics column and sae result as 2 new column (Polarity and Subjectivity)
- Do a quick check on the entire data frame
- Performe a train/test-split
- Do some more EDA
- Define what is happy, neutral and sad
- Model+tune
- Look at the predictions


## Jupyter Notebook 10 - good_model
Status: first version done

This notebook include: 
- Do the same as in notebook 9 but add som NLP before modeling
- Make list of stop words in eng, spa, ger, fre
- CountVectorizer
- Models (LinReg, LassoR, RFR)
- TF-IDF
- Models (LinReg, LassoR, RFR)
- Evaluate<BR />


## Jupyter Notebook 11 - better_model
Status: first version done

This notebook include: 
- Do the same as in notebook 10 but brooken down for each of the 8 countries
- Use stop word list
- CountVectorizer and TF-IDF
- Model LassoR for every country
- Evaluate<BR />


## Jupyter Notebook 12 - best_model
Status: in progress

This notebook include: 
- Do the same things as for the MVP (notebook 9). But stop before you drop the columns that may change for a 
song on different rows. 
- Get dummies for the 8 countries.
- Get dummies for the position column.
- Group by the column ID and summarize the rows together that are grouped. 
- Create an average streams column.
- Create an average position column.
- Drop duplicates.
- Drop columns that before might changed per song (per row)
- Put all the new columns into the data frame consisting of the 8 countries and the columns that would not 
change per song (per row): 'Position', 'Streams', 'Date', 'Year', 'Month', 'Day', 'Country', 'Region'.
- Drop rows with missing values in the Lyrics column.
- Use TextBlob on the Lyrics column to get 2 new columns for Polarity and Subjectivity.
- Performe a train/test-split
- Do some more EDA
- Use stop word list
- CountVectorizer
- Models (LinReg, LassoR, RFR, AdaBoost, GradientBoost)
- TF-IDF
- Models (LinReg, LassoR, RFR, AdaBoost, GradientBoost)
- Evaluate<BR />


## Jupyter Notebook 13 - 
Status: in progress

This notebook include: 
- Do the same thing as for notebook 12 but for each country
-
-


## Jupyter Notebook 14 - break_per_country
Status: First version done

This notebook include: 
- Look at the data per country, just to get some facts
- How many times a song appeard on top 200 in each country
- Total number of times on top 200 for the song
- Average number of times on top 200 for the song
- What song is the most popular in what country
- Plot some different songs appearance on top 200 per country.

Why did I do this. It is because I am interested in seeing the differences between countries. 
It also helps me to get more info to understand the different markets and there tastes. 


## Jupyter Notebook 15 - comparing_models
Status: in progress

This notebook include: 
- Take code from notebook 12 (remove stuff that do not need to be there)
- Run the models 3 times with different random sees (24, 42, 57) to get 3 different R2-scores
- Take the STD of the 3 R2-scores and make error bars for the different models.


## Jupyter Notebook 16 - visuals_final_data
Status: in progress

This notebook include: 
- Make some more plots based on the final data
-
-


## Jupyter Notebook n
Then when we have the best model we can use our predictions and decide the mood of a country and the 
mood of an artist. Then we can say what artist is suitable for what country.
(Since low quality, maybe just for on eor two countries, depending on the individual performances)

And in the end the concution and presentation.

I will update this document as the project progresses.


## .gitignore
This prevents checkpoints and csv-files from being commited and pushed to remote repo.

## This is a working copy
The project is still active and not finnished yet.
So please keep that in mind. 
