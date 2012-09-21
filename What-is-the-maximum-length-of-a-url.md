[RFC 2616](http://www.faqs.org/rfcs/rfc2616.html) (Hypertext Transfer Protocol HTTP/1.1) section 3.2.1 says

The HTTP protocol does not place any a priori limit on the length of
a URI. Servers MUST be able to handle the URI of any resource they serve, and SHOULD be able to handle URIs of unbounded length if they provide GET-based forms that could generate such URIs. A server SHOULD return 414 (Request-URI Too Long) status if a URI is longer than the server can handle (see section 10.4.15).

Note: Servers ought to be cautious about depending on URI lengths above 255 bytes, because some older client or proxy implementations might not properly support these lengths.

...and the reality

That's what the standards say. For the reality, see [this research over at boutell.com](http://www.boutell.com/newfaq/misc/urllength.html) to see what individual browser and server implementations will support. It's worth a read, but the executive summary is:

Extremely long URLs are usually a mistake. URLs over 2,000 characters will not work in the most popular web browser. Don't use them if you intend your site to work for the majority of Internet users.

Also, be aware that the [sitemaps protocol](http://www.sitemaps.org/protocol.html), which allows a site to inform search engines about available pages, has a limit of 2048 characters in a URL. If you intend to use sitemaps, a limit has been decided for you! (see [Calin-Andrei Burloiu's answer](http://stackoverflow.com/a/7056886/6521) below)

There's also some research from 2010 into the [maximum URL length that search engines will crawl and index.](http://www.seomofo.com/experiments/title-and-h1-of-this-post-but-for-the-sake-of-keyword-prominence-stuffing-im-going-to-mention-it-again-using-various-synonyms-stemmed-variations-and-of-coursea-big-fat-prominent-font-size-heres-the-stumper-that-stumped-me-what-is-the-max-number-of-chars-in-a-url-that-google-is-willing-to-crawl-and-index-for-whatever-reason-i-thought-i-had-read-somewhere-that-googles-limit-on-urls-was-255-characters-but-that-turned-out-to-be-wrong-so-maybe-i-just-made-that-number-up-the-best-answer-i-could-find-was-this-quote-from-googles-webmaster-trends-analyst-john-mueller-we-can-certainly-crawl-and-index-urls-over-1000-characters-long-but-that-doesnt-mean-that-its-a-good-practice-the-setup-for-this-experiment-is-going-to-be-pretty-simple-im-going-to-edit-the-permalink-of-this-post-to-be-really-really-long-then-im-going-to-see-if-google-indexes-it-i-might-even-see-if-yahoo-and-bing-index-iteven-though-no-one-really-cares-what-those-assholes-are-doing-url-character-limits-unrelated-to-google-the-question-now-is-how-many-characters-should-i-make-the-url-of-this-post-there-are-a-couple-of-sources-ill-reference-to-help-me-make-this-decision-the-first-is-this-quote-from-the-microsoft-support-pages-microsoft-internet-explorer-has-a-maximum-uniform-resource-locator-url-length-of-2083-characters-internet-explorer-also-has-a-maximum-path-length-of-2048-characters-this-limit-applies-to-both-post-request-and-get-request-urls-the-second-source-ill-cite-is-the-http-11-protocol-which-says-the-http-protocol-does-not-place-any-a-priori-limit-on-the-length-of-a-uri-servers-must-be-able-to-handle-the-uri-of-any-resource-they-serve-and-should-be-able-to-handle-uris-of-unbounded-length-if-they-provide-get-based-forms-that-could-generate-such-uris-a-server-should-return-414-request-uri-too-long-status-if-a-uri-is-longer.html) They found the limit was 2047 chars, which appears allied to the sitemap protocol spec. However, they also found the Google SERP tool wouldn't cope with URLs longer than 1855 chars.

Footnote

This is a popular question, and as the original research is nearly 6 years old I'll try to keep it up to date: As of July 2012, the advice still stands, as [IE8's maximum URL length is 2083 chars](http://support.microsoft.com/kb/q208427/), and it seems [IE9 has a similar limit](http://stackoverflow.com/questions/3721034/how-long-an-url-can-internet-explorer-9-take).

Source: http://stackoverflow.com/a/417184