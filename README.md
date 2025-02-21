# postmark-go

[![GoDoc](https://godoc.org/github.com/mattevans/postmark-go?status.svg)](https://godoc.org/github.com/mattevans/postmark-go)
[![Build Status](https://travis-ci.org/mattevans/postmark-go.svg?branch=master)](https://travis-ci.org/mattevans/postmark-go)
[![license](https://img.shields.io/github/license/mashape/apistatus.svg)](https://github.com/mattevans/postmark-go/blob/master/LICENSE)

postmark-go is a [Go](http://golang.org) client library for accessing the Postmark API (http://developer.postmarkapp.com/).

This is an unofficial library that is not affiliated with [Postmark](http://postmarkapp.com). Official libraries are available
[here](http://developer.postmarkapp.com/developer-official-libs.html).

Installation
-----------------

`go get -u github.com/rhomber/postmark-go`

Setup
-----------------

You'll need to pass an `SERVER_API_TOKEN` when initializing the client. This token can be
found under the 'Credentials' tab of your Postmark server. More info [here](http://developer.postmarkapp.com/developer-api-overview.html#authentication).

Authentication
-------------
```go
auth := &http.Client{
  Transport: &postmark.AuthTransport{Token: "SERVER_API_TOKEN"},
}
client := postmark.NewClient(auth)
```

Example usage (with Template)
-------------

```go
emailReq := &postmark.Email{
  From:       "mail@company.com",
  To:         "jack@sparrow.com",
  TemplateID: 123456,
  TemplateModel: map[string]interface{}{
    "name": "Jack",
    "action_url": "http://click.company.com/welcome",
  },
  Tag:        "onboarding",
  TrackOpens: true,
}

email, response, err := client.Email.Send(emailReq)
if err != nil {
  return err
}
```

Example usage (with HtmlBody)
-------------

```go
emailReq := &postmark.Email{
  From:       "mail@company.com",
  To:         "jack@sparrow.com",
  Subject:    "My Test Email",
  HtmlBody:   "<html><body><strong>Hello</strong> dear Postmark user.</body></html>",
  TextBody:   "Hello dear Postmark user",
  Tag:        "onboarding",
  TrackOpens: true,
}

email, response, err := client.Email.Send(emailReq)
if err != nil {
  return err
}
```

What's Implemented?
----------------

At the moment only a handful of the more common endpoints have been implemented. Open an
issue (or PR) if you required something that's missing.

- Send Email - [API Docs](http://developer.postmarkapp.com/developer-api-email.html#send-email) | [Example](examples/send-email/main.go)
- Send Email & Attachment - [API Docs](http://developer.postmarkapp.com/developer-api-email.html#send-email) | [Example](examples/send-email-attachment/main.go)
- Batch Emails - [API Docs](http://developer.postmarkapp.com/developer-api-email.html#batch-emails) | [Example](examples/batch-emails/main.go)
- Get Delivery Stats - [API Docs](http://developer.postmarkapp.com/developer-api-bounce.html#delivery-stats) | [Example](examples/bounce/main.go)
- Get Bounces - [API Docs](http://developer.postmarkapp.com/developer-api-bounce.html#bounces) | [Example](examples/bounce/main.go)
- Get Single Bounce - [API Docs](https://postmarkapp.com/developer/api/bounce-api#single-bounce)
- Get Bounce Dump - [API Docs](https://postmarkapp.com/developer/api/bounce-api#bounce-dump)
- Activate a Bounce - [API Docs](https://postmarkapp.com/developer/api/bounce-api#activate-bounce)
- Get Bounced Tags - [API Docs](https://postmarkapp.com/developer/api/bounce-api#bounced-tags)
- List Templates - [API Docs](https://postmarkapp.com/developer/api/templates-api#list-templates)
- Get Single Template - [API Docs](https://postmarkapp.com/developer/api/templates-api#get-template)

Thanks &amp; Acknowledgements :ok_hand:
----------------

The packages's architecture is adapted from
[go-github](https://github.com/google/go-github), created by [Will
Norris](https://github.com/willnorris). :beers:
