# A fork of Snacktory

This is a fork of Snacktory with some functionality added, some removed and hopefully a lot more documentation of the methods. 

# Usage

Clone the repository and using maven either build the project into a JAR file or copy the source files and maven dependencies across into your project. 

 Now you can use it as follows:
 
 ```java
 // you'll need to set these two variables to values appropriate to your needs. :
 int minParagraphLength = 40;          // the minimum length of a string between <p> and </p> tags to keep.
 String htmlSource = "<html></html>";  // this should be a full html document,
                                       // you can also use the HtmlFetcher class to get a document for you. 
 
// Create a new ArticleTextExtractor from Snacktory
ArticleTextExtractor ate = new ArticleTextExtractor();

// Create an output formatter object for use in extracting the main article text.
OutputFormatter formatter = new OutputFormatter(minParagraphLength);

// We can set the JSoup selector for what we are looking for in the structure 
// of the <p> tags in the document we want to extract from.
// For instance skip any paragraphs containing <em> tags:
formatter.setNodesToKeepCssSelector("p:not(:has(em))");

// Run Snacktory's extract content method using out output formatter
JResult parsedDocument = ate.extractContent(row.getString("raw_data"), formatter);

// get the body content out of the result object
String bodyText = parsedDocument.getText();
```

# Project Overview

The original Snacktory repository is here: https://github.com/karussell/snacktory and the most of the rest of this Readme is from there. 

This is a small helper utility for people who don't want to write yet another java clone of Readability.
In most cases, this is applied to articles, although it should work for any website to find its major
area, extract its text, keywords, its main picture and more.

Snacktory borrows some ideas and a lot of test cases from [goose](https://github.com/GravityLabs/goose) 
and [jreadability](https://github.com/ifesdjeen/jReadability):

# License 

The software stands under Apache 2 License and comes with NO WARRANTY

# Features

 * article text detection 
 * get top image url(s)
 * get top video url
 * extraction of description, keywords, ...
 * good detection for none-english sites (German, Japanese, ...), snacktory does not depend on the word count in its text detection to support CJK languages 
 * good charset detection
 * possible to do URL resolving, but caching is still possible after resolving
 * skipping some known filetypes
 * no http GET required to run the core tests

TODOs

 * only top text supported at the moment


# General Usage

 If you need this for Android be sure you read [this issue](https://github.com/karussell/snacktory/issues/36).

 Now you can use it as follows:
 
 ```java
 HtmlFetcher fetcher = new HtmlFetcher();
 // set cache. e.g. take the map implementation from google collections:
 // fetcher.setCache(new MapMaker().concurrencyLevel(20).maximumSize(count).
 //    expireAfterWrite(minutes, TimeUnit.MINUTES).makeMap();

 JResult res = fetcher.fetchAndExtract(articleUrl, resolveTimeout, true);
 String text = res.getText(); 
 String title = res.getTitle(); 
 String imageUrl = res.getImageUrl();
```

# Build

via Maven. Maven will automatically resolve dependencies to jsoup, log4j and slf4j-api
