# imdb-data-scraper
IMDB Project using Python libraries

import requests
from bs4 import BeautifulSoup
import pandas as pd

url = "https://www.imdb.com/chart/top"

response = requests.get(url)

soup = BeautifulSoup(response.content, "html.parser")

movies = []

table = soup.find("table", attrs={"class": "chart"})
for row in table.find_all("tr")[1:]:
    title_column = row.find("td", attrs={"class": "titleColumn"})
    rating_column = row.find("td", attrs={"class": "ratingColumn imdbRating"})

    title = title_column.a.text
    year = title_column.span.text.strip("()")
    rating = float(rating_column.strong.text)

    movie = {
        "title": title,
        "year": year,
        "rating": rating
    }
    movies.append(movie)

df = pd.DataFrame(movies)
df.to_excel("movies.xlsx", index=False)
