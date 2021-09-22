# Protecting unique content

In order to prevent our unique content from being utilized by unauthorized third parties, we require that you design your site in such a way that this content will not be indexed by search engines.

The content that must be protected comprises:

- **Product reviews** – This includes all review text (from any provider) as obtained via the [/reviews/product](../../../openapi/reference/operation/reviewsProduct) endpoint; and, that in the `reviews` element in the product content response. 
- **Viator unique content** – This includes all data in all elements within the `viatorUniqueContent` element in the product content response.

## Guidelines to prevent indexing of unique content

In order to properly protect reviews and other unique content, you must ensure that the protected content **never** appears directly in the source code of the loaded page. This includes both HTML and Javascript.

In order to do this correctly, the unique content must be loaded via a call to an **external** Javascript, and that Javascript must be blocked in [robots.txt](https://moz.com/learn/seo/robotstxt) so that search engines can not see or index it.

## How to check

A simple way to check that the unique content does not appear in the source code of the loaded page – and, therefore, cannot be indexed – is to copy a snippet of the unique content text that is displayed when you load the page normally in your browser. Approximately 10 words will suffice.

Then, access the source code of that page using the **View Source** feature in your browser. Use your browser's in-page search feature to search for the text snippet copied in the previous step. If the text is found anywhere in the source code of the page, then your implementation is **not** correctly protecting the content, as it will be able to be indexed by search enginges. 

## Example implementations

❌ **Unacceptable implementation**: pure HTML

Here the protected content appears directly in the HTML and can be indexed by search engines:

```html
<html>
  <head>
  </head>
  <body>
    <div>”I had a great time at this hotel”</div>
  </body>
</html>
```

❌ **Unacceptable implementation**: Javascript in the page source

Here, the protected content still appears in the page source, even though it is in the 'script' section, and will therefore be indexed by search engines:


```html
<html>
  <head>
  <script>
    var review_content = ”I had a great time at this hotel”;
    $(“review”).html(review_content);
  </script>
  </head>
  <body>
    <div id=”review”></div>
  </body>
</html>
```

✅ **Acceptable implementation:** protected content is loaded using an **external** Javascript and access to that endpoint is blocked using robots.txt:

```html
<html>
  <head>
    <script>
      var review_content = $.ajax(“https://api.hello.xyz/getReviewContentForHotel/123/”);
      $(“review”).html(review_content);
    </script>
  </head>
  <body>
    <div id=”review”></div>
  </body>
</html>
```

Robots.txt in the document root for `https://api.hello.xyz`:

```bash
User-Agent: *
Disallow: /getReviewContentForHotel
```

**Note**: The robots.txt file must be in the root directory for whichever domain or subdomain the call is being made to. Please review the [robots.txt guidelines](https://developers.google.com/search/docs/advanced/robots/create-robots-txt) to determine the correct syntax for your site.