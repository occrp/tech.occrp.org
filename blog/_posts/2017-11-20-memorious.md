---
layout: post
title: "Introducing memorious, a web crawling toolkit"
author: Amy Guy, Friedrich Lindenberg
description: >
    A Python-based web crawling and scraping framework that combines the
    versatility of requests/BeautifulSoup-style scraping with a robust
    story for error logging, timed runs and component re-use.
---

<img src="/assets/images/2017-11/funes.png" class="img-right">

One way we improve the efficiency of investigations here at OCCRP is through
a technical infrastructure which makes diverse data sources more accessible
to journalists. Part of this involves gathering data from across the web;
data which is often found on obscure and potentially uncooperative websites,
in a huge variety of languages, formats and structures.

Today we are pleased to announce that we have channelled our years of scraping
and crawling expertise into a lightweight open source framework for web
crawling: `memorious`.

Our goal is to make it as simple as possible to create and maintain a fleet of
crawlers. With `memorious`, you can:

* Schedule crawlers to run at regular intervals (or run them ad-hoc as you need).
* Distribute scraping tasks across multiple machines.
* Keep track of error messages and warnings and help admins see which crawlers
  are in need of maintenance.
* Maintain an overview of your crawlers' status using the command line or a 
  web-based admin interface.

For common crawling tasks, `memorious` does all of the heavy lifting. One of
our most frequent objectives is to follow every link on a large website and download
every PDF. To achieve this with `memorious`, all you need to write [is a YAML file](https://github.com/alephdata/memorious/blob/master/example/config/simple_web_scraper.yml).

<div class="captioned">
    <img src="/assets/images/2017-11/memorious-ui.png" class="img-responsive">
    <div class="caption">
        A web-based admin interface allows you to keep track of the status of all
        of your crawlers.
    </div>
</div>

`memorious` is easily extensible, and contains lots of helpers to make building
your own custom crawlers as convenient as possible. Any `memorious` crawler is
comprised of a pipeline of different stages which can call each other in succession
(or themselves, recursively), depending on the results. Each stage executes an built-in
Memorious operation, or an operation you have defined yourself (as a Python function),
and can take input from a previous stage as well as from the crawler's configuration
file. This makes crawlers very modular, and common parts are easily reusable. All
crawlers benefit from automatic session persistence, caching and logging.

As an added bonus, `memorious` includes direct integration with the
[Aleph](https://github.com/alephdata/aleph) API, which means documents saved as a
result of your crawling become immediately searchable and sortable, taking
advantage of our state-of-the-art OCR and entity extraction.

For a complete description of what `memorious` can do, see the
[documentation](https://memorious.readthedocs.io) and check out our [example project](https://github.com/alephdata/memorious/tree/master/example).
You can try `memorious` out by running it locally in [development mode](https://memorious.readthedocs.io/en/latest/installation.html#development-mode), and we
also have a Docker setup for robust production deployment.

As we continually improve our own crawler development at OCCRP, we'll be adding
features to `memorious` for everyone to use. Similarly, we'd love input from the data 
journalism and open data communities; [issues](https://github.com/alephdata/memorious/issues)
and [PRs](https://github.com/alephdata/memorious) are welcome.

[TODO: insert hilarious sign-off pun here]