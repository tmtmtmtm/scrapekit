# scrapekit

Did you know the entire web was made of data? You probably did.
Scrapekit helps you get that data with simple Python scripts. Based on
[requests](http://docs.python-requests.org/), the library will handles
caching, threading and logging.

See the [full documentation](http://scrapekit.readthedocs.org/).

## Example

```python
from scrapekit import http, task
from lxml import html

@task
def get_index():
  res = http.get('http://databin.pudo.org/t/b2d9cf')
  doc = html.fromstring(res.content)
  for row in doc.findall('.//tr'):
    yield row

@task
def get_row(row):
  columns = row.findall('./td')
  print columns

pipeline = get_index | get_row
if __name__ == '__main__':
  pipeline.run()
  
```

## Works well with

Scrapekit doesn't aim to provide all functionality necessary for
scraping. Specifically, it doesn't address HTML parsing, data storage
and data validation. For these needs, check the following libraries:

* [lxml](http://lxml.de/) for HTML/XML parsing; much faster and more 
  flexible than [BeautifulSoup](http://www.crummy.com/software/BeautifulSoup/).
* [dataset](http://dataset.rtfd.org) is a sister library of scrapekit
  that simplifies storing semi-structured data in SQL databases.

## Why not Scrapy?

[Scrapy](http://scrapy.org/) is a much more mature and comprehensive
framework for developing scrapers. On the other hand, it requires you to
develop scrapers within its class system. This can be too heavyweight
for a simple script to grab data off a web site.

### Credits and license

Scrapekit is licensed under the terms of the MIT license, which is also
included in [LICENSE](LICENSE). It was developed through projects of
[ICFJ](http://icfj.org), [ANCIR](http://investigativecenters.org) and
[ICIJ](http://icij.org).
