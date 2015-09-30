
# reviewinsights
Amazon reviews downloader and parser
Amazon offers no more a simple public API to access its reviews. If ones wants to quickly build a dataset of Amazon reviews he/she has to download them using a web crawler on the HTML pages.

I wrote a Perl script that, given a first level domain (e.g., "com", "it") and list of IDs of Amazon products, automatically downloads, from the Amazon server that is dedicated to that domain, all and only the HTML pages that contain the reviews about that products.

Then I wrote another Perl script that, given a list of the downloaded HTML files, extract all the reviews contained in them, outputting for each review a record with the following information:

A counter of the extracted reviews so far (can be used as a unique ID for the dataset).
Date of the review in YYYYMMDD format (note: on non-English speaking domains this feature won’t work, edit the script to set the name of months in the desired language).
ID of the reviewed product.
Star rating assigned by the reviewer.
Count of "yes" helpfulness votes.
Count of total helpfulness votes ("yes"+"no").
Date of the review in human readable format (will be in the language used by the specified domain).
ID of the author of the review.
Title of the review
Content of the review
Example

Given the products with IDs, e.g., B0040JHVCC and B00004ZDB1, the reviews from the ".com" domain are downloaded with the command:

./downloadAmazonReviews.pl com B0040JHVCC B00004ZDB1
Reviews are automatically downloaded in the ./amazonreviews/com/B0040JHVCC and ./amazonreviews/com/B00004ZDB1 directory. The script automatically adapts a timeout between download requests in order to be polite with Amazon, and also retries failing downloads (503 errors) until every page is downloaded.

Then the reviews are extracted from the HTML file by issuing the command:

./extractAmazonReviews-DivLayout.pl ./amazonreviews/com/B0040JHVCC/ ./amazonreviews/com/B00004ZDB1/
The scripts outputs one review per line on the standard output, in a CSV format:

"0","20120118","B0040JHVCC","1.0","1","2","January 18, 2012","A1E5SQ7VA3I8OI","Not worth the price","I purchased the... (removed for brevity)  ...and definitely no."
"1","20120116","B0040JHVCC","5.0","0","0","January 16, 2012","A34FBZLFAU88UI","Compact version of the 7D, and neck and neck with D7000","This camera is... (removed for brevity) ...focus hunting issues."
"2",...
Redirection can be used to save the output to a file:

./extractAmazonReviews-DivLayout.pl ./amazonreviews/com/B0040JHVCC/ ./amazonreviews/com/B00004ZDB1/ > reviews.csv 
will save the CSV output into the "reviews.csv" file.

Note that there are two versions of the extract script: "extractAmazonReviews-DivLayout.pl" and "extractAmazonReviews-TableLayout.pl". The DivLayout version works on the last (at April 2015) Amazon layout which ditched tables for divs. This layout is currently (again, at April 2015) used on the ".com" domain. The other domains still use the table-based layout, for which the TableLayout version is designed.

Disclaimer

I provide you the tool to download the reviews, not the right to download them. You have to respect Amazon's rights on its own data. Do not release the data you download without Amazon's consent.

