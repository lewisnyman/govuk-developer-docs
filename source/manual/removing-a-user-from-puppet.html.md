---
owner_slack: "#govuk-2ndline"
title: Removing a user from Puppet
parent: "/manual.html"
layout: manual_layout
section: 2nd line
last_reviewed_on: 2018-05-18
review_in: 12 months
---

Removing a user from our infrastructure via Puppet is a 2 change process that
requires a deploy in the middle. The first change ensures that when puppet
runs the users home directory is removed, the second change removes the
user from puppet itself. If the user is just removed from puppet their files
will remain on our servers forever more.

1. First find the user manifest in: [modules/users/manifests][manifest-path].
1. Add an entry to the govuk_user class of `ensure => absent,`. Here is an
   [example][absent-example].
1. Once this has been raised as a PR and merged, deploy puppet to all
   environments.
1. Create another PR for puppet that:
  - Removes the user manifest file
  - Removes the user from [Integration users][integration-users]
  - Removes the user from [CI users][ci-users]
1. Create a PR in [GOV.UK secrets][govuk-secrets] that:
  - Remove the user from [production hieradata][production-hieradata]
  - Remove the user from [AWS production hieradata][aws-production-hieradata]
1. Once these have been merged, deploy puppet again to all environments.

[manifest-path]: https://github.com/alphagov/govuk-puppet/tree/master/modules/users/manifests
[absent-example]: https://github.com/alphagov/govuk-puppet/commit/0757bad41ed577f15c7f5d9e508f55e78c612ddb
[integration-users]: https://github.com/alphagov/govuk-puppet/blob/master/hieradata_aws/integration.yaml
[ci-users]: https://github.com/alphagov/govuk-puppet/blob/master/hieradata/integration.yaml
[govuk-secrets]: https://github.com/alphagov/govuk-secrets
[production-hieradata]: https://github.com/alphagov/govuk-secrets/tree/master/puppet/hieradata
[aws-production-hieradata]: https://github.com/alphagov/govuk-secrets/tree/master/puppet_aws/hieradata
