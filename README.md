# PORTFOLIO-WEBSITE-WITH-ADVANCED-ANIMATIONS
Import pandas as pd 
from pymongo import MongoClient

data = pd.read_csv(‘your_csv_file.csv’)

mongoClient = MongoClient(‘MongoDB Atlas URL with port’) 
db = mongoClient['your_database'] 
collection = db['your_collection']

jsondata = data.to_dict(orient='records')
collection.insert_many(jsondata);