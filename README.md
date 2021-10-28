# RaleighRedistricting2020
This repository contains files and information pertaining to the 2020 census and redistricting for Raleigh, NC
Note that much (if not all) of this information will be meaningful to people with technical experience with databases and programming. I provide it to provide at least some transparancy about what the data looks like and how it is analyzed to determine population by City Council District and by voting precinct. As time permits I will add to this description to hopefully provide further clarifications.

The file, census.zip, is a compressed file containing census.db which is a SQLite database. The database contains five files:

District
GeoHeader
Segment1
Segment2
Segment3

Census.db was created from the data files provided by the Census Bureau. These files are available at https://www.census.gov/programs-surveys/decennial-census/about/rdo/summary-files.html. After navigating to this page look for "Legacy Format Summary Files" which is a link to a directory where the files are organized into subfolders by state. Clicking on North Carolina will take you to another directory that contains a zip file that contains all the data files. It is from these files that the GeoHeader, Segment1, Segment2, and Segment3 tables were created in census.db.

GeoHeader, Segment1, Segment2, and Segment3 actually represent different sets of columns for one very large table. However, the Census Bureau wanted to keep each table to no more than 255 columns (yes, there are a lot of columns in this data). So, I imported the data as four different tables into census.db.

The fifth table, District, contains census blocks for each Raleigh City Council District. Each census block is associated with a GEOID and GEOCODE field. By joining District with GeoHeader on GEOCODE, it is possible to query the database to summarize the data by District or by voting precinct (referred to as a voting district - field VTD in the GeoHeader table).

To summarize the population by precinct you can use the following SQL:

SELECT District.district, geoheader.VTD, SUM(GEOHEADER.POP100) FROM DISTRICT INNER JOIN GEOHEADER ON DISTRICT.GEOCODE=GEOHEADER.GEOCODE group by district.district, geoheader.VTD

To summarize the population by district you can use this SQL:

select district, sum(pop100) from geoheader inner join district on DISTRICT.GEOCODE=GEOHEADER.GEOCODE group by district

Here are the results by district:

A:	85013, B:	98151, C:	94985, D:	94547, E:	94969

I will upload the populations by voting precinct into a separate file and will update this README file when it is available.

The Census Bureau provides an online course to explain the data. This course is available as several videos at the following website:
https://www.census.gov/data/academy/courses/2020-census-redistricting-data.html

A final word about redistricting. Raleigh's total population according to the 2020 Census is 467,665. Ideally, each City Council District will contain the same number of people. With five districts this comes out to be 93,533. Of course, in the real world, this will never happen. Thus, the rule is that each district cannot differ from the ideal by more than 5%. Five percent of 93,533 is 4677. Thus, each district's population should fall between 88,856 and 98,210.

Clearly, District A fails this test. Thus, District boundaries will have to be moved allowing District A to increase in population. It might seem that the easiest thing to do is to move people from District B to District A. And that is certainly a choice. However, it is also possible to change the boundaries of several districts to achieve balance.

And there is where the discussion begins. How should the boundaries be changed? What are the guiding principles for changing the boundaries? There is much to discuss.

