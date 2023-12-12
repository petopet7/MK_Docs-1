# Setup

## Introduction
This script is used to set up the movie database application, including installing dependencies, setting up the environment, and performing initial configurations.

## Key Steps
- Detail the steps involved in the setup process.
- Include any environment configuration or dependency installation instructions.

## Usage

    from setuptools import setup, find_packages

    setup(
        author='Group7',
        description='DS-223 Project',
        name='etl',
        version='0.1.0',
        packages=find_packages(include=['etl','etl.*']),
    
    )

...

