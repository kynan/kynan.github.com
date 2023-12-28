---
layout: post
title: Save all voicebox messages from your FRITZ!Box 7530 router
category: posts
---

My parents' FRITZ!Box 7530 (FRITZ!OS version 7.57) voicebox had "filled up" and before clearing all messages I was looking for a way to download all of them as a backup.

There is no really convenient way to do this in the web UI (there were close to 200 messages, some dating back more than 4 years), so I resorted to JavaScript to print a list of `curl` commands to download each message as a `.wav` file.

Messages are listed in a table with the following format:
```html
<tr class="thead">
    <th class="sortable"></th>
    <th class="sortable sort_by_date">Date<span class="sort_no">&nbsp;</span></th>
    <th class="sortable">Name/Number<span class="sort_no">&nbsp;</span></th>
    <th class="sortable">Your Number<span class="sort_no">&nbsp;</span></th>
    <th class="sortable">Duration<span class="sort_no">&nbsp;</span></th>
    <th></th>
</tr>
<tr>
    <td class="newicon" datalabel="28.12.23 16:00">&nbsp;</td>
    <td>28.12.23 16:00</td>
    <td datalabel="Name/Number">Caller's Name</td>
    <td datalabel="Your Number">1234</td>
    <td datalabel="Dauer">&lt; 1 Min</td>
    <td class="btncolumn" datalabel="">
        <button type="submit" class="icon fon_book" id="" name="" value="" title="" disabled=""><img src="/assets/icons/ic_fonbook_add.svg" alt=""></button>
        <a class="download icon" href="/cgi-bin/luacgi_notimeout?sid=<session uuid>&amp;script=%2Flua%2Fphoto.lua&amp;myabfile=%2Fdata%2Ftam%2Frec%2F<recording id>">
            <button type="submit" class="icon audio" id="play_1" name="play" value="1" title="Play message/Save">
                <img src="/assets/icons/ic_triangle_right_blue.svg" alt="Play message/Save">
            </button>
            <audio preload="auto"></audio>
        </a>
        <button type="submit" class="icon delete" id="delete_1" name="delete" value="1" title="Delete"></button>
    </td>
</tr>
...
```

We want to ignore the header row and for every subsequent row, get the date and the caller's name or number, to build a meaningful file name for the to-be-downloaded `.wav` file, and the URL of the link. The link points to a script that emits the raw WAV audio as a binary stream.

This is a quick & dirty piece of JavaScript to paste into the JavaScript console of Chrome's Developer tools (keyboard shortcut Ctrl-Shift-I) to generate a list of `curl` invocations to download each message with a useful file name:
```js
console.log(Array.from(document.querySelectorAll('table#uiTamCalls tr:not(.thead)')).map(e => {
    return `curl -s -o "${e.childNodes[1].innerText} ${e.childNodes[2].innerText}.wav" '${e.querySelector('a').href}'`;
}).join('\n'));
```

My initial attempt was downloading the file via the browser by invoking `.click()` on each link, however that turned out to work less well as Chrome doesn't really like downloading files over a non-secure connection, requires you to allow multiple simultaneous downloads via a popup box and even if you do, will only download 10 files at a time.

YMMV if you have a different FRITZ!Box model or FRITZ!OS version, however I'd expect a similar approach should still work.
