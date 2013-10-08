---
layout: post
title: Taarifa reporting for International Medical Corps UK at hack4good London
category: posts
---

On the weekend of 4-6th October I was part of [#hack4good][1] in London
working with [Chris Adams][2], [Corinne Pritchard][3] and [Nick Stanton][4]
tackling multi-agency reporting at refugee camps for [International Medical
Corps UK][5] with the [Taarifa][6] platform.

We approached the problem from two angles: One was developing a workflow for a
paper-based reporting system, where report forms are automatically generated
for a specific area like a refugee camp. Refugees would be able make a report
by filling in the form themselves with the help of a simple iconography to
overcome the language barrier, mark the location on a map and drop it into a
collection box. The forms are machine readable and can be digitized in an
automated process.

The other was the technical challenge of providing a platform that could
process a variety of different reports that various agencies working at a
refugee camp need to deal with from santiation over general feedback to sexual
exploitation. These can be aggregated in one place by the [Taarifa API][7]
which I've extended to be able to handle any report schema. Creating a new
report type is a simple as sending a POST request describing the schema to the
API in JSON format.  Reports can then be submitted immediately via the JSON
API or via an automatically generated web form. All of that is live and
operational at <http://api.taarifa.org>.

There was quite a bit of media coverage around [#hack4good][1]: [Channel
4][8], [SkyNews][9] and a [video of the London event][10] by [Big Bang
Lab][11].

[1]: http://hack4good.io
[2]: http://chrisadams.me.uk
[3]: http://www.simplyunderstand.com
[4]: https://twitter.com/nickpstanton
[5]: http://www.internationalmedicalcorps.org.uk
[6]: http://taarifa.org
[7]: http://api.taarifa.org
[8]: http://www.channel4.com/news/hackers-hack4good-charity-event-san-francisco-coding
[9]: http://uk.news.yahoo.com/video/hack-good-world-hackers-unite-090405127.html
[10]: https://www.youtube.com/watch?v=znkKOeGStpQ
[11]: http://bigbang-lab.com
