---
layout: twitter
title: Analysing Parliamentary Questions Answered at Accountability Hack 2015
category: posts
---

A very frosty November weekend marked the end of [Parliament Week] and the
fifth anniversary of the [Accountability Hack], originally named UK Parliament
Hack, organised by [Tracy Green] from the Parliament Digital Service, [Nick
Halliday] from the [National Audit Office] and [Terry Makewell] from the
[Office for National Statistics] with very active support from the
[RebelUncut] crew.

<blockquote class="twitter-tweet" data-cards="hidden" lang="en"><p lang="en" dir="ltr">Hack <a href="https://twitter.com/hashtag/Parliament?src=hash">#Parliament</a>&#39;s data and show us what you can do with it, sign up to <a href="https://twitter.com/hashtag/AccHack15?src=hash">#AccHack15</a> <a href="https://t.co/mcN02u4NbV">https://t.co/mcN02u4NbV</a></p>&mdash; UK Parliament (@UKParliament) <a href="https://twitter.com/UKParliament/status/665273451368873984">November 13, 2015</a></blockquote>

Hackers and "armchair auditors" were invited to tackle four different
challenges using a diverse set of [open data sources]:

* **NAO**: Use spend data and any other data set to improve accountability. 
* **Parliament**: Best use of linked data to improve accountability. 
* **ONS**: Use the ONS OpenAPI to improve accountability.
* **Wildcard**: Use any three open data sets to improve accountability.

<blockquote class="twitter-tweet" lang="en"><p lang="en" dir="ltr">And we&#39;re LIVE! <a href="https://twitter.com/hashtag/AccHack15?src=hash">#AccHack15</a> is go <a href="https://t.co/QHjV7QWOUd">pic.twitter.com/QHjV7QWOUd</a></p>&mdash; RebelUncut (@RebelUncut) <a href="https://twitter.com/RebelUncut/status/667991291893010432">November 21, 2015</a></blockquote>

Many prospective participants were deterred by either the freezing cold or by
issues with public transport, so not that many heard [Meg Hillier MP] give the
introductory address. After that, ideas were thrown around and teams started
forming. I joined [Natalia], [Mina] and [Emma], a brilliant trio who were
working on a visualisation of [Parliamentary Questions Answered] and were
looking for some help with crunching the data and classifying the quality of
answers.

<blockquote class="twitter-tweet" lang="en"><p lang="en" dir="ltr">Wonderful to have <a href="https://twitter.com/Meg_HillierMP">@Meg_HillierMP</a> sharing her passion for holding &quot;people like me&quot; accountable <a href="https://twitter.com/hashtag/AccHack15?src=hash">#AccHack15</a> <a href="https://t.co/q98Q85HkPq">pic.twitter.com/q98Q85HkPq</a></p>&mdash; RebelUncut (@RebelUncut) <a href="https://twitter.com/RebelUncut/status/668010032991223808">November 21, 2015</a></blockquote>

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

<blockquote class="twitter-tweet" lang="en"><p lang="en" dir="ltr"><a href="https://twitter.com/emjan29">@emjan29</a> <a href="https://twitter.com/minaorangina">@minaorangina</a> <a href="https://twitter.com/NataliaLKB">@NataliaLKB</a> I’m working with <a href="https://twitter.com/frathgeber">@frathgeber</a> at <a href="https://twitter.com/hashtag/AccHack15?src=hash">#AccHack15</a> we have a big flat CSV of MPs Q&amp;A, you coming back for today?</p>&mdash; John Sandall (@John_Sandall) <a href="https://twitter.com/John_Sandall/status/668401018288349184">November 22, 2015</a></blockquote>

<blockquote class="twitter-tweet" lang="en"><p lang="en" dir="ltr">I’m at Accountability Hack at the <a href="https://twitter.com/NAOorguk">@NAOorguk</a> today <a href="https://twitter.com/hashtag/AccHack15?src=hash">#AccHack15</a> using <a href="https://twitter.com/UKParliData">@ukparlidata</a> <a href="https://twitter.com/hashtag/data?src=hash">#data</a> to hold lawmakers to account! <a href="https://t.co/JKCQgzN33C">pic.twitter.com/JKCQgzN33C</a></p>&mdash; John Sandall (@John_Sandall) <a href="https://twitter.com/John_Sandall/status/668403230745980928">November 22, 2015</a></blockquote>

Before getting to work on that we needed to transform the raw data into a
suitable form and identify which attributes were relevant for our analysis.
Halfway through doing that I realised the answer text was missing from the
data and found out it was due to passing a query parameter to the API
(`_view=all`), which included extra fields, but left out the actual answer
data. By that point it was unrealistic to be able to rerun the entire download
in time for show & tell.

<blockquote class="twitter-tweet" lang="en"><p lang="en" dir="ltr">Need to pull all data from <a href="https://twitter.com/UKParliData">@UKParliData</a> again, because ?_view=all contrary to what you might think does *not* give you everything <a href="https://twitter.com/hashtag/AccHack15?src=hash">#AccHack15</a></p>&mdash; Florian Rathgeber (@frathgeber) <a href="https://twitter.com/frathgeber/status/668432437370880000">November 22, 2015</a></blockquote>

<blockquote class="twitter-tweet" lang="en"><p lang="en" dir="ltr">Full MPs answered questions <a href="https://twitter.com/hashtag/OpenData?src=hash">#OpenData</a> from <a href="https://twitter.com/UKParliData">@ukparlidata</a> as 62k row CSV (7MB compressed) -&gt; <a href="https://t.co/JmuqMdl3k9">https://t.co/JmuqMdl3k9</a> <a href="https://twitter.com/hashtag/AccHack15?src=hash">#AccHack15</a></p>&mdash; John Sandall (@John_Sandall) <a href="https://twitter.com/John_Sandall/status/668408168356139009">November 22, 2015</a></blockquote>

John did however still manage to run some statistical analysis on the data to
answer many interesting questions. Meanwhile I turned my download script into
a "proper" Python package using [PyScaffold], uploaded the package to [PyPI]
and the [documentation] to [Read the Docs] - just in time for going up on
stage!

<blockquote class="twitter-tweet" lang="en"><p lang="en" dir="ltr">Here we go!! <a href="https://twitter.com/gabysslave">@gabysslave</a> kicking off <a href="https://twitter.com/hashtag/AccHack15?src=hash">#AccHack15</a> show &amp; tell <a href="https://t.co/kkTCNOZ5jG">pic.twitter.com/kkTCNOZ5jG</a></p>&mdash; RebelUncut (@RebelUncut) <a href="https://twitter.com/RebelUncut/status/668447489339564033">November 22, 2015</a></blockquote>

<blockquote class="twitter-tweet" lang="en"><p lang="en" dir="ltr">Sounds like we&#39;re going to get some robust feedback on <a href="https://twitter.com/UKParliData">@UKParliData</a> now... <a href="https://twitter.com/hashtag/acchack15?src=hash">#acchack15</a></p>&mdash; Dan Barrett (@dasbarrett) <a href="https://twitter.com/dasbarrett/status/668459325011177472">November 22, 2015</a></blockquote>

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

<blockquote class="twitter-tweet" lang="en"><p lang="en" dir="ltr"><a href="https://twitter.com/hashtag/acchack15?src=hash">#acchack15</a> next up any questions answered... <a href="https://t.co/4KaEJHr64m">pic.twitter.com/4KaEJHr64m</a></p>&mdash; Ranjan Balakumaran (@financialeyes) <a href="https://twitter.com/financialeyes/status/668459577038520320">November 22, 2015</a></blockquote>

<blockquote class="twitter-tweet" lang="en"><p lang="en" dir="ltr">Fun fact: <a href="https://twitter.com/nick_clegg">@nick_clegg</a> ranks higher than any other MP for how long it takes to get questions answered (49 days on avg) <a href="https://twitter.com/hashtag/PoorNick?src=hash">#PoorNick</a> <a href="https://twitter.com/hashtag/AccHack15?src=hash">#AccHack15</a></p>&mdash; John Sandall (@John_Sandall) <a href="https://twitter.com/John_Sandall/status/668428101609775104">November 22, 2015</a></blockquote>

<blockquote class="twitter-tweet" lang="en"><p lang="en" dir="ltr">&quot;Have all these MPs always been so annoying?!&quot; <a href="https://twitter.com/John_Sandall">@John_Sandall</a> Love it <a href="https://twitter.com/hashtag/AccHack15?src=hash">#AccHack15</a></p>&mdash; RebelUncut (@RebelUncut) <a href="https://twitter.com/RebelUncut/status/668459719363833856">November 22, 2015</a></blockquote>

Later I went on stage again to present [DDPy], a command line interface to
interact with the Parliament [Linked Data API], which had evolved out of my
download script to pull the [Parliamentary Questions Answered]. I decided to
solve this once and for all and, wrote a generic downloader in Python and put
it on [PyPI]. Now anyone can easily download any data set after a simple

    pip install ddpkuk

<blockquote class="twitter-tweet" lang="en"><p lang="en" dir="ltr">Ok so *now* <a href="https://twitter.com/frathgeber">@frathgeber</a> is going to give us some robust feedback on <a href="https://twitter.com/UKParliData">@ukparlidata</a> <a href="https://twitter.com/hashtag/acchack15?src=hash">#acchack15</a></p>&mdash; Dan Barrett (@dasbarrett) <a href="https://twitter.com/dasbarrett/status/668469540301496322">November 22, 2015</a></blockquote>

<blockquote class="twitter-tweet" lang="en"><p lang="en" dir="ltr">DDPy from <a href="https://twitter.com/frathgeber">@frathgeber</a> <a href="https://twitter.com/hashtag/AccHack15?src=hash">#AccHack15</a> <a href="https://t.co/YeBULBhRrt">https://t.co/YeBULBhRrt</a></p>&mdash; RebelUncut (@RebelUncut) <a href="https://twitter.com/RebelUncut/status/668470085678444544">November 22, 2015</a></blockquote>

<blockquote class="twitter-tweet" lang="en"><p lang="en" dir="ltr">Read how to use DDPy to access <a href="https://twitter.com/UKParliData">@UKParliData</a> <a href="https://twitter.com/hashtag/data?src=hash">#data</a> at <a href="https://t.co/iJTmrdEv89">https://t.co/iJTmrdEv89</a> and get the code on <a href="https://twitter.com/github">@GitHub</a> <a href="https://t.co/ufLZdFP0pk">https://t.co/ufLZdFP0pk</a> <a href="https://twitter.com/hashtag/AccHack15?src=hash">#AccHack15</a></p>&mdash; Florian Rathgeber (@frathgeber) <a href="https://twitter.com/frathgeber/status/668473642708246528">November 22, 2015</a></blockquote>

The judges apparently came away quite impressed with both our presentations
since we received an honourable mention for the "Best Analysis of Parliamentary
Data" for [Any Questions Answered?][insights] and the "Best Tool for the
Community" for [DDPy]. As if that wasn't enough, I was quite touched for also
being awarded a "Community Spirit Prize". It was (and continues to be) an
honour and pleasure serving the community!

<blockquote class="twitter-tweet" lang="en"><p lang="en" dir="ltr">Got an honourable mention at <a href="https://twitter.com/hashtag/AccHack15?src=hash">#AccHack15</a> for &quot;Any Question Answered?&quot; our analysis of Parliamentary Questions Answered /w <a href="https://twitter.com/John_Sandall">@John_Sandall</a></p>&mdash; Florian Rathgeber (@frathgeber) <a href="https://twitter.com/frathgeber/status/668486316645081088">November 22, 2015</a></blockquote>

<blockquote class="twitter-tweet" lang="en"><p lang="en" dir="ltr">Got another honourable mention at <a href="https://twitter.com/hashtag/AccHack15?src=hash">#AccHack15</a> for DDPy as a useful tool for the <a href="https://twitter.com/hashtag/hackathon?src=hash">#hackathon</a> community. So go &amp; use it! <a href="https://t.co/80K4puh9Wh">https://t.co/80K4puh9Wh</a></p>&mdash; Florian Rathgeber (@frathgeber) <a href="https://twitter.com/frathgeber/status/668486769495695360">November 22, 2015</a></blockquote>

<blockquote class="twitter-tweet" lang="en"><p lang="en" dir="ltr">No end of awards today it seems ;)&#10;Many thanks to <a href="https://twitter.com/RebelUncut">@RebelUncut</a> <a href="https://twitter.com/nickmhalliday">@nickmhalliday</a> and <a href="https://twitter.com/greentrac">@greentrac</a> for running <a href="https://twitter.com/hashtag/AccHack15?src=hash">#AccHack15</a> <a href="https://t.co/9hvSrkubyY">https://t.co/9hvSrkubyY</a></p>&mdash; Florian Rathgeber (@frathgeber) <a href="https://twitter.com/frathgeber/status/668487299320176640">November 22, 2015</a></blockquote>

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
[DDPy]: https://github.com/kynan/DDPy

# Any Questions Answered?

* [Code](https://github.com/john-sandall/acchack2015)
* [Slides][insights]

# DDPy - data.parliament.uk for Humans

* [Code][DDPy]
* [Documentation][documentation]
* [Python Package][PyPI]

# Other resources

* [Accountability Hack 2015 hackpad](http://bit.ly/AccHackpad15)
