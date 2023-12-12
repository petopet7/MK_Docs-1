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

This section shows how to generate sample data using the package.
```python
from etl.data_preperation.data_generator import .
NUMBER_OF_MOVIES = 450
```
```python
movie_data = [generate_movie_metadata(movie_id) for movie_id in range(NUMBER_OF_MOVIES)]
movie_df = pd.DataFrame(movie_data)
movie_df.to_csv('movie_metadata.csv', index=False)


...
