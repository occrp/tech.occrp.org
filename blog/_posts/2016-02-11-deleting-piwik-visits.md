---
layout: post
title: Manually deleting visits from Piwik by their IP (or URL, or...)
author: Aleksandar Todorović (r3bl)
tags: ["Piwik"]
# categories: ["Sysadmin"]
---

[Piwik](http://piwik.org/) is an awesome self-hosted analytics service. We've been relying on it for a very long time and we were always satisfied with what it brought to us. During our work, however, we have accidentally allowed the traffic from our own servers to appear in Piwik, and some general traffic to be counted more than once, therefore we have accidentally boosted our own stats.

Now, since Piwik itself is open sourced, we see no reason why we should _not_ be able to delete the artificially inflated stats ourselves and by doing so making sure that our journalists see the stats as precisely as possible.

Since the informations for this process were not as clear as we wanted them to be, I've decided to write this blog post so we could make the job easier to anyone else who tries to do the same. To follow this tutorial, you're going to need a Piwik installation (obviously), access to the command line on the server and some SQL-fu.

## Step 1: Finding the records that you want to delete

This seems like a simple thing, but it turned out to be much harder. We had the list of couple of IP addresses that we wanted to exclude from Piwik, but after about half an hour of me searching through Piwik's interface, I was not able to find a way how to see the entire traffic that originated from a specific IP address. Luckily, I stumbled upon [this short post](http://blog.onlineinstitute.com/traffic-analytics/how-to-search-for-ip-addresses-within-piwiks-logs/) which gave me every information I needed. To see the traffic from a specific IP, you have to manually tweak the URL you are visiting to:

```
https://piwik.example.com/index.php/?module=CoreHome&action=index&idSite=1&period=year&date=2016#module=Live&action=getVisitorLog&idSite=1&segment=visitIp=={{ IP ADDRESS GOES HERE }}
```

Bear in mind that Piwik shows 500 actions per visit as a maximum, so if the requested IP made over 500 actions in a single visit (for example, if it was a bot, or if somebody tried to scrape your website), you're only going to see the very first 500 actions that were requested by that IP.

## Step 2: Finding and deleting records from the database(s)

The second step would be to find the records in the database as well. To do this for the IP you're interested in, you're going to have to convert the IP address to the HEX numeral system. Of course, everyone who finished two IT college courses should be able to convert the number to its HEX value by hand, but if you feel too lazy, just use [this online tool](http://www.miniwebtool.com/ip-address-to-hex-converter/) to do so. Or use `python`:

```python
print hex({{IP_ADDRESS_BYTE}}) # repeat for each IPv4 byte
```

Once you have the HEX equivalent of the IP in question, log into MySQL/MariaDB and execute the following command to get the count of rows (or: pageviews) that will be affected:

```sql
SELECT COUNT(*)
    FROM piwik_log_visit AS log_visit
    LEFT JOIN piwik_log_link_visit_action as log_link_visit_action
        ON log_visit.idvisit = log_link_visit_action.idvisit
    LEFT JOIN piwik_log_action as log_action
        ON log_action.idaction = log_link_visit_action.idaction_url
WHERE log_visit.location_ip=UNHEX("{{ HEX_VALUE_GOES_HERE }}");
```

As you can see, Piwik stores the relevant visitor info into three separate MySQL databases: `piwik_log_visit`, `piwik_log_link_visit_action` and `piwik_log_action`.

If you skip one of them, you'll encounter some unexpected results. For example, initially, we've tried removing the data from `piwik_log_visit` and `piwik_log_link_visit_action`, but once we've re-computed the logs, we've noticed that the IP was still there and the visit time was still being shown, even though we have successfully deleted the actions associated with that visit.

    0 Action - 42 min 59s

This is why it's important to delete the data from all three of the databases.

To delete the necessary entries from all the databases, you need to tweak the command above like this (for IP-based pruning):

```sql
DELETE log_visit, log_link_visit_action
    FROM piwik_log_visit AS log_visit
    LEFT JOIN piwik_log_link_visit_action as log_link_visit_action
        ON log_visit.idvisit = log_link_visit_action.idvisit
    LEFT JOIN piwik_log_action as log_action
        ON log_action.idaction = log_link_visit_action.idaction_url
WHERE log_visit.location_ip=UNHEX("{{ HEX_VALUE_GOES_HERE }}");
```

You can verify that the visits/pageviews are gone from the db by using the `SELECT` statements again, of course.

## Step 3: Re-compute the reports

If you have successfully completed the first two steps, your last step should be re-computing the reports. If you skip this step, you won't accomplish anything because the traffic will still be visible in the reports, even though the traffic has been removed from the databases.

To do so, I highly recommend you to take a careful look at Piwik's documentation. Specifically, you should pay a close attention to these two posts:

* [How do I force the reports to be re-processed from the logs?](http://piwik.org/faq/how-to/faq_59/)
* [How do I record tracking data in the past, and tell Piwik to invalidate and re-process the past reports?](http://piwik.org/faq/how-to/faq_155/)

Make sure that you invalidate data for the particular sites and dates affected, as processing time is directly dependant on this.

## Bonus -- what about URLs?

Notice what we've put after the `WHERE` keyword in step number two. You can do all sorts of crazy thing there. For example:

```sql
[...] WHERE log_action.name LIKE 'example.com/wp-content/themes/%'
```

...will remove the traffic that hit the files associated with the WordPress theme you are using.
