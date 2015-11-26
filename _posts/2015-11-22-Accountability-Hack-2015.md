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

<blockquote class="twitter-tweet" data-cards="hidden" lang="en"><p lang="en" dir="ltr">Hack <a href="https://twitter.com/hashtag/Parliament?src=hash">#Parliament</a>&#39;s data and show us what you can do with it, sign up to <a href="https://twitter.com/hashtag/AccHack15?src=hash">#AccHack15</a> <a href="https://t.co/mcN02u4NbV">https://t.co/mcN02u4NbV</a></p>&mdash; UK Parliament (@UKParliament) <a href="https://twitter.com/UKParliament/status/665273451368873984">November 13, 2015</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

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

Before getting to work on that we needed to transform the raw data into a
suitable form and identify which attributes were relevant for our analysis.
Halfway through doing that I realised the answer text was missing from the
data and found out it was due to passing a query parameter to the API
(`_view=all`), which included extra fields, but left out the actual answer
data. By that point it was unrealistic to be able to rerun the entire download
in time for show & tell.

John did however still manage to run some statistical analysis on the data to
answer many interesting questions. Meanwhile I turned my download script into
a "proper" Python package using [PyScaffold], uploaded the package to [PyPI]
and the [documentation] to [Read the Docs] - just in time for going up on
stage!

Quite a few extra spectators came along to attend the show & tell with a
rather impressive lineup of 16 projects! I was on stage twice: First to
present the results of our analysis of "Any Questions Answered?", which had
revealed some interesting [insights]. [Jim Shannon MP] asked the most questions,
the [Department of Health] had to answer the most. The [Foreign & Commonwealth
Office] was the slowest to respond. [Nick Clegg]'s questions were ignored the
longest and the Prime Minister referred the highest proportion of questions.
With more time to build up a training set by categorising some questions
manually e.g. for quality or difficulty, we could have trained a Bayes
classifier for the entire corpus.

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
[PyScaffold]: https://pyscaffold.readthedocs.org
[PyPI]: https://pypi.python.org/pypi/ddpuk
[documentation]: https://ddpy.readthedocs.org
[Read the Docs]: https://readthedocs.org
[insights]: https://github.com/john-sandall/acchack2015/blob/master/presentation/AccHack%20presentation.pdf
[Jim Shannon MP]: https://twitter.com/JimShannonMP
[Department of Health]: https://www.gov.uk/government/organisations/department-of-health
[Foreign & Commonwealth Office]: https://www.gov.uk/government/organisations/foreign-commonwealth-office
[Nick Clegg]: https://twitter.com/nick_clegg

# Any Questions Answered?

* [Code](https://github.com/john-sandall/acchack2015)
* [Slides][insights]

# DDPy - data.parliament.uk for Humans

* [Code](https://github.com/kynan/DDPy)
* [Documentation][documentation]
* [Python Package][PyPI]

# Other resources

* [Accountability Hack 2015 hackpad](http://bit.ly/AccHackpad15)

<a class="twitter-timeline" data-dnt="true"
href="https://twitter.com/frathgeber/timelines/668551765114142721"
data-widget-id="668931602173546497" data-width="100%">Accountability Hack 2015</a>
<script>!function(d,s,id){var js,fjs=d.getElementsByTagName(s)[0],p=/^http:/.test(d.location)?'http':'https';if(!d.getElementById(id)){js=d.createElement(s);js.id=id;js.src=p+"://platform.twitter.com/widgets.js";fjs.parentNode.insertBefore(js,fjs);}}(document,"script","twitter-wjs");</script>
