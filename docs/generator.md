# Data Generator

## Introduction
The `data_generator` module is designed to create mock data for movies, users, and their interactions. This module is especially useful for simulating a movie database environment for testing and development purposes.

## Key Functions
- `generate_movie_metadata(movie_id)`: Generates random metadata for a movie.
- `generate_user(user_id)`: Creates a fake user profile.
- `generate_ratings(user_id, movie_id)`: Generates a random movie rating by a user.
- `generate_credits(movie_id)`: Creates a random list of cast and crew for a movie.
- `generate_keywords(movie_id)`: Generates random keywords related to a movie.

## Usage Example

This section shows how the data is generated.

```python

Declaring Constants
NUMBER_OF_MOVIES = 450
NUMBER_OF_LINKS = 270
NUMBER_OF_RATINGS = 260
NUMBER_OF_TAGS = 750
NUMBER_OF_CREDITS = 300
NUMBER_OF_USERS = 200

Generating Movie MetaData

movie_data = [generate_movie_metadata(movie_id) for movie_id in range(NUMBER_OF_MOVIES)]
movie_df = pd.DataFrame(movie_data)
movie_df.to_csv('movie_metadata.csv', index=False)
Generating Ratings Data
ratings_data = [generate_ratings(user_id, movie_id) for user_id in range(NUMBER_OF_RATINGS) for movie_id in range(NUMBER_OF_MOVIES)]
ratings_df = pd.DataFrame(ratings_data)
ratings_df.to_csv('ratings.csv', index=False)

Generating Credits Data

credits = [generate_credits(movie_id) for movie_id in range(NUMBER_OF_CREDITS )]
pd.DataFrame(credits).head()
credits_df = pd.DataFrame(credits)
credits_df.to_csv('credits_data.csv', index=False)

Generating Keywords Data

keywords = [generate_keywords(movie_id) for movie_id in range(NUMBER_OF_MOVIES)]
keywords_df = pd.DataFrame(keywords)
keywords_df_df = pd.DataFrame(keywords)
keywords_df.to_csv('keywords.csv', index=False)

Generating User Data

users = [generate_user(user_id) for user_id in range(1, NUMBER_OF_USERS + 1)]
users_df = pd.DataFrame(users)

# Save user data to a CSV file
users_df.to_csv('users.csv', index=False)


...
