# Project: Analysis of Parallel Bus Routes to MRT Lines

## Project Overview
This project aims to analyze bus routes that significantly overlap with MRT lines in Singapore. By identifying redundancies, we propose potential adjustments to streamline services and optimize resource allocation, ensuring a sustainable and efficient public transport system that meets commuter needs.

## Prerequisites
1. **LTA DataMall API Key**: This project requires access to Singapore’s Land Transport Authority (LTA) DataMall API for bus and MRT route data. 
   - [Sign up for an LTA DataMall account](https://www.mytransport.sg/content/mytransport/home/dataMall.html).
   - Once registered, apply for an API key under the **DataMall API** section.
   - Note down the API key provided.

2. **Environment Setup**:
   - Ensure you have Python installed on your machine.

## Setup Instructions

You should set up your own virtual environment on your laptop to run all the notebooks in this repository. You may do this in your preferred way, but a simple way to set this up would be:

1. Run `python -m venv venv` to create your virtual environment (note that I am using Python 3.11.5)
2. Run `source venv/bin/activate` to activate the virtual environment
3. Run `pip install -r requirements.txt` to install the full list of requirements

4. Create a .env file named key.env in the root directory to store your LTA API key securely:
    Open key.env and add the following line, replacing YOUR_API_KEY with the API key you obtained from LTA DataMall:
`API_KEY=YOUR_API_KEY`
    Save the file.
5. Download dataset from Google drive link
6. Ensure your directory structure looks like this:
project-root/
├── src/
├── dataset/
├── key.env
├── requirements.txt
└── README.md

