# Pleiades Search API

Search *[Pleiades](https://pleiades.stoa.org)* from Python over-the-web. Get JSON results.

The `pleiades_search_api` package is written and maintained by [Tom Elliott](https://paregorios.org) for the [Institute for the Study of the Ancient World](https://isaw.nyu.edu).  
© Copyright 2022 by New York University  
Licensed under the AGPL-3.0; see LICENSE.txt file.

## Getting started

Use a python 3.10.2 environment and then run `pip install -r requirements_dev.txt`. Fire up the python interpreter and:

```python
>>> from pleiades_search_api.search import Query, SearchInterface
>>> q = Query()
>>> q.supported
['bbox', 'description', 'feature_type', 'tag', 'text', 'title']
q.set_parameter("title", "Zucchabar")
>>> si = SearchInterface()
Using default HTTP Request header for User-Agent = "pleiades_search_api/0.0.1 (+https://github.com/isawnyu/pleiades_search_api)". We strongly prefer you define your own unique user-agent string.
>>> results = si.search(q)
>>> results.keys()
dict_keys(['query', 'hits'])
>>> from pprint import pprint
>>> pprint(results["hits"], indent=4)
[   {   'id': '295374',
        'summary': 'Zucchabar was an ancient city of Mauretania Caesariensis '
                   'with Punic origins. The modern Algerian community of '
                   'Miliana lies atop and around the largely unexcavated '
                   'ancient site. Epigraphic evidence indicates that the Roman '
                   'emperor Augustus established a veteran colony there.',
        'title': 'Zucchabar',
        'uri': 'https://pleiades.stoa.org/places/295374'}]
>>> q.clear_parameters()
>>> q.set_parameter("text", ["Punic", "Phoenician"], "OR")
>>> results = si.search(q)
>>> len(results["hits"])
100
>>> q.set_parameter("bbox", (-4.0,33.0,2.0,40.0))
>>> results = si.search(q)
>>> len(results["hits"])
4
>>> pprint(results["hits"], indent=4)
[   {   'id': '482947334',
        'summary': 'A Phoenician settlement of the eighth and seventh '
                   'centuries B.C.',
        'title': 'Sa Caleta',
        'uri': 'https://pleiades.stoa.org/places/482947334'},
    {   'id': '266038',
        'summary': 'Sexi/Saxetanum (modern Almuñécar) was a Phoenician colony '
                   'established ca. 800 BC that eventually became a Roman one '
                   '(Sexi Firmum Iulium).',
        'title': 'Sexi/Saxetanum',
        'uri': 'https://pleiades.stoa.org/places/266038'},
    {   'id': '265954',
        'summary': 'Lucentum was a Phoenician and then Roman settlement in '
                   'Tarraconensis that was largely abandoned after the third '
                   'century.',
        'title': 'Lucentum',
        'uri': 'https://pleiades.stoa.org/places/265954'},
    {   'id': '265849',
        'summary': 'Carthago Nova/Col. Urbs Iulia (modern Cartagena, Spain) '
                   'was originally an indigenous settlement that was '
                   'subsequently refounded first as a Punic site Qart Hadasht '
                   '("New City") in 228 BC and later as a Roman city.',
        'title': 'Carthago Nova/Col. Urbs Iulia',
        'uri': 'https://pleiades.stoa.org/places/265849'}]
>>> p = si.web.get(results["hits"][3]["uri"] + "/json")
>>> print(p.json().keys())
dict_keys(['features', 'contributors', 'locations', 'connections', 'references', 'names', 'id', 'subject', 'title', 'provenance', 'placeTypeURIs', 'details', '@context', 'review_state', 'type', 'description', 'reprPoint', 'placeTypes', 'bbox', 'connectsWith', 'rights', 'created', 'uri', 'creators', '@type', 'history'])
>>> p.json()["reprPoint"]
[-0.9879534999999999, 37.602787]
>>> geometries = [loc["geometry"] for loc in p.json()["locations"]]
>>> for g in geometries:
...     f"{g['type']}({','.join([str(c) for c in g['coordinates']])})"
... 
'Point(-0.991387,37.605678)'
'Point(-0.98452,37.599896)'
>>> 
```



