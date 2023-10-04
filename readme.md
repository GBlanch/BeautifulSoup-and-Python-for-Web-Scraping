# BeautifulSoup and Python for Web Scraping

## Table of contents

+ [Introduction](#introduction)
+ [Before deploying any code](#before-deploying-any-code)
+ [Good to code](#good-to-code)
+ [Inspecting the HTML page and finding patterns](#inspecting-the-html-page-and-finding-patterns)

## Introduction

The purpose of this repository is to showcase a basic web scraping case and their best practices.


## Before deploying any code

Prior to retrieving any data from any website, we must know whether or not that website allows to do so. Under the `terms and conditions of service` of the website/service of most websites, it is normally specified whether they allow web scraping or not. 

Another way to acknowledge the legitimacy of our web scaping practices is to check whether or not the web directory we’re trying to access is permitted to be scraped. We can check this by adding `robots.txt` at the end of the root directory of their website. 
For instance, in this case it is:

![image](https://github.com/GBlanch/BeautifulSoup-and-Python-for-Web-Scraping/assets/136500426/ac71c1bb-ce67-4713-bd88-a16a01916461)

And if we check `eBay`'s `robots.txt` file, we can verify that the access to this directory is allowed, as shown below:

![image](https://github.com/GBlanch/BeautifulSoup-and-Python-for-Web-Scraping/assets/136500426/3a6c0cbf-9f1d-4323-ad33-5e0031b7e3d9)


Last but not least, and since there are some server and performance costs for the website owners every time their data gets fetched, it is a nice and considered practice to avoid sending a big amount of URL requests.

There's much more information about Web Scaping that can be found in the  website [ZenRows Blog - Web Scraping Best Practices and Tools 2023](https://www.zenrows.com/blog/web-scraping-best-practices#respect-robots-txt-sitemap)


## Good to code

As usual, we first import the needed libraries we will be needing. 

These happen to be: `BeautifulSoup` so that we can interact and handle the data within the `html file`, and `urllib.request` in order to get this `html file` into a `Python` variable named `client`. And not unexpectedly, we also will need `Pandas` to create and manipulate our Dataframes as a matter of course.

And so we also proceed by defining the variable for the `html file` by passing its URL address into a Python variable `url`. This is to code:

![image](https://github.com/GBlanch/BeautifulSoup-and-Python-for-Web-Scraping/assets/136500426/951df0c8-ef6e-4315-91fd-a5e8f3ea6cd4)

Please notice that we can also assign a variable `key_word_search` to replace anytime we need the content of key word `nkw` on the URL address we have just passed. We have placed some curly brackets `{}` in its place. This way the target of our scraping script shall be modified without having to access to any web browser, or even if we just decide to automate other processes.

Next we load the `html file` into memory with the function `uReq` from the library `urllib.request`. For that we will define the variable `client` which will conatin the http client object. We also can have a look at the content of this object, as follows below :

![image](https://github.com/GBlanch/BeautifulSoup-and-Python-for-Web-Scraping/assets/136500426/35c9b713-cd09-4cba-8bfa-c1689472e055)


As you may certainly know, this output (html page) is rather large and not easy to read, and here's where `BeautifulSoup` will come extremely handy. Once the variable `html_page` is finally read/assigned as a html file, we will also close the client object:

![image](https://github.com/GBlanch/BeautifulSoup-and-Python-for-Web-Scraping/assets/136500426/f11969e2-75e5-412d-a4d7-21f31b6e83e5)



And so we have our data ready to be parsed with `BeautifulSoup`. We create a new variable `page_soup` and we will assign to it the last variable `html_page` which contais the html file as explained before. This results as follows: 

![image](https://github.com/GBlanch/BeautifulSoup-and-Python-for-Web-Scraping/assets/136500426/51cf6a9a-764c-47af-84e1-81792ba8b26e)

Now that we have our soup object ready to be manipulated, we are finally ready to move onto the next stage.

## Inspecting the HTML page and finding patterns

We open `Google Inspect` (Ctrl + Shift + C) to start knowing more about how the html page is built.
When highlighting and inspecting any element of the website, i.e. _USD price_ for a _Hiking Pocket Compass_, we can see the `tag` and `class` that define each of them :

![image](https://github.com/GBlanch/BeautifulSoup-and-Python-for-Web-Scraping/assets/136500426/2b3d8574-5efe-4b2e-a501-81c09d8a6d4d)

And so we can identify that for _this item_, its `tag` and `class` are `span` and `s-item__price` respectively. 
These define the price in the HTML code, and so we can insert them as arguments into the `findAll()` function to track the price down. We can fetch therefore the values as a whole list - whose type is `bs4.ResultSet`- , as an interval, or even individually. This is to code:

![image](https://github.com/GBlanch/BeautifulSoup-and-Python-for-Web-Scraping/assets/136500426/98911b97-407b-4828-a092-481a89e544e2)


We can also even isolate only the price value with the currency sign of a single observation as shown below with the `BeautifulSoup` method `.text`. Also, looking into the type of a single element of the list, we confirm they are strings:


![image](https://github.com/GBlanch/BeautifulSoup-and-Python-for-Web-Scraping/assets/136500426/1310a4ab-b566-4ad1-95a8-288b4db3d2fc)


But we want to apply theses filters and the rest of the cleaning procedures to the entire findings, not just to one observation. Therefore, we will store the whole list in a variable named `span_tag` so that we can perform the same operations to them all. 

To do this, we will declare the an empty list `prices` and we will run it into a for-loop. In this iteration, we will append the whole value of each `span_tag` string, excep for their 1st possition. All this is:

![image](https://github.com/GBlanch/BeautifulSoup-and-Python-for-Web-Scraping/assets/136500426/9dca4fc1-5682-4b20-ae27-bb60ab67f4fb)


Please notice the omission  of the 1st position -`$` symbol- of each string in the for-loop by means of the indexing `[1:]`. We could have used the `.strip()` method to achieve this end as well. 

Moving foward, and as we saw before, the type of each element within the list is still a string, therefore we ought to cast them into a numeric value. We will utilize the function `to_numeric` to attain this:


![image](https://github.com/GBlanch/BeautifulSoup-and-Python-for-Web-Scraping/assets/136500426/234fae45-b34d-4abd-b7a7-0daefa0dc06b)


And so Python complains about not being able to parse the non-numerical values as shown in the Console output. That is why we are going to utilize the argument `coerce` within the function `to_numeric` from `Pandas`, as the script reads below:

![image](https://github.com/GBlanch/BeautifulSoup-and-Python-for-Web-Scraping/assets/136500426/f4085677-3651-4d2d-8a0a-8d0b4125fe79)


We next get rid of the `NaN` values. We confirm there are 3 amongst the original dataframe `df`, and after having declared a new dataframe `df1` , we will remove these and we confirm they don't exist anymore. This is to code:

![image](https://github.com/GBlanch/BeautifulSoup-and-Python-for-Web-Scraping/assets/136500426/e6fdebe5-3805-4499-81a0-2caf8e14941f)

Finally we can parse this dataframe `df1` into a `csv` file and store it in the local folder where this web scraping script is runing :

![image](https://github.com/GBlanch/BeautifulSoup-and-Python-for-Web-Scraping/assets/136500426/a7a346aa-7e7d-407e-bae7-628fd2d7eac7)

![image](https://github.com/GBlanch/BeautifulSoup-and-Python-for-Web-Scraping/assets/136500426/f61a9c5e-1354-48b8-963e-42ddcc15f611)







