# Art analysis to import publications

This has been written in 2025.

The main objective is to retrieve some information about a publication
and then to store it in the *R3MOB* *SQL* database.

## Where to search - Academic Databases

There are a lot of databases around the world that store scientific publications.

Among themselves, there are for instance:

- ***www.science.gov***: a *U.S government* database that contains scientific
research from *U.S federal agencies*.

This one contains more than *200 million pages* of scientific information for free,
and is a gateway to over 2000 scientific websites.
However, it is centered on the *U.S*.

- ***https://csxstatic.ist.psu.edu/home*** (*CiteSeer*): centered on computer
and information science.

- ***https://ieeexplore.ieee.org/Xplore/home.jsp*** (*IEEE Xplore*): very famous,
however, it requires a subscription.

- ***https://www.springer.com*** (*Springer*): historical database, and also
requires a subscription.


Anyway, you can follow [__this link__](https://en.wikipedia.org/wiki/List_of_academic_databases_and_search_engines)
to get a full list of academic databases.


Among those databases, there are the one which are:

- **Not Free**: import a publication from it will be hard.

- **Free**: import a publication from it will be possible.


So the idea is to search for the publication in the right database.
The main idea is to find something that can make request to all those databases.

## How to search - Search Engine

The databases discussed above also contain a *search engine*.
From the user perspective, it is a *research bar*.

### Indexer and Search Library

The *Search library* is the *core* of the search engine. It contains all
the algorithmic stuff to be as efficient and accurate as possible.

For instance, let's talk about [__***Apache Lucene***__](https://lucene.apache.org/core/).
This is an *open source* *Java* library providing indexing and search features,
such as spellchecking, highlighting and advanced tokenization capabilities.

It is used by websites like *Twitter*, *Apple*, and even *Wikipedia*.

However, *Lucene* does not contain *crawling* and *HTML parsing* functionalities.
That's why it is not recommended to use it directly.

### Lucene-based Search Engines

Many services already extended *Lucene*'s capabilities of parsing, such as:

- [__***Apache Nutch***__](https://github.com/apache/nutch): a *web crawler*,
It provides *web crawling* and *HTML* parsing.
[__Wikipedia__](https://en.wikipedia.org/wiki/Web_crawler) says:

> Web crawler, sometimes called a spider or spiderbot and often shortened to crawler,
> is an Internet bot that systematically browses the World Wide Web and
> that is typically operated by search engines for the purpose of Web indexing.

For instance, the *GoogleBot* is *Google*'s generic web crawler that is
responsible for crawling sites that will show up on *Google*'s search engine,
[__***Kinsta.com***__](https://kinsta.com/blog/crawler-list/) says.

Another example could be **citeseerxbot**, the crawler of ***CiteSeerX***,
discussed above.

- [__***Apache Solr***__](https://solr.apache.org/): an 2004 *enterprise-search*
platform. It can also *crawl* some web content as well as public databases.
This is exactly what it is needed to search for publications.

It is used by [__***Apache Hadoop***__](https://hadoop.apache.org/) which is
a very famous framework for *distributed computing*.

- [__***ElasticSearch***__](https://www.elastic.co/elasticsearch): also an
*enterprise-search* platform, but this one has been released in 2010.
It is currently the most popular *search engine*, even if it seems that
it is only working using *json* format.

### Differences between Solr and ElasticSearch

Both **ElasticSearch** and **Apache Solr** are very powerful.
However, the *ElasticSearch* *API* is only working using *json* format.

*ElasticSearch* is actually replacing every *Solr*-based app thanks to its
ability to integrate *NLP* (Natural Language Processing) and, for instance,
**BERT**.

Here is [__***a more detailed explanation***__](https://www.capellasolutions.com/blog/solr-vs-elasticsearch-which-search-engine-is-right-for-you-lets-compare).

## Which tools are designed to search specifically publications

The *search engines* discussed above are already great to use.
They give access to a *REST Api*, with nice documentation.
However, they are too generic, they are not configured for especially
*scientific publications*.

That's why I'll introduce here mostly two services that are based on a
*search engine* and that can be used to search for publications among public
databases.

1. [__***Crossref***__](https://api.crossref.org/swagger-ui/index.html):
A *RESTFUL Api* that has been recently updated to use *ElasticSearch* instead
of *Solr*. That's why some of the content is a little bit deprecated, but
it seems to be the best option for this project.

It is specialized on *research objects*. You can [__try it here__](https://www.crossref.org/).

It is so impressive because *ElasticSearch* is based on a 
*Peer 2 Peer* architecture. *Crossref* gathers more than *22,000* members
across the world, with nearly *2 billion monthly API queries*.

One difficulty could be to *construct the DOIs* of a broken publication if
the user wants to import a publication with, actually, a broken *DOI*.
Indeed, the *DOIs* (example: *10.1037/0003-066x.59.1.29*) could break a link,
because of their format. So this type of error will be hard to solve, a publication
may not be retrieved if the user gives a *DOI*.

2. [__***Semantic Scholar***__](ttps://api.semanticscholar.org/api-docs): A
*REST Api*. Its corpus gathers *214 Million* papers and more than *79 Million*
authors.

It is possible to get an *Api key* [__right here__](https://www.semanticscholar.org/product/api).
Without an *Api key*, the limitation is set to *100* requests every 5minutes.
However, *Semantic Scholar* does not *crawl/index* for material that is behind a
*paywall*, [__wikipedia__](https://en.wikipedia.org/wiki/Semantic_Scholar) says.

This one is very powerful because it is natively based on *NLP*. The main
advantadge compared to *Crossref* is its capability to analyse the content.

You can try it [__right here__](https://www.semanticscholar.org/).

## My plan to import publications on the website

There are some publications that are not available freely on the internet.
One may take the example of *IEEE Xplore* database, even the *abstract* is
private.
To then classify the publication rightfully, the abstract is at least needed.

The user may want to import a publication from *IEEE* database, or may not.
That's why the user needs to be able to import the publication using a file.

### Import using a file

The idea is to build a *Javascript* module that will:

1. parse the given file, in *json* format, because the former dev on the site
chose *json* as main file type, and because *ElasticSearch*, so *Crossref*,
only gives *json* files.


The different file formats could all be parsed in *json* using external
modules such as for instance:

- [__Bibtex JS__](https://github.com/digitalheir/bibtex-js) to parse *Bibtex*
files.

- [__XML JS__](https://github.com/nashwaan/xml-js) to parse *XML* files.

- etc..


2. put the data in this *json* file, into the *MySQL* database, using, as the
former dev used before me, [__Sequelize__](https://sequelize.org/).


I assume that all this part of parsing should be done on the *client side*.

### Import using a search engine

The idea is to use the *Crossref* *API*, to retrieve
*json* files and then put it in the *MySQL* database using *Sequelize*.

I assume that all this part of parsing should be done on the *client side*,
to retrieve the publication.

However, when the publication is retrieved, one important thing may be to also
find the authors. So here the *Semantic Scholar* *API* could be used to
retrieve data about the authors because this *API* has access to more than
*79 Million* authors.

I assume that all this searching part should be done on the *server side*.

Besides, it could be possible to retrieve very cool analytics from
*Semantic Scholar* *API*, such as **themes**,
and even **AI-generated topics and subthemes**.

## How to use the Crossref tool

There are multiple implementations that simplify the use of *Crossref API*.
Here are some examples from [***this website***](https://github.com/CrossRef/rest-api-doc/tree/master):

1. [***Crossref Commons***](https://gitlab.com/crossref/crossref_commons_py):
it is an official client from *Crossref*, written in *Python*.
However, it seems designed only for the use of *DOI*s.

2. [***ScienceAI Crossref***](https://github.com/scienceai/crossref):
it is a *javascript* client. However, it seems very old, and is not official.

3. [***habanero***](https://github.com/sckott/habanero): it is not an official
client, however it is maintained by something like 12 contributors.
The repository is well documented, with a lot of stars.
It is written in *Python*.

4. [***crossrefapi***](https://github.com/fabiobatalha/crossrefapi):
it is not an official client, however it is maintained by 14 contributors.
The repository is not as documented as the previous one.
Besides, it seems to be the choice of the community.


The *license* of *crossrefapi* is a *BSD* license. Apparently, this license
is not compatible with every other licenses, which can create some conflicts
with other licenses. However, it seems that it can protect developpers, giving
them some legal protections.

The *license* of *habanero* is a classic *MIT* license, it is compatible
with the *GNU general public license*, which is one of the most widely used
open source licenses, [***this article***](https://thisvsthat.io/bsd-license-vs-mit-license) says.

That's why I think I'll choose *habanero* for the *crossref API* client.

## How to use the Semantic Scholar tool

There are not a lot a *SemanticScholar* clients. This one
[***right here***](https://semanticscholar.readthedocs.io/en/stable/),
called *semanticscholarapi*, with around *400 stars*, is using a *MIT License*.
Here is the doc for it
[***right here***](https://semanticscholar.readthedocs.io/en/stable/).

There are so many possibilities with *SemanticScholar*. It is possible
to generate analytics with
[***Apache Spark***](https://spark.apache.org/docs/latest/api/python/index.html)
that is a processing engine, explained
[***right here***](https://www.semanticscholar.org/product/api/tutorial).

Anyway, what I want from *Semantic Scholar* is to give me recommandations,
number of citations, and keywords from a given publication.

*Semantic Scholar* is based on 3 different *API*s:

1. [***Datasets API***](https://api.semanticscholar.org/api-docs/datasets)
released in *2022*. It contains *S2AG datasets* with some publications that 
could contain full parsed text from the publication *PDF*, paper embeddings
and author attributes,
[***this article***](https://medium.com/ai2-blog/new-academic-graph-datasets-released-from-semantic-scholar-18b6b3b3140e)
says.

Anyway, this one is not implemented by *semanticscholarapi*.

2. [***Academic Graph API***](https://api.semanticscholar.org/api-docs/graph).
It can be used to retrieved some other metadata on a given publication and
on authors.

3. [***Recommandations API***](https://api.semanticscholar.org/api-docs/recommendations).
This one gives recommanded papers from a simple positive example paper.


The **recommendations are very great**, it can sometimes display a list
of more than 100 recommendations with all the raw data possible, including
4 different publication Ids as *DOI*, **paperId**, etc.. but not *ORCID*! (sad)

*SemanticScholar* is also **better at finding references and citations**.
Indeed, *Crossref* *facets* are hard to use, here with *SemanticScholar*
I just need to precise the **paperId**.

Let's talk about the **paperId**. This is wrong!
Sometimes, I can't retrieve a publication because the *DOI* is not found.
Indeed, the datasets (**Academic Graph** and **Recommandations API**) are
using **their own paperId** to identify a research paper. And sometimes, with
the right *paperId*, I can't even find the publication. It's like, buggy.


Anyway, the limit of **100 queries per 5minutes** is also tough. I think
**SemanticScholar is only useful to get recommendations, and citation papers**.
I will definitly use it to find recursively as many publications as possible
to create the labellised dataset, but that's all, **SemanticScholar has
paywalls too**.

## How to use the ORCID tool

We talked about finding metadata on a publication.
**It could also be interesting to find metadata on an author**

This is exactly what *ORCID* is doing, [***right here***](https://orcid.org/).

There are 2 ways of using the *ORCID API*:

- "public": when u have no subscription, u will be bloqued by *paywalls*.

- "member": when u have a subscription to *ORCID*, all is accessible for you.

The big difference with other *API*s is that *ORCID* requires an
authentication even for the *public* way. It is needed to get a **client-id**
and a **client-secret**.

I don't want to authenticate myself. It will ask a mail and I don't know which
mail provide.
That's why I will use their so called
[***OrcidScrapper***](https://pypi.org/project/PyOrcid/#access-through-orcidscrapper-feature-of-pyorcid)
that does not require any authentication. It's an alternative to *ORCID API*
that just gives access to the *public* features. **OrcidScrapper can access
all methods of Orcid class as it is inherited from it.**

To get the metadata about an author, it is needed to find the *orcid-id*
related to the author.

I assume that *Crossref* and *SemanticScholar* could help finding this
*orcid-id*. It is possible to seach using *Orcid* giving a name and
a surname however it will require an *ORCID account* to make the search.

The limit for this **OrcidScrapper** *API* is:

- 12 req/sec

- 25k reads/day (IP address)

[***this article***](https://info.orcid.org/ufaqs/what-are-the-api-limits/)
says.


__Edit__: Even for the *Public API*, a "laisser-passer" is required,
[***this article***](https://orcid-france.fr/espace-api/en-pratique/)
says.

I think *ORCID* is not a good thing to use.

## How to use the IDREF tool

*ORCID* requires an *orcid-id* to find the author, which could not be found
using *Crossref*. So it could be useful to use *IDREF*, which is another
*API* specialized on authors, to get the *orcid-id* of the author.

*IDREF* for **Identifiers and Repositories for Higher Education and Research**
is a public interface for consulting records produced by member of the
**French higher education and research documentary networks (Sudoc,
Calamec, Star)**,
[***this article***](https://apropos.cairn.info/en/questions-frequentes/a-propos-didref)
says.

For now, *IDREF* is based on *Solr*,
[***this article***](https://documentation.abes.fr/aideidrefdeveloppeur/index.html#UtiliserApiSolr)
says. So yeah, it seems to be quite old.

It is hard to find an *IDREF* client. This is an *API* that is not used very
often.

By default, the response is in *XML*. To get *JSON*, it is needed to add
`&wt=json` in the *url*.

I can't find the *request limit rate*.

*Abes* (=*IDREF*),  has been created in *2019*,
[***this article***](https://abes.fr/reseaux-idref-orcid/le-reseau/)
says:

> Pour valoriser le corpus des chercheurs, l’identifiant ORCID (Open Researcher and Contributor ID)
> est devenu la référence depuis plusieurs années. Depuis fin 2019, en relation avec le consortium
> COUPERIN, l’Abes est co-pilote du consortium ORCID France, regroupant les établissements français
> désireux d’adhérer aux services payants d’ORCID.

I don't want to use this *API* because there are no clients, the website of
*Abes* is so old, it seems very vulnerable, and I can't find a *rate limit*.
*Idref* has been created in *2019*, but the website seems to be from *2002*...
Go next.

## How to use the SCOPUS tool

"Scopus is the largest abstract and citations of peer-reviewing literature",
[***this article***](https://service.elsevier.com/app/answers/detail/a_id/15100/supporthub/scopus/)
says.

It could solve the issue of *not finding the abstract* with *Crossref*?
Anyway, I just need *Scopus* to give me a correlation between an author
and his *ORCID* id.

*Scopus* has its own *ID* for each authors because there are sometimes
authors with the same name,
[***this article***](https://service.elsevier.com/app/answers/detail/a_id/11212/c/10546/supporthub/scopus/)
says.

There is also this tool called **Scopus AI** that can generate summaries
on an abstract however it is "an add-on subscription".
[***this article***](https://service.elsevier.com/app/answers/detail/a_id/36844/c/18457/supporthub/scopus/)
says.

As other *API*'s, there are 2 ways to use it:

- *Non Commercial Users*: everything is accessible except *SciVal*
and *Embase* *API*'s.

- *Commercial Users*: pay to win mode.

There are actually a lot of accessible data,
[***this article***](https://dev.elsevier.com/)
says.

Unfortunately, it will be necessary to ask for an *API* key.

The documentation encourages us to use this *Python* client, called **elapsy**,
[***right here***](https://github.com/ElsevierDev/elsapy).
It has a *BSD-3* License, and a lot of stars. However, it seems to be
a little bit deprecated.

For the quotas, this website 
[***right here***](https://service.elsevier.com/app/answers/detail/a_id/34322/supporthub/dataasaservice/p/17729/)
says that it depends on the service used.
The reset is every seven days.

The *README* says to use *Pandas* on *Python 3.6*, however this example
[***right here***](https://github.com/ElsevierDev/elsapy/blob/master/exampleProg.py)
is not talking about it. I just hope not to get a version issue.

__Edit__: this client is deprecated, don't use it.


I found the python client *API* called **pybliometrics**
[***right here***](https://github.com/pybliometrics-dev/pybliometrics)
which is doing the same thing as *Elapsy* and *Elsevier*, but this one
is not deprecated, and currently has *33 contributors*.
Moreover, it has an *MIT* license and more stars than *Elapsy*.
The documentation is
[***right here***](https://pybliometrics.readthedocs.io/en/stable/)
.

this doc
[***right here***](https://my.bioinfo.guru/posts/g_scholar/g_scholar)
explains how to use *scopus*, it requires a specific configuration file
and an *API* key. The official documentation is actually very bad.


__Edit__: *Scopus* is **shit**. It needs an *API key* linked to an
organization.

### Comparison of the efficiency of Crossref against others

This article
[***right here***](https://link.springer.com/article/10.1007/s11192-024-05073-5)
compares how *World of Science*, *Scopus* are better
than *Crossref*.

### Final words

There are *paywalls* and *copyrights* everywhere.
As a developer it is impossible to get the data I need.

This article
[***right here***](https://www.servicescape.com/blog/open-access-vs-paywalls-new-paradigms-in-academic-publishing)
says that **Academia Edu** and **ResearchGate** could bypass the paywalls.
I will try to explore those paths.


For instance, **ResearchGate** does not have an *API*, and even its
*term of services* are explicitely clear about *scrapping* and *crawling*,
[***this article***](https://www.researchgate.net/post/API_on_ResearchGatenet)
says.


**Academia Edu** does not seem to have an *API* either.

## Edit July, 08 2025: OPENALEX

The goal of finding the author affiliations and the abstracts can be solved
with just one simple tool, released in 2022.

The main issues are the *paywalls* and the *copyrights*.
It is possible to bypass them by using *ORCID* or *SCOPUS* but
they require an *API Key* related to an *Institution*, which
I could ask for but I don't want to.

Released in 2022, [***OpenAlex***](https://openalex.org/) is a bibliographic
catalogue of scientific papers, authors and institutions accessible in
**open access** mode, named after the **Library of Alexandria**,
[***wikipedia***](https://en.wikipedia.org/wiki/OpenAlex) says.

The company behind it is [***Our Research***](https://ourresearch.org/team)
from *Canada*. It is the same company behind **Unpaywall**,
which is a browser extension that finds legal free versions of scholarly
articles (around 49 million free articles in 2024). *Unpaywall* has been
integrated to *World of Science* and *Scopus* in 2017,
[***wikipedia***](https://en.wikipedia.org/wiki/OurResearch) says.

The big asset of *OpenAlex* is that **it is a non for profit organization**,
providing a fully access **based on a Creative Commons Zero license**,
[***this article***](https://help.openalex.org/hc/en-us/articles/27190301279127-What-content-does-OpenAlex-include-and-how-does-it-compare-to-other-databases)
says.
Remember
[**Aaron Swartz**](https://creativecommons.org/2013/01/12/remembering-aaron-swartz/).

According to [***Frédérique Bordignon***](https://carnetist.hypotheses.org/2182),
*OpenAlex* covered in 2023 a huge amount of research papers,
a lot more than *World of Science* and *Scopus*, but less than *Google Scholar*.
This article also provides a criticism about the authenticity of the metadata
provided by *OpenAlex*, mostly on the author affiliations which are resulting
of bad translations to english, or a bad management of acronyms, or merely
wrong inputs by the authors themselves.

Indeed, *OpenAlex* is still young, here is some graphs on the proportion
of publications that have been retracted, comparing to other databases,
[***right here***](https://link.springer.com/article/10.1007/s11192-024-05034-y).
*OpenAlex* has "an important coverage documents without DOI", but it is still
less than *The Lens* or *World of Science*.
Besides, "OpenAlex collects more papers on its own than 
Dimensions and Scopus together."
This article strongly recommends to cross the results between multiple
databases to ensure their authenticity, it recommends the use of *Zotero*.

*OpenAlex* is based on *Crossref*, *ORCID*, making it available for free,
and gathering around **263 Million works** with more than **100,000
institutions** covered in 2025.

The *Sorbonne University* replaced *Web of Science* by *OpenAlex*
in 2023.
The *CNRS* may want to use *OpenAlex*, to replace *Scopus*, in the next
years, according to
[***Frédérique Bordignon***](https://carnetist.hypotheses.org/2182).
The *MESR* established a partnership with *OpenAlex* in 2024, providing
financial assistance.
The *French Ministry of Research and Higher Education* pledged to contribute
financially to the project, considering it "as a crucial open science
infrastructure", [***Wikipedia***](https://en.wikipedia.org/wiki/OpenAlex)
says.

As a developer, *OpenAlex* is the *Saint Graal*,
[***right here***](https://github.com/J535D165/pyalex).
The *Python* client for the *API* is well documented, with a lot of stars,
and has an *MIT License*. Besides, no authentication is needed!
It is possible to only use *OpenAlex* because it is internally using
*Crossref* and *ORCID*.

For instance, a strong feature of *OpenAlex* is its capability to provide
keywords on a publication abstract. Sometimes, abstracts are protected
by copyrights, and *Crossref* can't access to them. *OpenAlex* provides
*keywords* related to the *ngrams of the abstract*, storing not only the
word but also its position in the text, letting us to bypass the copyright
by reconstructing the abstract, called an *abstract on the fly*, which is
a rewritten version of the original abstract,
[***right here***](https://github.com/J535D165/pyalex#get-abstract).

Example right here:

```python
{
  'doi' : 'https://doi.org/10.1109/vtc2023-spring57618.2023.10199400',
  'title' :
      'DRL-Based RAT Selection in a Hybrid Vehicular Communication Network',
  'publication_date' : '2023-06-01',
  'primary_topic' : {
    'id' : 'https://openalex.org/T10761',
    'display_name' : 'Vehicular Ad Hoc Networks (VANETs)',
    'score' : 0.9998,
    'subfield' : {
      'id' : 'https://openalex.org/subfields/2208',
      'display_name' : 'Electrical and Electronic Engineering'
    },
    'field' : {
      'id' : 'https://openalex.org/fields/22',
      'display_name' : 'Engineering'
    },
    'domain' : {
      'id' : 'https://openalex.org/domains/3',
      'display_name' : 'Physical Sciences'
    }
  },
  'topics' : [
    {
      'id' : 'https://openalex.org/T10761',
      'display_name' : 'Vehicular Ad Hoc Networks (VANETs)',
      'score' : 0.9998,
      'subfield' : {
        'id' : 'https://openalex.org/subfields/2208',
        'display_name' : 'Electrical and Electronic Engineering'
      },
      'field' : {
        'id' : 'https://openalex.org/fields/22',
        'display_name' : 'Engineering'
      },
      'domain' : {
        'id' : 'https://openalex.org/domains/3',
        'display_name' : 'Physical Sciences'
      }
    },
    {
      'id' : 'https://openalex.org/T11099',
      'display_name' : 'Autonomous Vehicle Technology and Safety',
      'score' : 0.9788,
      'subfield' : {
        'id' : 'https://openalex.org/subfields/2203',
        'display_name' : 'Automotive Engineering'
      },
      'field' : {
        'id' : 'https://openalex.org/fields/22',
        'display_name' : 'Engineering'
      },
      'domain' : {
        'id' : 'https://openalex.org/domains/3',
        'display_name' : 'Physical Sciences'
      }
    },
    {
      'id' : 'https://openalex.org/T10524',
      'display_name' : 'Traffic control and management',
      'score' : 0.9633,
      'subfield' : {
        'id' : 'https://openalex.org/subfields/2207',
        'display_name' : 'Control and Systems Engineering'
      },
      'field' : {
        'id' : 'https://openalex.org/fields/22',
        'display_name' : 'Engineering'
      },
      'domain' : {
        'id' : 'https://openalex.org/domains/3',
        'display_name' : 'Physical Sciences'
      }
    }
  ],
  'keywords' : [],
  'concepts' : [
    {
      'id' : 'https://openalex.org/C41008148',
      'wikidata' : 'https://www.wikidata.org/wiki/Q21198',
      'display_name' : 'Computer science',
      'level' : 0,
      'score' : 0.7734215
    },
    {
      'id' : 'https://openalex.org/C43214815',
      'wikidata' : 'https://www.wikidata.org/wiki/Q7310987',
      'display_name' : 'Reliability (semiconductor)',
      'level' : 3,
      'score' : 0.5840638
    },
    {
      'id' : 'https://openalex.org/C48044578',
      'wikidata' : 'https://www.wikidata.org/wiki/Q727490',
      'display_name' : 'Scalability',
      'level' : 2,
      'score' : 0.5217869
    },
    {
      'id' : 'https://openalex.org/C97541855',
      'wikidata' : 'https://www.wikidata.org/wiki/Q830687',
      'display_name' : 'Reinforcement learning',
      'level' : 2,
      'score' : 0.5155418
    },
    {
      'id' : 'https://openalex.org/C120314980',
      'wikidata' : 'https://www.wikidata.org/wiki/Q180634',
      'display_name' : 'Distributed computing',
      'level' : 1,
      'score' : 0.49229825
    },
    {
      'id' : 'https://openalex.org/C31258907',
      'wikidata' : 'https://www.wikidata.org/wiki/Q1301371',
      'display_name' : 'Computer network',
      'level' : 1,
      'score' : 0.45947522
    },
    {
      'id' : 'https://openalex.org/C192448918',
      'wikidata' : 'https://www.wikidata.org/wiki/Q682677',
      'display_name' : 'Vehicular ad hoc network',
      'level' : 4,
      'score' : 0.45362726
    },
    {
      'id' : 'https://openalex.org/C158379750',
      'wikidata' : 'https://www.wikidata.org/wiki/Q214111',
      'display_name' : 'Network packet',
      'level' : 2,
      'score' : 0.44738722
    },
    {
      'id' : 'https://openalex.org/C47796450',
      'wikidata' : 'https://www.wikidata.org/wiki/Q508378',
      'display_name' : 'Intelligent transportation system',
      'level' : 2,
      'score' : 0.42497796
    },
    {
      'id' : 'https://openalex.org/C555944384',
      'wikidata' : 'https://www.wikidata.org/wiki/Q249',
      'display_name' : 'Wireless',
      'level' : 2,
      'score' : 0.2760596
    },
    {
      'id' : 'https://openalex.org/C127413603',
      'wikidata' : 'https://www.wikidata.org/wiki/Q11023',
      'display_name' : 'Engineering',
      'level' : 0,
      'score' : 0.19744387
    },
    {
      'id' : 'https://openalex.org/C94523657',
      'wikidata' : 'https://www.wikidata.org/wiki/Q4085781',
      'display_name' : 'Wireless ad hoc network',
      'level' : 3,
      'score' : 0.17525944
    },
    {
      'id' : 'https://openalex.org/C154945302',
      'wikidata' : 'https://www.wikidata.org/wiki/Q11660',
      'display_name' : 'Artificial intelligence',
      'level' : 1,
      'score' : 0.11538404
    },
    {
      'id' : 'https://openalex.org/C22212356',
      'wikidata' : 'https://www.wikidata.org/wiki/Q775325',
      'display_name' : 'Transport engineering',
      'level' : 1,
      'score' : 0.10022694
    },
    {
      'id' : 'https://openalex.org/C76155785',
      'wikidata' : 'https://www.wikidata.org/wiki/Q418',
      'display_name' : 'Telecommunications',
      'level' : 1,
      'score' : 0.08261272
    },
    {
      'id' : 'https://openalex.org/C163258240',
      'wikidata' : 'https://www.wikidata.org/wiki/Q25342',
      'display_name' : 'Power (physics)',
      'level' : 2,
      'score' : 0.0
    },
    {
      'id' : 'https://openalex.org/C121332964',
      'wikidata' : 'https://www.wikidata.org/wiki/Q413',
      'display_name' : 'Physics',
      'level' : 0,
      'score' : 0.0
    },
    {
      'id' : 'https://openalex.org/C62520636',
      'wikidata' : 'https://www.wikidata.org/wiki/Q944',
      'display_name' : 'Quantum mechanics',
      'level' : 1,
      'score' : 0.0
    },
    {
      'id' : 'https://openalex.org/C77088390',
      'wikidata' : 'https://www.wikidata.org/wiki/Q8513',
      'display_name' : 'Database',
      'level' : 1,
      'score' : 0.0
    }
  ],
  'sustainable_development_goals' : [ {
    'score' : 0.48,
    'display_name' : 'Decent work and economic growth',
    'id' : 'https://metadata.un.org/sdg/8'
  } ],
  'abstract_inverted_index' : {
    'Cooperative' : [0],
    'intelligent' : [1],
    'transport' : [2],
    'systems' : [3],
    'rely' : [4],
    'on' : [ 5, 33, 106 ],
    'a' : [ 6, 34, 100 ],
    'set' : [7],
    'of' : [ 8, 37, 84, 94, 176 ],
    'Vehicle-to-Everything' : [9],
    '(V2X)' : [10],
    'applications' : [ 11, 19, 31 ],
    'to' : [ 12, 90, 111, 145, 154, 157 ],
    'enhance' : [ 13, 146 ],
    'road' : [14],
    'safety.' : [15],
    'Emerging' : [16],
    'new' : [17],
    'V2X' : [ 18, 53 ],
    'like' : [20],
    'Advanced' : [21],
    'Driver' : [22],
    'Assistance' : [23],
    'Systems' : [24],
    '(ADASs)' : [25],
    'and' : [ 26, 40, 48, 59, 164 ],
    'Connected' : [27],
    'Autonomous' : [28],
    'Driving' : [29],
    '(CAD)' : [30],
    'depend' : [32],
    'significant' : [35],
    'amount' : [36],
    'shared' : [38],
    'data' : [39],
    'require' : [41],
    'high' : [ 42, 49, 131 ],
    'reliability,' : [43],
    'low' : [44],
    'end-to-end' : [45],
    '(E2E)' : [46],
    'latency,' : [47],
    'throughput.' : [50],
    'However,' : [51],
    'present' : [52],
    'communication' : [ 54, 78, 101, 140, 179 ],
    'technologies' : [55],
    'such' : [56],
    'as' : [57],
    'ITS-G5' : [58],
    'C-V2X' : [60],
    '(Cellular' : [61],
    'V2X)' : [62],
    'cannot' : [63],
    'satisfy' : [64],
    'these' : [ 65, 95 ],
    'requirements' : [66],
    'alone.' : [67],
    'In' : [68],
    'this' : [69],
    'paper,' : [70],
    'we' : [ 71, 98, 121 ],
    'propose' : [ 72, 99 ],
    'an' : [73],
    'intelligent,' : [74],
    'scalable' : [75],
    'hybrid' : [ 76, 138 ],
    'vehicular' : [ 77, 139 ],
    'architecture' : [ 79, 141 ],
    'that' : [ 80, 129, 136 ],
    'leverages' : [81],
    'the' : [ 82, 92, 113, 126, 137, 143, 147, 159, 165, 174, 177 ],
    'performance' : [83],
    'multiple' : [85],
    'Radio' : [86],
    'Access' : [87],
    'Technologies' : [88],
    '(RATs)' : [89],
    'meet' : [91],
    'needs' : [93],
    'applications.' : [96],
    'Then,' : [97],
    'mode' : [ 102, 180 ],
    'selection' : [ 103, 162, 169 ],
    'algorithm' : [104],
    'based' : [105],
    'Deep' : [107],
    'Reinforcement' : [108],
    'Learning' : [109],
    '(DRL)' : [110],
    'maximize' : [112],
    "network's" : [114],
    'reliability' : [115],
    'while' : [116],
    'limiting' : [117],
    'resource' : [ 118, 184 ],
    'consumption.' : [119],
    'Finally,' : [120],
    'assess' : [122],
    'our' : [123],
    'work' : [124],
    'using' : [125],
    'platooning' : [127],
    'scenario' : [128],
    'requires' : [130],
    'reliability.' : [132],
    'Numerical' : [133],
    'results' : [134],
    'reveal' : [135],
    'has' : [142],
    'potential' : [144],
    'packet' : [148],
    'reception' : [149],
    'rate' : [150],
    '(PRR)' : [151],
    'by' : [ 152, 181 ],
    'up' : [153],
    '30%' : [155],
    'compared' : [156],
    'both' : [158],
    'static' : [160],
    'RAT' : [161],
    'strategy' : [163],
    'multi-criteria' : [166],
    'decision-making' : [167],
    '(MCDM)' : [168],
    'algorithm.' : [170],
    'Additionally,' : [171],
    'it' : [172],
    'improves' : [173],
    'efficiency' : [175],
    'redundant' : [178],
    '20%' : [182],
    'regarding' : [183],
    'consumption' : [185]
  },
}
```

It also provides a wide range of topics (in the example above)
with the given score precision,
related to the publication itself, which is inspiring me for the
classification to use **an automated multi-tag hierarchical classifier**,
[***right here***](https://github.com/Tencent/NeuralNLP-NeuralClassifier),
or
[***right here***](https://github.com/RandolphVI/Hierarchical-Multi-Label-Text-Classification).

### EOF

