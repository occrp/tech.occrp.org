---
layout: post
title: "Introducing memorious, a web crawling toolkit"
author: Amy Guy and Friedrich Lindenberg
description: >
    A Python-based web crawling and scraping framework that combines the
    versatility of requests/BeautifulSoup-style scraping with a robust
    story for error logging, timed runs and component re-use.
---

Investigating the (often dubious) dealings of businessmen and politicians,
our reporters need access to documents and databases from all over the
world.

<img src="/assets/images/2017-11/funes.png" class="img-right">

To make their searches better, we're developing tools that make large
amounts of data accessible with a single keystroke. We have built a set
of crawlers that combine data from governments, corporations and other
media into a [search engine](https://data.occrp.org).

However, these crawlers need to deal with uncooperative websites in
different languages, formats and structures and they often break when
pages are updated. 

After experimenting with some existing solutions, we decided to make a
tool that encapsulates our experience with web crawling. The result is a
lightweight open source framework named `memorious`
([GitHub](https://github.com/alephdata/memorious)).

`memorious` is simple and yet allows you to create and maintain a fleet
of crawlers, while not forcing too much specific process.

* Schedule crawlers to run at regular intervals (or run them ad-hoc as you need).
* Keep track of error messages and warnings that help admins see which crawlers
  are in need of maintenance.
* Lets you use familiar tools like `requests`, `BeautifulSoup`, `lxml` or 
  `dataset` to do the actual scraping.
* Distribute scraping tasks across multiple machines.
* Maintain an overview of your crawlers' status using the command line or a 
  web-based admin interface.

For common crawling tasks, `memorious` does all of the heavy lifting. One
of our most frequent objectives is to follow every link on a large website and
download all PDF files. To achieve this with `memorious`, all you need to write
[is a YAML file](https://github.com/alephdata/memorious/blob/master/example/config/simple_web_scraper.yml)
that plugs together existing components.

<div class="captioned">
    <img src="/assets/images/2017-11/memorious-ui.png" class="img-responsive">
    <div class="caption">
        A web-based admin interface allows you to keep track of the status of all
        of your crawlers.
    </div>
</div>

Each `memorious` crawler is comprised of a set of different stages that call each
other in succession (or themselves, recursively). Each stage either executes a
built-in component, or a custom Python function, that may fetch, parse or store a
page just as you like it. `memorious` is also extensible, and contains lots of
helpers to make building your own custom crawlers as convenient as possible. 

These configurable chains of operations have made our crawlers very modular, and
common parts are reused efficiently. All crawlers can benefit from automatic
cookie persistence, HTTP caching and logging.

Within OCCRP, `memorious` is used to feed documents and structured data into
[aleph](https://github.com/alephdata/aleph) via an API, which means documents
become searchable as soon as they have been crawled. There, they will also
be sent through OCR and entity recognition. `aleph` aims to use these extracted
entities as bridges that link a given document to other databases and documents.

For a more detailed description of what `memorious` can do, see the
[documentation](https://memorious.readthedocs.io) and check out our
[example project](https://github.com/alephdata/memorious/tree/master/example).
You can try `memorious` by running it locally in [development mode](https://memorious.readthedocs.io/en/latest/installation.html#development-mode),
and, of course, we also have a Docker setup for robust production deployment.

As we continually improve our crawler infrastructure at OCCRP, we'll be adding
features to `memorious` for everyone to use. Similarly, we'd love input from the data 
journalism and open data communities; [issues](https://github.com/alephdata/memorious/issues)
and [PRs](https://github.com/alephdata/memorious) are welcome.

---

> &ldquo;&hellip;the solitary and lucid spectator of a multiform, instantaneous and almost
> intolerably precise world&hellip;&rdquo; (*Funes the memorious, Jorge Luis Borges*)