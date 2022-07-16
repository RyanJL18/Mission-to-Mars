# Mission-to-Mars
## Overview of Project

### Background

>Robin's web app is looking good and functioning well, but she wants to add more polish to it. She had been admiring images of Mars’s hemispheres online and realized that the site is scraping-friendly. She would like to adjust the current web app to include all four of the hemisphere images. To do this, you’ll use BeautifulSoup and Splinter to scrape full-resolution images of Mars’s hemispheres and the titles of those images, store the scraped data on a Mongo database, use a web application to display the data, and alter the design of the web app to accommodate these images.


1. **_Deliverable 1_**: Scrape Full-Resolution Mars Hemisphere Images and Titles
2. **_Deliverable 2_**: Update the Web App with Mars Hemisphere Images and Titles
3. **_Deliverable 3_**: Add Bootstrap 3 Components
4. **_Extra_**: A written report on the employee database analysis [README.md](https://github.com/RyanJL18/Mission-to-Mars/edit/main/README.md).

### Resources and Before Start Notes:
- Data Source: ```Mission_to_Mars.ipynb```, ```app.py```, ```scraping.py``` and ```index.html```. 
- Data Tools: Jupyter Lab, Python and MongoDB
- Software: MongoDB, Python 3.9.7, Visual Studio Code 1.69.0, Flask Version 2.0.1

## Deliverable 1
### __Scrape Full Resolution Mars Hemisphere Images and Titles__

__Identifying Images of Mars and Dev Tools to inspect for full scale images of Mars.__

![Identifying_Image.png](https://github.com/RyanJL18/Mission-to-Mars/blob/main/Resources/Identifying_Image.png)

```
# 1. Use browser to visit the URL 
url = 'https://astrogeology.usgs.gov/search/results?q=hemisphere+enhanced&k1=target&v1=Mars'
browser.visit(url)

# 2. Create a list to hold the images and titles.
hemisphere_image_urls = []
```
```
# 3. Write code to retrieve the image urls and titles for each hemisphere.
for i in range(4):
    #create empty dictionary
    hemispheres = {}
    browser.find_by_css('a.product-item h3')[i].click()
    element = browser.find_link_by_text('Sample').first
    img_url = element['href']
    title = browser.find_by_css("h2.title").text
    hemispheres["img_url"] = img_url
    hemispheres["title"] = title
    hemisphere_image_urls.append(hemispheres)
    browser.back()

# 4. Print the list that holds the dictionary of each image url and title.
hemisphere_image_urls

[{'img_url': 'https://astropedia.astrogeology.usgs.gov/download/Mars/Viking/cerberus_enhanced.tif/full.jpg',
  'title': 'Cerberus Hemisphere Enhanced'},
 {'img_url': 'https://astropedia.astrogeology.usgs.gov/download/Mars/Viking/schiaparelli_enhanced.tif/full.jpg',
  'title': 'Schiaparelli Hemisphere Enhanced'},
 {'img_url': 'https://astropedia.astrogeology.usgs.gov/download/Mars/Viking/syrtis_major_enhanced.tif/full.jpg',
  'title': 'Syrtis Major Hemisphere Enhanced'},
 {'img_url': 'https://astropedia.astrogeology.usgs.gov/download/Mars/Viking/valles_marineris_enhanced.tif/full.jpg',
  'title': 'Valles Marineris Hemisphere Enhanced'}]
```

Using the code above, four images were scraped for use in the Web App below:

![Webapp_Mars_Images_Code.png](https://github.com/RyanJL18/Mission-to-Mars/blob/main/Resources/D1_Image_scrape.png)

![Webapp_Mars_Images.png](https://github.com/RyanJL18/Mission-to-Mars/blob/main/Resources/Screenshot%202022-05-22%20175513.png)

## Deliverable 2
### __Update the Web App with Mars’s Hemisphere Images and Titles__ ###

__Full code in ```Scraping.py```__

1. Using the ```Scraping.py``` file, the images and titles of the Mars images are scraped.

2. Using ```App.py``` the Mongo database is updated to include the full resolution images and titles.

### Code ###
```
from flask import Flask, render_template, redirect, url_for
from flask_pymongo import PyMongo
import scraping

app = Flask(__name__)

# Use flask_pymongo to set up mongo connection
app.config["MONGO_URI"] = "mongodb://localhost:27017/mars_app"
mongo = PyMongo(app)

@app.route("/")
def index():
    mars = mongo.db.mars.find_one()
    return render_template("index.html", mars=mars)

@app.route("/scrape")
def scrape():
   mars = mongo.db.mars
   mars_data = scraping.scrape_all()
   mars.update_one({}, {"$set":mars_data}, upsert=True)
   return redirect('/', code=302)

if __name__ == "__main__":
   app.run(port = 5000, debug =True)
```
3. ```Index.html``` file is updated to display the full resolution image and title.

## Deliverable 3
### __Add aditional Bootstrap Components__
```
 <body>
    <div class="container", style="background-color:sandybrown">
      <!-- Add Jumbotron to Header -->
      <div class="jumbotron text-center" style="background-image: url('https://wallpapercave.com/wp/wp7720219.jpg'); background-position: bottom; background-size: cover">
        <h1 style="color:whitesmoke" > Mission to Mars</h1>
        <!-- Add a button to activate scraping script -->
        <p><a style="color:whitesmoke; background-color: darkred; border:coral ;" class="btn btn-primary btn-lg" href="/scrape" role="button">Scrape New Data</a></p>
      </div>
```

![Mars_Webapp_Page.png](https://github.com/RyanJL18/Mission-to-Mars/blob/main/Resources/Screenshot%202022-05-22%20175420.png)

## Summary

This project detailed the process of scraping and creating a Webapp for the purposes of displaying Mars data and images. This project was customized using ```HTML``` code with ```Bootstrap 3``` details. Specific details like color, shape, size, font, etc. were used to customize this project and display the image in a fun and unique webapp.

##Created by Ryan J. Lynch##
