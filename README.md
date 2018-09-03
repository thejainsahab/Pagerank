# Pagerank
A simple python scraper combined with a program to rank all the links on a website according to the Pagerank algorithm.
Simple Python Search Spider and Page Ranker.

This is a set of programs that emulate some of the functions of a 
search engine.  They store their data in a SQLITE3 database named
'spider.sqlite'.  This file can be removed at any time to restart the
process.
This program crawls a web site and pulls a series of pages into the
database, recording the links between pages.

Ubuntu: rm spider.sqlite
Ubuntu: python3 spider.py

Win: del spider.sqlite
Win: py spider.py

Enter web url or enter: http://books.toscrape.com/
['http://books.toscrape.com/']
How many pages:2
1 http://books.toscrape.com (51294) 94
23 http://books.toscrape.com/catalogue/category/books/young-adult_21/index.html (50167) 95
How many pages:

In this sample run, we told it to crawl a website and retrieve two 
pages.  If you restart the program again and tell it to crawl more
pages, it will not re-crawl any pages already in the database.  Upon 
restart it goes to a random non-crawled page and starts there.  So 
each successive run of spider.py is additive.

Ubuntu: python3 spider.py 
Win: py spider.py

Restarting existing crawl.  Remove spider.sqlite to start a fresh crawl.
['http://books.toscrape.com']
How many pages:3
92 http://books.toscrape.com/catalogue/stars-above-the-lunar-chronicles-45_632/index.html (20988) 16
52 http://books.toscrape.com/catalogue/category/books/erotica_50/index.html (21248) 56
22 http://books.toscrape.com/catalogue/category/books/new-adult_20/index.html (28732) 66
How many pages:

You can have multiple starting points in the same database - 
within the program these are called "webs".   The spider
chooses randomly amongst all non-visited links across all
the webs.

If you want to dump the contents of the spider.sqlite file, you can 
run spdump.py as follows:

Ubuntu: python3 spdump.py 
Win: py spdump.py

(5, None, 1.0, 23, 'http://books.toscrape.com/catalogue/category/books/young-adult_21/index.html')
(4, None, 1.0, 22, 'http://books.toscrape.com/catalogue/category/books/new-adult_20/index.html')
(4, None, 1.0, 52, 'http://books.toscrape.com/catalogue/category/books/erotica_50/index.html')
(2, None, 1.0, 92, 'http://books.toscrape.com/catalogue/stars-above-the-lunar-chronicles-45_632/index.html')
4 rows.

This shows the number of incoming links, the old page rank, the new page
rank, the id of the page, and the url of the page.  The spdump.py program
only shows pages that have at least one incoming link to them.

Once you have a few pages in the database, you can run Page Rank on the
pages using the sprank.py program.  You simply tell it how many Page
Rank iterations to run.

Ubuntu: python3 sprank.py 
Win: py sprank.py 

How many iterations:2
1 0.6666666666666666
2 0.3333333333333334
[(1, 1.7763568394002506e-16), (23, 1.5), (92, 0.7777777777777779), (52, 1.3611111111111112), (22, 1.3611111111111112)]

You can dump the database again to see that page rank has been updated:

Ubuntu: python3 spdump.py 
Win: py spdump.py 

(5, 1.0, 1.5, 23, 'http://books.toscrape.com/catalogue/category/books/young-adult_21/index.html')
(4, 1.0, 1.3611111111111112, 22, 'http://books.toscrape.com/catalogue/category/books/new-adult_20/index.html')
(4, 1.0, 1.3611111111111112, 52, 'http://books.toscrape.com/catalogue/category/books/erotica_50/index.html')
(2, 1.0, 0.7777777777777779, 92, 'http://books.toscrape.com/catalogue/stars-above-the-lunar-chronicles-45_632/index.html')
4 rows.

You can run sprank.py as many times as you like and it will simply refine
the page rank the more times you run it.  You can even run sprank.py a few times
and then go spider a few more pages sith spider.py and then run sprank.py
to converge the page ranks.

If you want to restart the Page Rank calculations without re-spidering the 
web pages, you can use spreset.py

Ubuntu: python3 spreset.py 
Win: py spreset.py 

All pages set to a rank of 1.0

Ubuntu: python3 sprank.py 
Win: py sprank.py 

How many iterations:50
1 0.25555555555555576
2 0.18333333333333363
3 0.1342592592592595
4 0.09768518518518539
5 0.07121913580246939
6 0.05189043209876574
...
42 5.849915886591006e-07
43 4.2629129957116876e-07
44 3.1064424783622257e-07
45 2.2637067378372678e-07
46 1.649593782815373e-07
47 1.2020813473512247e-07
48 8.759729714036268e-08
49 6.383333763793076e-08
50 4.6516218445979974e-08
[(1, 1.7763568394002506e-16), (23, 1.8749999509794473), (92, 0.6250000224233312), (52, 1.2500000132986133), (22, 1.2500000132986133)]

For each iteration of the page rank algorithm it prints the average
change per page of the page rank.   The network initially is quite 
unbalanced and so the individual page ranks are changing wildly.
But in a few short iterations, the page rank converges.  You 
should run prank.py long enough that the page ranks converge.
