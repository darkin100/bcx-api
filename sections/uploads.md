Uploads
=======

Uploads show up as "Files" inside of Basecamp.


Create uploads
--------------

* `POST /projects/1/uploads.json` will create a new entry in the "Files" section on the given project, with the given attachment token.

Attaching files requires both the token and the name of the attachment. The
token is returned from the [Create attachments](https://github.com/37signals/bcx-api/blob/master/sections/attachments.md)
endpoint, which you must hit first before creating an upload.

The `name` parameter *must* be a valid filename with an extension. Only one
attachment is allowed.

```json
{
  "content": "Here's the new logo!",
  "attachments": [
    {
      "token": "4f71ea23-134660425d1818169ecfdbaa43cfc07f4e33ef4c",
      "name": "new_logo.png"
    }
  ]
}
```

*Note*: Uploads can only have one attachment, despite the json blob accepting plural `attachments`. This is for consistency across the other endpoints that accept attachments. Hit the endpoint multiple times if you need to create multiple uploads.


Get upload
----------

* `GET /projects/1/upload/2.json` will show the content, comments, and attachments for this upload.

Each attachment blob includes the `url` parameter, which you can make a
`GET` request (with authentication) in order to directly download the attachment.

```json
{
  "created_at": "2012-03-27T22:48:49-04:00",
  "updated_at": "2012-03-28T11:36:10-04:00",
  "content": "Hi there!",
  "attachments": [
    {
      "key": "40b8a84cb1a30dbe04457dc99e094b6299deea41",
      "name": "bearwave.gif",
      "byte_size": 508254,
      "content_type": "image/gif",
      "created_at": "2012-03-27T22:48:49-04:00",
      "url": "https://basecamp.com/1111/api/v1/projects/2222/attachments/3333/40b8a84cb1a30dbe04457dc99e094b6299deea41/original/bearwave.gif",
      "creator": {
        "id": 73,
        "name": "Nick Quaranto"
      }
    }
  ],
  "comments": [
    {
      "id": 5566323,
      "content": "Testing a comment",
      "created_at": "2012-03-28T11:36:10-04:00",
      "updated_at": "2012-03-28T11:36:10-04:00",
      "attachments": [],
      "creator": {
        "id": 73,
        "name": "Nick Quaranto"
      }
    }
  ]
}
```


Get uploads
-----------

* `GET /projects/1/uploads.json` will show the content, comments, and
attachments for this upload.

Each upload will have its associated `attachment` and `url`, along with any
`comments` on it.

We will return 50 uploads per page. If the
result set has 50 uploads, it's your responsibility to check the next page 
to see if there are any more topics -- you do this by adding `&page=2` to the 
query, then `&page=3` and so on.

```json
[
  {
    "created_at": "2012-03-27T22:48:49-04:00",
    "updated_at": "2012-03-28T11:36:10-04:00",
    "content": "Hi there!",
    "attachments": [
      {
        "key": "40b8a84cb1a30dbe04457dc99e094b6299deea41",
        "name": "bearwave.gif",
        "byte_size": 508254,
        "content_type": "image/gif",
        "created_at": "2012-03-27T22:48:49-04:00",
        "url": "https://basecamp.com/1111/api/v1/projects/2222/attachments/3333/40b8a84cb1a30dbe04457dc99e094b6299deea41/original/bearwave.gif",
        "creator": {
          "id": 73,
          "name": "Nick Quaranto"
        }
      }
    ],
    "comments": []
  },
  {
    "created_at": "2012-03-27T22:48:49-04:00",
    "updated_at": "2012-03-28T11:36:10-04:00",
    "content": "Here's the report",
    "attachments": [
      {
        "key": "773c74212f81f5c7d66917fb7236d5aece36c56a",
        "name": "report.pdf",
        "byte_size": 508254,
        "content_type": "application/pdf",
        "created_at": "2012-03-27T22:48:49-04:00",
        "url": "https://basecamp.com/1111/api/v1/projects/2222/attachments/4444/773c74212f81f5c7d66917fb7236d5aece36c56a/original/report.pdf",
        "creator": {
          "id": 73,
          "name": "Nick Quaranto"
        }
      }
    ],
    "comments": []
  }
]
```