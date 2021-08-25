# Workpath Developer Candidate Code Project

In order to help us get a better sense of your approach to coding and ability as a developer, we have designed a small code exercise.  The goal of the exercise is to build a simple comments API on top of the NY Times Top Stories API.


What's Allowed
--------------

- You may create your project in Elixir, Ruby, or JavaScript.
- You may use any frameworks, libraries or third-party modules you wish. Your choice to use or not use third party tools is not going to impact your evaluation one way or the other. Just keep in mind that we want to be able to see your code, not your familiarity with a specific framework.


What's Not Allowed
------------------

- It should go without saying, but you cannot simply copy/paste a solution that you found on the internet.  This should be original code written by you.
- Do not use a hosting provider or submit a link to a hosted version of your app.


Prerequisites
-------------

You will need to request your own API key for access to the Top News service:

https://developer.nytimes.com/accounts/create

Documentation for the service is located here:

https://developer.nytimes.com/docs/top-stories-product/1/overview


Requirements
--------------

- The comments should be stored in a way that does not require an additional service to be installed or configured.
- Comments do not have to be saved between server restarts, but it is a bonus if they are.

The API should support to these requests:

```json
// GET /articles?section={section}

// Response
// 200 OK
// - If the section query parameter is not given, default it to "home"


[
  {
    "id": "article-uuid", // Can be extracted from the `uri` property in NY Times API response
    "title": "NY Times Article Title Here",
    "byline": "NY Times Article Byline Here",
    "published_date": "ISO-8601 publication date here",
    "comments_count": 10, // The number of comments for this article
    "comments_path": "/articles/{article-uuid}/comments"
  }
]
```

```json
// GET /articles/{article-uuid}/comments

// Response
// 200 OK
// - Comments should be ordered by `posted_at` descending

[
  {
    "id": "7697b7d8-a998-4939-83ce-58b79b7a179d",
    "article_id": "a6b8fe85-10ef-45d1-8e67-ae942da25034",
    "author": "Foo Bar",
    "body": "Comment body goes here.",
    "posted_at": "2021-08-22T18:02:02Z"
  },
  {
    "id": "7a4ea4d6-df90-470c-abb1-03fd28111643",
    "article_id": "a6b8fe85-10ef-45d1-8e67-ae942da25034",
    "author": "Baz Qux",
    "body": "Comment body goes here.",
    "posted_at": "2021-08-22T14:08:02Z"
  }
  // ...
]
```

```json
// POST /articles/{article-uuid}/comments

// Request

{
  "author": "Foo Bar",
  "body": "This is how I feel about this article."
}

// Response
// 200 OK

{
  "id": "generated-uuid",
  "article_id": "article-uuid",
  "author": "Foo Bar",
  "body": "This is how I feel about the article.",
  "posted_at": "2021-08-22T14:08:02Z"
}

// Response
// 400 Bad Request
// - Can occur if `author` and/or `body` are not provided or are blank strings

{
  "message": "Bad Request",
  "errors": {
    "author": ["can't be blank"],
    "body": ["can't be blank"]
  }
}

```


Extra Credit
--------------

- Convert all dates in the API response to the Fiji time zone (FJT).
- Store comments in a way that persists them independently of the server.
- Cache fetched NY Times article data for 15 minutes.