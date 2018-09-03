# Pagerank
A simple python scraper combined with a program to rank all the links on a website according to the Pagerank algorithm.
Simple Python Search Spider and Page Ranker.
This program uses BeautifulSoup library. Make sure it is installed on your system before running it.

This is a set of programs that emulate some of the functions of a 
search engine. They store their data in a SQLITE3 database named
'spider.sqlite'. This file can be removed at any time to restart the
process.
This program crawls a web site and pulls a series of pages into the
database, recording the links between pages.

If you restart the program again and tell it to crawl more
pages, it will not re-crawl any pages already in the database. Upon 
restart it goes to a random non-crawled page and starts there. So 
each successive run of spider.py is additive.

You can have multiple starting points in the same database - 
within the program these are called "webs". The spider
chooses randomly amongst all non-visited links across all
the webs.

If you want to dump the contents of the spider.sqlite file, you can 
run spdump.py.

This shows the number of incoming links, the old page rank, the new page
rank, the id of the page, and the url of the page. The spdump.py program
only shows pages that have at least one incoming link to them.

Once you have a few pages in the database, you can run Page Rank on the
pages using the sprank.py program. You simply tell it how many Page
Rank iterations to run.

You can dump the database again to see that page rank has been updated.

You can run sprank.py as many times as you like and it will simply refine
the page rank the more times you run it. You can even run sprank.py a few times
and then go spider a few more pages sith spider.py and then run sprank.py
to converge the page ranks.

If you want to restart the Page Rank calculations without re-spidering the 
web pages, you can use spreset.py

For each iteration of the page rank algorithm it prints the average
change per page of the page rank. The network initially is quite 
unbalanced and so the individual page ranks are changing wildly.
But in a few short iterations, the page rank converges. You 
should run sprank.py long enough that the page ranks converge.
