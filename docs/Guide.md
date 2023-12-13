# User's Guide

## Installation
1. Clone the repository: `git clone [repository-url]`
2. Navigate to the project directory: `cd [project-directory]`
3. Install dependencies: `pip install -r requirements.txt`

## Setting Up the Database
Run the `setup.py` script to set up the initial database and environment:


    python setup.py

Run this line to install our package

    !pip install movie_recommend
    
# Data Generation

**This section shows how to generate sample data using the package.**


    from etl.data_preperation.data_generator import .

    NUMBER_OF_MOVIES = 450
    NUMBER_OF_LINKS = 270
    NUMBER_OF_RATINGS = 260
    NUMBER_OF_TAGS = 750
    NUMBER_OF_CREDITS = 300
    NUMBER_OF_USERS = 200

**Generating Movie MetaData**

    from etl.data_preperation.data_generator import .

    movie_data = [generate_movie_metadata(movie_id) for movie_id in range(NUMBER_OF_MOVIES)]
    movie_df = pd.DataFrame(movie_data)
    movie_df.to_csv('movie_metadata.csv', index=False)

**Generating Ratings Data**

    ratings_data = [generate_ratings(user_id, movie_id) for user_id in range(NUMBER_OF_RATINGS) for movie_id in range(NUMBER_OF_MOVIES)]
    ratings_df = pd.DataFrame(ratings_data)
    ratings_df.to_csv('ratings.csv', index=False)


**Generating Credits Data**


    credits = [generate_credits(movie_id) for movie_id in range(NUMBER_OF_CREDITS )]
    pd.DataFrame(credits).head()
    credits_df = pd.DataFrame(credits)
    credits_df.to_csv('credits_data.csv', index=False)


**Generating Keywords Data**
    
    keywords = [generate_keywords(movie_id) for movie_id in range(NUMBER_OF_MOVIES)]
    keywords_df = pd.DataFrame(keywords)
    keywords_df_df = pd.DataFrame(keywords)
    keywords_df.to_csv('keywords.csv', index=False)


**Generating User Data**
   
    users = [generate_user(user_id) for user_id in range(1, NUMBER_OF_USERS + 1)]
    users_df = pd.DataFrame(users)
    users_df.to_csv('users.csv', index=False)    


# Movie Recommendation System Schema Setup

**This code demonstrates the setup and schema definition for a movie recommendation system using SQLAlchemy, a Python SQL toolkit and Object-Relational Mapping (ORM) library.**

    from etl.data_preperation.schema import *

    
**This simple line will create schema for you movie dataset**


# SQL Connection

**Establish a SQL connection and execute a query to retrieve movie data.**

## Create SQL handlers for each table

    Inst_movie_metadata = SqlHandler('temp', 'MovieMetadata')
    Inst_user = SqlHandler('temp', 'User')
    Inst_ratings_small = SqlHandler('temp', 'Ratings_small')
    Inst_credits = SqlHandler('temp', 'Credits')
    Inst_keywords = SqlHandler('temp', 'plot_keywords')


 ##  Load data from respective CSV files

 
    movie_data = pd.read_csv('movie_metadata.csv')
    user_data = pd.read_csv('users.csv')
    ratings_data = pd.read_csv('ratings.csv')
    credits_data = pd.read_csv('credits_data.csv')
    keywords_data = pd.read_csv('keywords.csv')



 ##  Insert data into respective SQL tables

 
    Inst_movie_metadata.insert_many(movie_data)
    Inst_user.insert_many(user_data)
    Inst_ratings_small.insert_many(ratings_data)
    Inst_credits.insert_many(credits_data)
    Inst_keywords.insert_many(keywords_data)

## Close connections for each table


    Inst_movie_metadata.close_cnxn()
    Inst_user.close_cnxn()
    Inst_ratings_small.close_cnxn()
    Inst_credits.close_cnxn()
    Inst_keywords.close_cnxn()    

# Recommendation Algorithm

## Use the recommendation algorithm on the generated data.

    from etl.movie_recommender.check import get_recommendations_for_movie

    test_movie_title = "Organized mission-critical core"
    recommendations = get_recommendations_for_movie(test_movie_title)

    # Print recommendations
    print(f"Recommendations for '{test_movie_title}':")
    for recommendation in recommendations:
        print(recommendation)

# FastAPI Integration
## Example of integrating the recommendation algorithm with FastAPI.

    import uvicorn
    import os
    import threading
    import uvicorn
    from etl.api.api import app


    def run_server():
        uvicorn.run(app, host="127.0.0.1", port=5001, log_level="info")

# Run the server in a separate thread

    server_thread = threading.Thread(target=run_server)
    server_thread.start()

After this steps copy the host link and paste it in the search bar adding "docs" after it.
There are different functionalities in the FASTAPI page.
We can CREATE a movie, UPDATE a movie, delete a movie in our data. Also there is a function to get recommendation based on the imput of a movie name.


With all these steps it will be very easy to run the package even if the user does not have a lot of experience in Python coding.
