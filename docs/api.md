# API

## Introduction
The `api.py` module is responsible for handling API requests related to the movie data. It defines various endpoints for accessing and manipulating movie-related data.

## Key Endpoints
- Describe each API endpoint, its purpose, input parameters, and output format.
- Include any authentication mechanisms if applicable.

## Usage 

    from fastapi import FastAPI, HTTPException
    import sqlite3
    import logging
    import os
    from pydantic import BaseModel
    from typing import Any
    from ...movie_recommender.recommender import MovieRecommender
    from ...movie_recommender.database import preview
    import random
    #create instance called app
    app=FastAPI()

    
    logger = logging.getLogger(os.path.basename(__file__))
    logger.setLevel(logging.DEBUG)
    ch = logging.StreamHandler()
    ch.setLevel(logging.DEBUG)
    #ch.setFormatter(CustomFormatter())
    logger.addHandler(ch)

    # Defining a function to open a connection to our SQLite database
    def get_db():
      db = sqlite3.connect('temp.db')
      return db

    @app.get("/")
    async def root():
        return {"message": "Initializing"}



    @app.get("/get_data/{movie_id}")
    async def get_record(movie_id: int):
        # Open a connection to the database
        with get_db() as db:
            cursor = db.cursor()
            # Executing a SQL query to fetch the data
            cursor.execute(f"SELECT * FROM MovieMetadata WHERE movie_id = {movie_id}")
            record = cursor.fetchone()

    if record is None:
        return {"error": "Record not found"}

    # Converting the record to a dictionary 
    column_names = [description[0] for description in cursor.description]
    record_dict = dict(zip(column_names, record))
    return record_dict


    #Defining a Pydantic model for the data
    class CreatemovieRequest(BaseModel):
        movie_id: int
        title: str
        budget: int
        revenue: int
        release_date: str
        language: str
        production_country: str
        production_company: str
 

    # Columns: ['movie_id', 'title', 'budget', 'revenue', 'release_date', 'language', 'production_country', 'production_company']

    @app.post("/create_data")
    async def create_record(new_data: CreatemovieRequest):
        try:
            # Opening a database connection using the get_db function
            db = get_db()
            cursor = db.cursor()

            # Defining the SQL query to insert data into the table
            insert_query = """
            INSERT INTO MovieMetadata (
                movie_id, title, budget, revenue, release_date, language, production_country ,production_company
            )
             VALUES (?, ?, ?, ?, ?, ?, ?, ?)
            """

        # Executing the insert query with the data from the new_data parameter
        cursor.execute(insert_query, (
            new_data.movie_id, new_data.title, new_data.budget,
            new_data.revenue, new_data.release_date, new_data.language,
            new_data.production_country, new_data.production_company
        ))

        # Committing the transaction to save the data in the database
        db.commit()

        return {"message": "Record created successfully"}
    except Exception as e:
        logger.error(f"Failed to insert data: {str(e)}")
        raise HTTPException(status_code=500, detail=f"Failed to insert data: {str(e)}")
    finally:
        # Closing the database connection in the 'finally' block
        db.close()


    class UpdateRecordRequest(BaseModel):
        column_name: str  # The type that column gets
        new_value: Any # As it can be both integer and str, defined as Any. Later can find better solution
        movie_id: int  # The type that the movie_id is

    @app.put("/update_data")
    async def update_record(update_request: UpdateRecordRequest):
    try:
        # Opening a database connection using the get_db function
        db = get_db()
        cursor = db.cursor()

        # Defining the SQL query to update the specified column for the given record ID
        update_query = f"UPDATE MovieMetadata SET {update_request.column_name} = ? WHERE movie_id = ?"

        # Executing the update query with the new value and record ID
        cursor.execute(update_query, (update_request.new_value, update_request.movie_id))

        # Committing the transaction to save the data in the database
        db.commit()

        return {"message": "Record updated successfully"}
    except Exception as e:
        raise HTTPException(status_code=500, detail=f"Failed to update data: {str(e)}")
    finally:
        # Closing the database connection in the 'finally' block
        db.close()

    @app.get("/recomend")
    async def recomend(title: str):
    # Open a connection to the database
    movie_data = preview('MovieMetadata')
    recommender = MovieRecommender(movie_data)
    recommendations = recommender.recommend_movies(title, num_recommendations=10)
    if recommendations is None:
        return {"error": "Record not found"}

     
    db = get_db()
    cursor = db.cursor()

    # Defining the SQL query to insert data into the table
    insert_query = """
    INSERT INTO inout (
        movie_id, input_movie, output_recommendations
    )
    VALUES (?, ?, ?)
    """
    # Generate a random movie_id
    movie_id = recommender.get_movie_id(title)  # Adjust the range as needed

    # Convert the list of recommendations to a long string
    recommendations_str = ', '.join(recommendations)
    # Executing the insert query with the data
    cursor.execute(insert_query, (int(movie_id), title, recommendations_str))

    # Committing the transaction to save the data in the database
    db.commit()

    return recommendations
##Imput for recommendation
![image](https://github.com/petopet7/MK_Docs-1/assets/146641668/315ad8de-fd86-4be6-b44c-c8fe3288ae6c)
##Recommendation outcome
![image](https://github.com/petopet7/MK_Docs-1/assets/146641668/7d16b332-4030-48d1-8835-160a3b2927b1)

...



