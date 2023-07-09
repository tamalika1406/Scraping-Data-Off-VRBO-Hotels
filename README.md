# Scraping-Data-Off-VRBO-Hotels
Used BeautifulSoup &amp; Selenium to scrape 2500 vacation rentals in the United States &amp; MongoDB to store the data

## The Scraping Process: 
As part of the data design part of this project, firstly we shortlisted the top ten popular cities within the US
that are among the largest market place for VRBO, namely: San Francisco, Las Vegas, Los Angeles, New
York, Chicago, Boston, Miami, Orlando, Honolulu, and Washington DC. We scraped essential features, as
mentioned below, from all of the properties listed in these cities using relevant Python libraries
(BeautifulSoup, Selenium). Finally, we designed a database on MongoDB to store the data for driving
further business insights using Machine Learning. The next part of our project to analyze prices and
recommendations has been covered in the report for BAX 452.

● Standardize the data: In order to ensure our data is standardized for further analysis, we decided
to use a per-day rate of different properties for convenient comparison (1st April ‘23 to 2nd April
‘23). Additionally, we based our search results on a standard 2 adults, 0 children per room basis,
and also included a filter to exclude pets, as the inclusion of pets is a highly relative situation
based on the type of pet (big, small, noisy, non-noisy)

● Defining the main loop: The first essential loop in our code is a for loop, which first loops
through a list containing all the cities required, followed by 10 pages under each city which
comprises the property listings. Next, a sub for loop runs through a total of 50 properties on each
of the 10 pages. Finally, in case the loops fail to access any of the mentioned information, an
exception handling ensures the smooth running of the code

● Saving web pages to local: To prevent ourselves from either being blocked on the website for
scraping heavy loads of data or ensuring we are able to scrape details from a static web page, we
downloaded all properties across the 10 cities in our local system. Using selenium, we find the
appropriate class identifier to navigate to the web page of that property. Next, we account for one
city and one page at a time and make the selenium driver click on properties one-by-one and
download the HTML version of the property web page. In the end, we have a total of 2506
downloaded pages (out of which only 2503 seem to contain information, and 3 are blank pages
potentially due to some website error)

● Accessing property features: Here, we finally extract the different features under each property
that will be used in the analysis later. Using beautiful soup, we identify the necessary HTML tag
identifiers that will give us the targetted information, as mentioned below:
  ○ Rank: Describes the rank of the property listing under each city, based on its popularity
  amongst customers
  ○ Name: Describes the title of the property for identification
  ○ VRBO_City: Describes the city under which the property is located
  ○ VRBO_Text: Describes the details about the property as updated by the owner. This
  the section gives an overview of the uniqueness and offerings of the listing to a customer
  ○ VRBO_Type: Describes the type of property offered by VRBO namely, hotel, studio,
  guest house, apartment, house, building, hotel suite, condo, resort, cottage, townhome,
  yacht, villa, estate, recreational vehicle, bungalow, cabin, houseboat, hostel, corporate
  apartment, mobile home, and boat
  ○ Number_of_Bedrooms: Describes the number of bedrooms available in the property
  ○ Star Rating: Describes the rating of the property as rated by the visitors
  ○ VRBO_Near: Describes the top 6 famous tourist attractions or important landmarks
  around the property (typically within 0-3 miles range)
  ○ VRBO_Price: Describes the price of the property based on occupancy for 1 day, 2 adults,
  0 children, and 0 pets
  ○ VRBO_Number_Images: Describes the number of images posted by the owner of a
  property. More images would indicate more confidence from the owner, in showcasing
  the property to ensure transparency between the owner and the customer
  ○ VRBO_Area_SQ: Describes the area covered by the property in square feet
  ○ Number_Beds: Describes the number of beds available in the property
  ○ Number_Sleepers: Describes the sleeping capacity available in the property
  ○ Number_Bathrooms: Describes the number of bathrooms available on the property
  ○ Number_Baths: Describes the number of baths available on the property
  ○ Number_Reviews: Contains all the reviews as shared by visitors on the property
  ○ Reviews_Text: Contains a concatenated format of all the reviews for a particular
  property, separated by ‘|||’ to distinguish between two different reviews
  ○ Number_Amenities: Describes the number of amenities supported for each property,
  namely, microwave, fridge, room heater, hairdryer, etc.
  6
  UC Davis Graduate School of Management
  ○ Amenities_Text: Describes the type of amenities supported for each property, namely,
  microwave, fridge, room heater, hairdryer, etc.
  ○ Number_Facilities: Describes the type of facilities available namely, shower, sofa, dining
  table, etc.
  ○ Facilities_Text: Describes the number of facilities available namely, shower, sofa, dining
  table, etc.


● Collecting the Data: After extracting the aforementioned details using beautiful soup, we store
the details in a list of Python dictionaries, which can finally be pushed into the MongoDB
database

## Database Design:
We chose MongoDB over MySQL because of two important reasons, first, the ability of MongoDB to
scale horizontally and handle large amounts of data and if needed, distribute across multiple servers and
ensure fast processing. Second, MongoDB’s flexibility as a document-oriented database to store data in a
schema-less format. This allows for easy modification of data structures, without having to change the
underlying database and thus more adaptability to changing data requirements.
The different features for properties across 10 cities are firstly stored in the MongoDB database under the
collection called “vrbo”. However, on inspecting the data from the websites, we observed that a lot of
these data were either not in a business consumable format, or, present in lists and nested list elements, or,
included several noise characters in the texts present. To tackle this problem, we decided to perform some
cleaning operations using regex. We also ensured that in the case of nested information in fields such as
amenities, facilities, and reviews, we are able to extract such nested data to CSV formats hence we
un-nested the nested information and parsed the different texts using a pipe symbol “|||” so that on
exporting the files to CSV formats, the data is not distorted.

Therefore, the code first generates a collection called “vrbo” which contains the unformatted fields of
different features. We perform the data formatting exercises in Python for each field, in order to generate
updated formats consumable for business analysis and store the final version of the business-ready data in
a new collection called “vrbo_formatted”. The final database created contains 2503 rows and 21 fields
and this collection has the following breakdown of entries per city:
San Francisco (290), Las Vegas (316), Los Angeles (326), New York (280), Chicago (133), Boston (236),
Miami (183), Orlando (178), Honolulu (397), and Washington DC (167).
