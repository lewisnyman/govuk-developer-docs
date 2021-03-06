---
owner_slack: "#govuk-2ndline"
title: Monitoring screens
parent: "/manual.html"
layout: manual_layout
section: Tools
last_reviewed_on: 2018-04-12
review_in: 6 months
---

Most teams in GOV.UK have screens set up to show data about pull requests and releases.

Most often displayed are the [deploy lag radiator][deploy-lag] and the [fourth wall][fourth-wall].

[deploy-lag]: https://github.com/dsingleton/deploy-lag-radiator
[fourth-wall]: https://github.com/alphagov/fourth-wall

## Search screen

![Screen shot of the search screen](images/search-screen.png)

The [search screen][search-screen] displays live data from GOV.UK. It includes the number of people on GOV.UK, latest searches, trending and recent content. It's not publicly accessible because there's sometimes personal data in the latest searches.

[search-screen]: https://github.com/alphagov/govuk-display-screen

## 2nd line screen

![Photo of the 2nd line monitoring screen](images/monitoring.jpg)

There is a screen by the 2ndline desks.

The screen is a webpage running [David Singleton's Frame Splits][frame-splits] with 3 splits: production health, icinga alert summary per environment, recent deployments.

[frame-splits]: https://github.com/dsingleton/frame-splits

### Production health

This screen contains [a dashboard giving an overview of health for the
platform][production-health],
a list of upcoming releases, and a dashboard showing the alerts for each
environment.

This dashboard contains 2 graphs, one of origin 4xx and 5xx, and one of
edge 4xx and 5xx. It's worth keeping an eye on this and looking for any
anomalies, as this may indicate issues on production. It's likely due to
our caching behaviour that the top graph of origin errors will indicate
issues before they are visible in the second graph, and to end users.

[production-health]: https://grafana.publishing.service.gov.uk/#/dashboard/file/2ndline_health.json

### Troubleshooting

Sometimes the 'EDGE' graphs may disappear. These are obtained by the
[collectd-cdn plugin][collectd-cdn] on
`monitoring-1.management.production`. If the graphs disappear, they
should write errors to `/var/log/syslog`. They may look something like
this:

```sh
Nov 10 11:37:17 monitoring-1.management collectd[32764]: cdn_fastly plugin: Failed to query service: govuk

Nov 10 11:37:17 monitoring-1.management collectd[32764]: cdn_fastly plugin: Failed to query service: tldredirect

Nov 10 11:37:17 monitoring-1.management collectd[32764]: cdn_fastly plugin: Failed to query service: assets

Nov 10 11:37:17 monitoring-1.management collectd[32764]: cdn_fastly plugin: Failed to query service: redirector
```

If this happens, restarting collectd on the monitoring server may kick
things into life.

```
sudo service collectd restart
```

[collectd-cdn]: https://github.com/gds-operations/collectd-cdn

### Icinga alert summary per environment

This screen shows a summary of the critical and warning alerts for our
three environments (production, staging, integration) in a colour coded
box (red for criticals, yellow for warnings, purple for unknowns, green for no issues).

This is powered by [blinkenjs][blinkenjs] which is deployed to Heroku on
the [govuk-secondline-blinken][secondline] app. You must be in the office
or on the VPN to access the Icinga instances it gets its data from.

[blinkenjs]: https://github.com/alphagov/blinkenjs
[secondline]: https://govuk-secondline-blinken.herokuapp.com/blinken.html

### Recent deployments

This shows a list of recent app deployments to staging and production.
