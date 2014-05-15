# Ahoy

:fire: A foundation of knowledge and libraries for awesome analytics

Best practices for:

- tracking visits, events, emails, and referrals
- scaling storage
- funnels, cohorts, and LTV calculations
- experiments like split tests
- and much more

Designed to work with any progamming language and any device.

Never build an analytics platform from scratch again.

This is a work in progress, built for the open-source community.  If you have great practices, articles, or videos, [please share](https://github.com/ankane/ahoy_guide/issues/new).

:warning: Anything below is scattered and probably makes no sense

## Outline

- Intro
- The Perfect Platform
- People, Not Page Views
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
- HTTP Spec

## Web

There are two ways to tell where a visitor has come from:

- the `Referer` header
- query parameters, like `utm_source`

When a user clicks on a link, most browsers set the `Referer` header with the URL of the previous page.  From this, you can extract:

- the exact page
- search keywords

**TODO:** Explain how different browsers handle redirects

## iOS

- [Link to App Store](http://stackoverflow.com/a/2337601/1177228)
- [Smart App Banners](https://developer.apple.com/library/ios/documentation/AppleApplications/Reference/SafariWebContent/PromotingAppswithAppBanners/PromotingAppswithAppBanners.html)

## Android

- [Link to Play Store](http://developer.android.com/distribute/googleplay/promote/linking.html)

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
