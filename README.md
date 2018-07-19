# Ahoy

:fire: A foundation of knowledge and libraries for solid analytics

Best practices for:

- tracking visits, events, emails, and referrals
- scaling storage
- funnels, cohorts, and LTV calculations
- experiments like split tests
- and much more

Designed to work with any progamming language and any device.

Never build an analytics platform from scratch again.

This is a work in progress, built for the open-source community.  If you have great practices, articles, or videos, [please share](https://github.com/ankane/ahoy_guide/issues/new).

:warning: Everything below is scattered and probably makes no sense

## Outline

- Intro
- The Perfect Platform
- People
- Qualitative Feedback
- Funnels
- Split Tests
- Web
  - Landing page
    - Acquisition
    - Funnels
    - Experiments
  - Product
    - Same as above
- iOS
  - Same as above
- Android
  - Same as above
- Emails
- Referrals
- Load Times
- Storage
- Privacy
- HTTP Spec

## Entities

### Visitor

Visitors have properties

### Visit (session)

A visit belongs to a visitor.  Users can have visits through authentication events.

A visit provides:

- a way to attach events that happened before sign in
- how someone arrived at the website or app
- a rough idea of location for local services
- information about the technology (browser, screen size, OS version), which you can use this to tailor your product to users

### Event

Events are actions a person performs.

Events have a visit (from which you can get the visitor) possibly a user.

### User

Users are authenticated visitors

Users have properties

## Technical Notes

References to “unique id” in this guide refer to a [UUID](https://en.wikipedia.org/wiki/Universally_unique_identifier). These should be stored as a 128-bit number, not a string (except for logs).

Visitors and visits should be given a unique id *on the server* for web apps and stored in cookies.

## People

Two users viewing a page is different than one user viewing it twice.  This is critical for funnels and experiments.

There are two type of people:

- Users - authenticated
- Visitors - anonymous

## Qualitative Feedback

- [Peek](https://peek.usertesting.com/) - free!
- [Session recording](https://www.inspectlet.com)

## Funnels

Intent is key

Segment by:

- mobile vs desktop
- channel
- experiment

## Split Tests

Commonly called split tests or A/B tests

- [How Not To Run An A/B Test](https://www.evanmiller.org/how-not-to-run-an-ab-test.html)
- [Why multi-armed bandit algorithm is not “better” than A/B testing](https://visualwebsiteoptimizer.com/split-testing-blog/multi-armed-bandit-algorithm/) - time is $$$
- [Sample Size Calculator](https://www.evanmiller.org/ab-testing/sample-size.html)
- [Experiments at Airbnb](https://nerds.airbnb.com/experiments-at-airbnb/)
- [Bayesian AB Testing](https://developers.lyst.com/data/2014/05/10/bayesian-ab-testing/)

Start with big changes (exploration), not button colors

Use same tracking as events for conversions

Segment key funnels by experiment variation

Split tests are special properties attached to visitors or users.

Variation membership should be stored.  They should be looked up by user id (first), then visitor id.

If there is a user but only a split test variation for the visitor, the user should use the same variation.

It should be easy for developers to test variations.

## Web

### Visits

There are two ways to tell where a visitor has come from:

- the `Referer` header
- query parameters, like `utm_source`

When a user clicks on a link, most browsers set the `Referer` header with the URL of the previous page.  From this, you can extract:

- the page
- search keywords - thanks to [great libraries](https://github.com/snowplow/referer-parser)

**TODO:** Explain how [different browsers handle redirects](https://stackoverflow.com/questions/2158283/will-a-302-redirect-maintain-the-referer-string) and note about [HTTPS -> HTTP](https://webmasters.stackexchange.com/questions/47405/how-can-i-pass-referrer-header-from-my-https-domain-to-http-domains)

There are a few things you can calculate about the visitor:

- estimated location from IP address
- browser, OS, and device model from user agent

Client libraries have access to more information, like:

- screen size
- pixel density (retina) - does this really matter?

Be sure to exclude bots from your metrics - some like Googlebot run JavaScript.

### Landing Page

The landing page is one of the most important pages of your website.

When an unauthenticated visitor lands on your site, there are a few things that could happen:

- register (success!)
- sign in
- bounce

**TODO:** Note about multiple authentication strategies (email, [Facebook](https://developers.facebook.com/docs/facebook-login/permissions/v2.0#optimizing), Google, etc)

**Best practice:** For third-party services, ask for the miminum number of permissions needed

#### Authenticated Visitors

If a visitor is authenticated, do **not** show them the landing page with a “Customer Login” link.  Drop them right into your product.

**Best practice:** Keep users signed in between visits - unless you run a banking website of course

## iOS

- [Link to App Store](https://stackoverflow.com/a/2337601/1177228)
- [Smart App Banners](https://developer.apple.com/library/ios/documentation/AppleApplications/Reference/SafariWebContent/PromotingAppswithAppBanners/PromotingAppswithAppBanners.html)

## Android

- [Link to Play Store](https://developer.android.com/distribute/googleplay/promote/linking.html)
- [Link with referrer](https://developers.google.com/analytics/devguides/collection/android/v2/campaigns#google-play-implement)

## Storage

Start simple and scale as needed - “premature optimization is the root of all evil”

- logs (not queryable)
  - backup to [S3]
- database
  - PostgreSQL, Redis, Logstash, Fluentd
- distributed data stores
  - Hadoop, Cassandra, [Amazon Redshift]

[not open source]

**TODO:** Recommendations for starting, scaling, and scaling again

## Email

Give each message a unique id.

- Track opens and clicks
- One-click unsubscribe - don't make users confirm or sign in
- Give the option to resubscribe or manage other lists
- Use your own unsubscribe link rather than rely on your email server

**TODO:** What emails to send, when to send them

Experiment with the message, time of day, triggers

## Referrals

How to track referrals correctly

## Load Times

[Latency matters](http://highscalability.com/latency-everywhere-and-it-costs-you-sales-how-crush-it)

- Amazon - every 100ms cost them 1% in sales
- Google - an extra half second dropped search traffic by 20%

No one ever wants a slow service

How to instrument load times: Give each request a unique id and record the time the:

- request starts
- server completes request
- JavaScript says ready

Tricks:

- Limit redirects

Acceptable tresholds

[Better SEO?](https://unbounce.com/conversion-rate-optimization/a-fast-web-site-increases-conversions/)

## Privacy

Things not to do

- [supercookies](https://mashable.com/2011/09/02/supercookies-internet-privacy/)
- [device fingerprinting](https://panopticlick.eff.org/)

Section on Do Not Track

## HTTP Spec

### Visits

A `POST` request is sent with:

- visit_token
- visitor_token
- referrer
- landing_page

The server can capture:

- ip
- user_agent
- user - from app authentication

And calculate things like:

- referring_domain and search_keyword from referrer
- utm_source, utm_medium, utm_term, utm_content, and utm_campaign from landing_page
- city, region, and country from ip
- browser, os, and device_type from user_agent

### Events

A `POST` request is sent with:

- name
- properties
- time

The server can capture:

- visit_token - from cookies
- user - from app authentication

As a precaution, the server should reject times that do not match:

```
1 minute ago < time <= now
```

## Libraries

### Client Tracking

Works with any backend

- [JavaScript](https://github.com/ankane/ahoy.js)
- iOS (soon)
- Android (soon)

### Server Tracking + Backend

- [Ruby](https://github.com/ankane/ahoy)
- Others (help make this possible)

### Email

- [Ruby](https://github.com/ankane/ahoy_email)
- Others (help make this possible)

### Experiments, Funnels, Cohorts, More

- Coming soon

## Great Content

- [Startup Metrics for Pirates](https://www.youtube.com/watch?v=irjgfW0BIrw)
