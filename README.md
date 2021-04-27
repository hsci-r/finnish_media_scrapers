# Finnish Media Scrapers

[![DOI](https://zenodo.org/badge/335605978.svg)](https://zenodo.org/badge/latestdoi/335605978)


Scrapers for extracting articles from Finnish journalistic media websites by the [University of Helsinki](https://www.helsinki.fi/) [Human Sciences – Computing Interaction research group](https://heldig.fi/hsci/). 

The general workflow for using the scapers is as follows:
 1. The scrapers support specifying a keyword as well as a timespan for extraction, and output a CSV of all matching articles with links. 
 2. A second set of scripts then allows downloading the matched articles in HTML format. 
 3. Third, there are further scripts for extracting plain text versions of the article texts out of the HTML.
 4. Finally, a script exists to post-filter the resulting plain texts again with keywords. 

Important to know when applying the workflow is that due to the fact that all the sources use some kind of stemming for their search, they can often return also spurious hits. Further, if searching for multiple words, the engines often perform a search for either word instead of the complete phrase. The post-filtering script above exists to counteract this by allowing the refiltering of the results more rigorously and uniformly locally.

At the same time and equally importantly, the stemming for a particular media may not cover e.g. all inflectional forms of words. Thus, it often makes sense to query for at least all common inflected variants and merge the results. For a complete worked up example of this kind of use, see the [members_of_parliament](https://github.com/hsci-r/finnish-media-scraper/tree/master/members_of_parliament) folder, which demonstrates how one can collect and count how many articles in each media mention the members of the Finnish Parliament.

Included are scrapers for [YLE](https://www.yle.fi/uutiset/), [Helsingin Sanomat](https://www.hs.fi/), [Iltalehti](https://www.iltalehti.fi/) and [Iltasanomat](https://www.is.fi/). See below for limitations relating to individual sources. 

## Installation

The easiest way to install the required dependencies for these scripts is through Conda. This can be done by running e.g. `conda env create -f environment.yml --prefix venv` and then `conda activate ./venv`.

## Helsingin Sanomat

First, query the articles you want using `query-hs.py`. For example, `python query-hs.py -f 2020-02-16 -t 2020-02-18 -o hs-sdp.csv -q SDP`.

For downloading articles, this scraper requires 1) a user id and password for Helsingin Sanomat and 2) a Selenium Docker container to be running. After installing Docker, run `docker run -p 4444:4444 -v /dev/shm:/dev/shm selenium/standalone-chrome:4.0.0-beta-1-20210215` in another console before invoking the script. After these prequisites are fulfilled, you can fetch the articles using `fetch-hs.py`. For example `python fetch-hs.py -i hs-sdp.csv -o hs-sdp`. After fetching the articles, extract texts with `python3 convert-hs-to-text.py -o hs-sdp-output hs-sdp`

Known special considerations:

- The search engine used seems to be employing some sort of stemming/lemmatization, so e.g. the query string `kok` seems to match `kokki`, `koko` and `koki`.
- A single query can return at most 9,950 hits. This can be sidestepped by invoking the script multiple times with smaller query time spans.

## Yle

Known special considerations:

- A single query can return at most 10,000 hits. This can be sidestepped by invoking the script multiple times with smaller query time spans.

example: `python query-yle.py -f 2020-02-16 -t 2020-02-18 -o yle-sdp.csv -q SDP` + `python fetch-open.py -i yle-sdp.csv -o yle-sdp` + `python3 convert-yle-to-text.py -o yle-sdp-output yle-sdp` 

## Iltalehti

example: `python query-il.py -f 2020-02-16 -t 2020-02-18 -o il-sdp.csv -q SDP` + `python fetch-open.py -i il-sdp.csv -o il-sdp` + `python3 convert-il-to-text.py -o il-sdp-output il-sdp`

## Iltasanomat

example: `python query-is.py -f 2020-02-16 -t 2020-02-18 -o is-sdp.csv -q SDP` + `python fetch-open.py -i is-sdp.csv -o is-sdp` + `python3 convert-is-to-text.py -o is-sdp-output is-sdp`

## Contact

For more information on the scrapers, please contact associate professor [Eetu Mäkelä](http://iki.fi/eetu.makela).
