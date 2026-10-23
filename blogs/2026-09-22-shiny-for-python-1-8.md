---
title: "Shiny for Python 1.8"
url: "https://opensource.posit.co/blog/2026-09-22_shiny-python-1-8/"
date: "2026-09-22"
author: "Barret Schloerke"
feed_url: "https://opensource.posit.co/blog/index.xml"
---
We’re happy to announce that Shiny for Python v1.8 is now on PyPI! pip install -U shiny The highlights: * test_server() runs your app’s server logic in memory so you can test it without a browser, * ui.page_html() lets a complete HTML document (say, the index.html your JS bundler emits) be the app’s UI, and * session.allow_reconnect() lets the browser reconnect to a live session after a dropped connection. Full details are in the Shiny for Python changelog .
