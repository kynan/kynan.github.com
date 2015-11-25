---
layout: post
title: Reflections on the Accountability Hack 2015
category: posts
draft: true
---

A very frosty November weekend marked the end of [Parliament Week] and the
fifth anniversary of the [Accountability Hack], originally named UK Parliament
Hack, organised by [Tracy Green] from the Parliament Digital Service, [Nick
Halliday] from the [National Audit Office] and [Terry Makewell] from the
[Office for National Statistics] with very active support from the
[RebelUncut] crew.

Hackers and "armchair auditors" were invited to tackle four different
challenges using a diverse set of [open data sources]:

* **NAO**: Use spend data and any other data set to improve accountability. 
* **Parliament**: Best use of linked data to improve accountability. 
* **ONS**: Use the ONS OpenAPI to improve accountability.
* **Wildcard**: Use any three open data sets to improve accountability.

Many prospective participants were deterred by either the freezing cold or by
issues with public transport, so not that many heard [Meg Hillier MP] give the
introductory address. After that, ideas were thrown around and teams started
forming. I joined [Natalia], [Mina] and [Emma], a brilliant trio who were
working on a visualisation of [Parliamentary Questions Answered] and were
looking for some help with crunching the data and classifying the quality of
answers.

We first of all needed to pull all data from the Parliament's [Linked Data
API] in JSON format. Downloading all 63000 questions in batches of 500 (which
is the maximum batch size the API allows unfortunately) by hand was of course
not an option, so I started by implementing a download script in Python.
Pulling down all questions took several hours due to the rather poor
performance of the API.

In the evening, [Kevin] ran a very entertaining round of the [MLH !LIGHT]
challenge, where each contestant has 15 minutes to re-create a given website
(in our case it was the [bootstrap] front page) using a very bare bones
browser based editor with no syntax highlighting or auto completion. No
navigating away from the tab to bring up help and you don't get to see a
rendered preview of your creation until after you submitted.

The overnight stay at the NAO was again quite comfortable and we could use a
shower in the morning. My team from the Saturday did sadly not come back,
however [John], a good friend and brilliant data scientist, arrived in time
for breakfast. We discussed classifying answer quality with an N-gram analysis
using a list of previously identified phrases commonly used to defer
questions. An alternative would be training a text analysis model on the
entire text corpus based on a training set of manually classified answers.

Before getting to work on that we needed to transform the data pulled by my
script into a suitable form and identify which attributes were relevant for
our analysis. Halfway through doing that I realised the answer text was
missing from the data and found out it was due to passing a query parameter to
the API (`_view=all`), which included some extra fields, but left out the
actual answer data. By that point it was unrealistic to be able to rerun the
entire download in time for show & tell.

[Parliament Week]: https://parliamentweek.org
[Accountability Hack]: http://accountabilityhack.org
[Tracy Green]: https://twitter.com/greentrac
[National Audit Office]: http://nao.org.uk
[Terry Makewell]: https://twitter.com/TerryMakewell
[Office for National Statistics]: http://ons.gov.uk
[RebelUncut]: http://rebeluncut.co.uk
[open data sources]: http://accountabilityhack.org/#data
[Meg Hillier MP]: https://twitter.com/Meg_HillierMP
[Natalia]: https://twitter.com/NataliaLKB
[Mina]: https://twitter.com/minaorangina
[Emma]: https://twitter.com/emjan29
[Parliamentary Questions Answered]: http://explore.data.parliament.uk/?learnmore=Parliamentary Questions Answered
[Linked Data API]: http://lda.data.parliament.uk/answeredquestions
[Kevin]: http://lws.io
[MLH !LIGHT]: http://no-light.mlh.io
[bootstrap]: http://getbootstrap.com
[John]: http://artofinference.com

<a class="twitter-timeline" data-dnt="true"
href="https://twitter.com/frathgeber/timelines/668551765114142721"
data-widget-id="668931602173546497" data-width="100%">Accountability Hack 2015</a>
<script>!function(d,s,id){var js,fjs=d.getElementsByTagName(s)[0],p=/^http:/.test(d.location)?'http':'https';if(!d.getElementById(id)){js=d.createElement(s);js.id=id;js.src=p+"://platform.twitter.com/widgets.js";fjs.parentNode.insertBefore(js,fjs);}}(document,"script","twitter-wjs");</script>
