---
layout: post
title: Linked Data for investigative journalism
author: Amy Guy
description: ..
---

OCCRP's Investigative Dashboard ([https://data.occrp.org](https://data.occrp.org)) houses data about hundreds of thousands of people, companies, and the relationships between them. The structured data has been scraped from the Web, ported from public records and news outlets, and extracted from leaked documents like the [Panama Papers](), as well as input directly by journalists.

Until now, this data has been accessible to the public via a UI and a JSON API. This week, we have released an RDF dump: x million triples, about x million entities.

You can download it from archive.org: [Turtle](), [JSON-LD]().

The vocabulary we use to describe the data is [FollowTheMoney](https://alephdata.github.io/followthemoney/ns/).

What will you do with this data?

Problems we would like to see solved include:

* ...
* entity disambiguation
* data enrichment and linking with other sources
* easy-to-follow visualisations of networks
* ...

If you want to start a project using this data, or if you plan to use it as a teaching resource, please [get in touch]() and let us know!

If you run your own instance of aleph, you can take advantage of the RDF export feature for your own data, by running `aleph rdfdump [path]` at the aleph commandline. 