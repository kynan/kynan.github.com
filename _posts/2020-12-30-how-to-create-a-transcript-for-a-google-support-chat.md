---
layout: post
title: How to create a transcript of a Google Support chat
category: posts
---

When you interact with Google Support via chat, you can ask for a transcript of
the conversation to be sent to you. That transcript is in PDF format though. If
that's not suitable for you or you forgot to request a transcript, there is a
way out.

Open the chat in its own pop-out window via the arrow icon on the right of the
blue header bar. Then open the Chrome developer console
(<kbd>Ctrl</kbd>-<kbd>Shift</kbd>-<kbd>J</kbd>), paste the following JavaScript
code and hit enter to copy a transcript of the conversation to your clipboard.

```javascript
const copyToClipboard = str => {
  const el = document.createElement('textarea');
  el.value = str;
  document.body.appendChild(el);
  el.select();
  document.execCommand('copy');
  document.body.removeChild(el);
};

copyToClipboard(Array.from(document.querySelectorAll('.chatsupport_cbf_qb')).map(e => {
  t = new Date(parseInt(e.getAttribute('ts'))).toISOString();
  if (e.querySelector('.systemMessageUserWrapper')) {
    return `[${t}] ${e.querySelector('.systemMessageUserWrapper').innerText}`;
  } else {
      n = e.querySelector('.chatsupport_cbf_rb').getAttribute('aria-label');
      m = e.querySelector('.chatsupport_cbf_ob').innerText;
      return `[${t}][${n}]: ${m}`;
  }
}).join('\n'));
```