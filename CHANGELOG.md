# Changelog

## v0.38 - 2019-07-03

- [CHANGE] Remove old Prometheus v1 rule format #148
- [IMPROVEMENT] Support selecting probe module for blackbox monitoring #154
- [IMPROVEMENT] When listing rules, show rule specific labels #153

Setting for `url_writer` changes to `prometheus:blackbox`

See `promgen/tests/examples/promgen.yml`

## v0.37 - 2019-06-17

- [INTERNAL] Upgrade to Django 2.2.x #152
- [BUGFIX] Fix rule rendering table on host detail page #151
- [BUGFIX] Fix rendering errors with Silence/Alert dropdowns #150
- [BUGFIX] Fix search page rendering #149
- [INTERNAL] Move remaining celery tasks to tasks.py #139

## v0.36 - 2019-05-29

- [CHANGE] Refactor shard assignment from service to Project #147

## v0.35 - 2019-04-16

- [BUGFIX] Refactor rule validation to take into account labels and annotations #144
- [BUGFIX] Move promtool validation into RuleForm.clean #142
- [INTERNAL] Update various dependencies #143
- [IMPROVEMENT] Add proxy for /api/v1/labels #140
- [IMPROVEMENT] Use django-filters for api filtering #126

## v0.34 - 2019-03-13

This release adds support for filters for notifications. Alerts can be filtered so that different notification targets can be restricted by label. For example, sending urgent messages directly to LINE but sending less urgent messages to be logged to slack

- [INTERNAL] Refactor how we proxy errors #136
- [BUGFIX] Fix arguments for create/filter/get_or_create #137
- [BUGFIX] Fix filterActiveSilences condition #135
- [FEATURE] Implement sender whitelist filters #132
- [INTERNAL] Refactor Breadcrumb from template to simple_tag #133
- [INTERNAL] Refactor manager class and validators #131

## v0.33 - 2019-03-01

- [BUGFIX] Fix computed labels #129
- [BUGFIX] Fix validation of silence duration #130
- [IMPROVEMENT] Add logo + favicon #128
- [IMPROVEMENT] Various page query optimizations #127

## v0.32 - 2019-02-22

- [IMPROVEMENT] Add button to silence a single alert #125
- [CHANGES] Rewritten display of alerts and silences to use vuejs #124
- [INTERNAL] Split proxy views to their own module #123
- [IMPROVEMENT] Add default notifier when subscribing #122
- [BUGFIX] Validate URL field for notifications #121
- [ENHANCEMENT] Improvements to Admin Alert view #116

## v0.31 - 2018-12-05

- [BUGFIX] Ensure that invalid annotations are caught #113
- [CHANGES] Bump Python to 3.6 and use Pipfile for easier development onboarding #109
- [CHANGES] Update Django version #107
- [ENHANCEMENT] Add 'owner' field for Projects and Services #111
- [ENHANCEMENT] Initial read-only API provided by django-rest-framework #112
- [ENHANCEMENT] New homepage to show subscribed services #105
- [ENHANCEMENT] Refactor to support Project and Site rule exports #104
- [ENHANCEMENT] Support adding exporter defaults from admin gui #114

## v0.30 - 2018-10-02

- [BUGFIX] Fix for missing promtool validation error #103
- [BUGFIX] Fix reference to self.get_object() in RuleToggle #99
- [CHANGE] Refactor notifications queue for audibility and reliability #94
- [CHANGE] Add prune-alerts command #106

This version refactors the alert sender queue to better separate responsibility between components and to provide a more auditable record of received alerts

## v0.29 - 2018-08-28

- [IMPROVEMENT] Implement Django permissions for Rule editor #96

This first version implements permissions primarily for common, shared rules to reduce the chance of accidentally modifying the parent rule. In the future, permissions will be applied to more objects. Migrations will automatically create a Default group in the Django admin.

## v0.28 - 2018-08-01

- [CHANGE] Update to Django 2.0 #82
- [CHANGES] Re-add link to shard #91
- [IMPROVEMENT] Add /api/v1/query proxy #88
- [IMPROVEMENT] Add help text describing job label usage #93
- [IMPROVEMENT] RuleTest will test against all Prometheus instances #90

## v0.27 - 2018-06-14

- [BUGFIX] Fix parsing error with silence end date
- [BUGFIX] Fix webhook POST data
- [CHANGE] Update Celery version for bug fix
- [IMPROVEMENT] Cleanup some page headers to make navigation easier
- [IMPROVEMENT] Simple proxy page for `/graph` view
- [IMPROVEMENT] Speed up services page with prefetch_related_objects
- [IMPROVEMENT] Support entering comma separated list of hosts
- [IMPROVEMENT] Support for disabling a shard (disables adding new services)
- [IMPROVEMENT] Use css grid to improve readability of long pages
- [IMPROVEMENT] Warn if parent rull is missing macro

## v0.26 - 2018-03-12

- [BUGFIX] Allow STATIC_ROOT to be configured via Environment
- [BUGFIX] Fix bytes/string mismatch with outputing rules
- [BUGFIX] Fix error message when registering a duplicate Service
- [BUGFIX] Fix missing list of exporters on host detail page
- [BUGFIX] Fix typo in view names
- [CHANGE] Bootstrap default admin user when starting Promgen with an empty database
- [CHANGE] Change handling of DEBUG variable. Now requires that a value is set in /path/DEBUG instead of just the file existing
- [CHANGE] CONFIG_DIR renamed to PROMGEN_CONFIG_DIR
- [CHANGE] Temporarily disable typeahead plugin pending refactoring
- [IMPROVEMENT] Added Slack notifier
- [IMPROVEMENT] Ship promtool in docker image
- [IMPROVEMENT] Updates to documentation

## v0.25

- [BUGFIX] Fix formatting on yaml rules file to output as unicode
- [BUGFIX] Minor optimization for admin page
- [BUGFIX] Include missing package data
- [BUGFIX] Fix double slash with exporter path test
- [IMPROVEMENT] Add default project/service label when creating new rule
- [IMPROVEMENT] Show disabled exporters as light grey
- [IMPROVEMENT] Show confirmation when toggling exporter

## v0.24

- [BUGFIX] Minor permissions fix with atomic_write method
- [IMPROVEMENT] Remove special 'default' group and support Site as a parent for shared rules
- [IMPROVEMENT] Add support for Prometheus 2.x rule format (yaml rules)
- [IMPROVEMENT] Fix getting started documentation with Docker
- [BUGFIX] Add test cases for alert manager silence
- [BUGFIX] Switch to Django's builtin LoginRequiredMixin to handle auth
- [CHANGE] remove 'rule_writer' stanza from config and mere into 'prometheus' stanza

```yaml
# These are used for Promgen to automatically trigger a reload on target changes
prometheus:
  url: http://prometheus:9090/
  version: 2
  # Promtool was moved into the prometheus stanza. To skip validation this can
  # be set to the path of the 'true' binary
  promtool: /usr/local/bin/promtool
  # promtool: /usr/bin/true # to disable
  # Output rule configuration to this path
  rules: /etc/prometheus/promgen.rule.yml
  # Or remove .yml for when working with Prometheus 1.x
  # rules: /etc/prometheus/promgen.rule
# The old rule_writer format is now unused and can be deleted
#rule_writer:
#  path: /etc/prometheus/promgen.rule
#  promtool_path: /usr/local/bin/promtool
```

## v0.23

- [BUGFIX] Fix double escape regression
- [BUGFIX] Fix host silence tag on search page
- [IMPROVEMENT] Minor query speedup for Admin pages
- [IMPROVEMENT] Support 'User' notifications. User's can be set as a notification target and Users can configure their own subscriptions from a profile page

## v0.22

- [IMPROVEMENT] Add test button to test exporters from Promgen
- [IMPROVEMENT] Further simplify notifications by dedeuplicating labels/annotations
- [BUGFIX] Ensure we properly raise Exceptions if a sender fails
- [IMPROVEMENT] Urlize comments in Silence list (for linking to bug tracker)
- [IMPROVEMENT] Add description field to rules so developers can add additional context
- [IMPROVEMENT] Update Django to 1.11

## v0.21

- [BUGFIX] Fix headers for Prometheus Proxy
- [IMPROVEMENT] Add description field to Project and Service for adding additional context information
- [IMPROVEMENT] Add filters to audit log, so that history can be filtered by object
- [IMPROVEMENT] Add owner field to Notifier object for security auditing purposes

## v0.20

- [BUGFIX] Return upstream Prometheus error when proxying requests
- [IMPROVEMENT] Notifications are grouped as they are received from Alert Manager
- [IMPROVEMENT] Refactor Alerts to be rendered mostly client side
- [IMPROVEMENT] Refactor search page to accept searches from Grafana links
- [IMPROVEMENT] Refactored Farm buttons to better indicate local (promgen) or remote

## Rewrite

- Rewrote in Django and Celery

  - Use Django to take advantage of more robust ORM and admin site.
  - Use Celery for writing configuration files to Prometheus nodes. This allows us to decouple the Promgen webui from the Prometheus nodes themselves and gives us better auditing when writing out the configuration files.
  - Support for using Celery to send out notifications instead of blocking a single thread
  - Optionally use Sentry for debugging exceptions from within Promgen
  - Take advantage of Python's setuptools endpoints for easier plugin management

- [IMPROVEMENT] Senders can be set for both Projects and Services
- [IMPROVEMENT] Improved rule editor

  - More easily edit labels and annotations on rules
  - Button to test query against Prometheus to help testing

- [IMPROVEMENT] Shard support. Supports assigning services to different shards for capacity management
- [IMPROVEMENT] Better support for writing sender plugins by using Python's setuptools framework
- [IMPROVEMENT] Blackbox exporter support by adding URLs to Project pages
- [IMPROVEMENT] Support toggling Rules and Exporters
