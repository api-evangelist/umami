# Umami

Umami is an open source, privacy-first web analytics platform that provides website traffic insights without cookies or personal data collection, serving as a simple and fast alternative to Google Analytics. The Umami API provides full programmatic access to analytics data, website management, session tracking, event data, and team collaboration features.

**Run: [Capabilities Using Naftiko](https://naftiko.com)**

**URL:** [https://raw.githubusercontent.com/api-evangelist/umami/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/umami/refs/heads/main/apis.yml)

## APIs

### Umami API

The Umami API provides programmatic access to website analytics data including pageviews, sessions, events, and metrics. Self-hosted instances use JWT bearer tokens; Umami Cloud uses API key authentication.

**Human URL:** [https://umami.is](https://umami.is)

**Base URL:** https://api.umami.is

**Tags:** Open Source, Privacy, Tracking API, Web Analytics

**Properties:**

| Type | URL |
|------|-----|
| Documentation | [https://umami.is/docs](https://umami.is/docs) |
| Getting Started | [https://umami.is/docs/getting-started](https://umami.is/docs/getting-started) |
| API Documentation | [https://umami.is/docs/api](https://umami.is/docs/api) |
| GitHub | [https://github.com/umami-software/umami](https://github.com/umami-software/umami) |
| OpenAPI | [openapi/umami-openapi.yml](openapi/umami-openapi.yml) |

## Common Properties

| Type | URL |
|------|-----|
| Website | [https://umami.is](https://umami.is) |
| Documentation | [https://umami.is/docs](https://umami.is/docs) |
| Blog | [https://umami.is/blog](https://umami.is/blog) |
| Pricing | [https://umami.is/pricing](https://umami.is/pricing) |
| GitHub | [https://github.com/umami-software/umami](https://github.com/umami-software/umami) |
| Login | [https://cloud.umami.is](https://cloud.umami.is) |
| Signup | [https://cloud.umami.is](https://cloud.umami.is) |
| Self-Hosting | [https://umami.is/docs/install](https://umami.is/docs/install) |
| Support | [https://umami.is/docs/support](https://umami.is/docs/support) |

## Features

| Name | Description |
|------|-------------|
| Privacy-First Analytics | Tracks website traffic without cookies or personal data, fully GDPR compliant without consent banners. |
| Real-Time Data | Live visitor counts and real-time pageview tracking for immediate traffic insights. |
| Custom Events | Track custom user interactions and conversions with a simple JavaScript API. |
| Team Collaboration | Share analytics access across teams with role-based access control. |
| Self-Hosting Support | Deploy on your own infrastructure for complete data ownership and control. |
| Open Source | MIT-licensed open source software with active community development and full transparency. |
| Multi-Site Support | Manage and analyze multiple websites from a single Umami instance. |
| API Access | Full REST API for programmatic access to all analytics data and management functions. |

## Use Cases

| Name | Description |
|------|-------------|
| Website Performance Monitoring | Track pageviews, unique visitors, bounce rates, and session duration for website optimization. |
| Privacy-Compliant Analytics | Replace Google Analytics with a cookieless solution that requires no consent banners under GDPR. |
| Marketing Analytics | Analyze traffic sources, referrers, and UTM campaign data to measure marketing effectiveness. |
| Custom Event Tracking | Track button clicks, form submissions, and custom conversions using the Umami event API. |
| Developer Dashboards | Build custom analytics dashboards using the REST API to display site metrics in your own apps. |
| Multi-Tenant Analytics | Provide analytics access to multiple clients or teams with shared infrastructure and access controls. |

## Integrations

| Name | Description |
|------|-------------|
| Next.js | First-class integration with Next.js via the @umami/nextjs package for page view tracking. |
| WordPress | Track WordPress sites by adding the Umami tracking script via plugin or manual installation. |
| Vercel | Deploy Umami on Vercel with a one-click deployment for managed self-hosting. |
| Docker | Run Umami in any environment using the official Docker container image. |
| Cloudflare | Deploy tracking scripts behind Cloudflare for performance and abuse prevention. |

## Solutions

| Name | Description |
|------|-------------|
| Umami Cloud | Hosted Umami instance at cloud.umami.is with managed infrastructure and API key authentication. |
| Umami Self-Hosted | Run your own Umami instance on any infrastructure with full data ownership and JWT authentication. |

## Artifacts

### OpenAPI Specs

| Name | URL |
|------|-----|
| Umami Analytics API | [openapi/umami-openapi.yml](openapi/umami-openapi.yml) |

### JSON Schema

| Name | URL |
|------|-----|
| Website | [json-schema/umami-website-schema.json](json-schema/umami-website-schema.json) |
| Website Stats | [json-schema/umami-website-stats-schema.json](json-schema/umami-website-stats-schema.json) |
| Pageview Data | [json-schema/umami-pageview-data-schema.json](json-schema/umami-pageview-data-schema.json) |
| Metric | [json-schema/umami-metric-schema.json](json-schema/umami-metric-schema.json) |
| Active Visitors | [json-schema/umami-active-visitors-schema.json](json-schema/umami-active-visitors-schema.json) |
| Session | [json-schema/umami-session-schema.json](json-schema/umami-session-schema.json) |
| Session Stats | [json-schema/umami-session-stats-schema.json](json-schema/umami-session-stats-schema.json) |
| User | [json-schema/umami-user-schema.json](json-schema/umami-user-schema.json) |
| Team | [json-schema/umami-team-schema.json](json-schema/umami-team-schema.json) |
| Team Member | [json-schema/umami-team-member-schema.json](json-schema/umami-team-member-schema.json) |
| Login Request | [json-schema/umami-login-request-schema.json](json-schema/umami-login-request-schema.json) |

### JSON-LD Contexts

| Name | URL |
|------|-----|
| Umami Context | [json-ld/umami-context.jsonld](json-ld/umami-context.jsonld) |

## Capabilities

### Shared Definitions

| Name | URL |
|------|-----|
| Umami Analytics | [capabilities/shared/umami.yaml](capabilities/shared/umami.yaml) |

### Workflow Compositions

| Name | Description | URL |
|------|-------------|-----|
| Web Analytics | Track website performance and visitor behavior | [capabilities/web-analytics.yaml](capabilities/web-analytics.yaml) |

## Vocabulary

| Name | URL |
|------|-----|
| Umami API Vocabulary | [vocabulary/umami-vocabulary.yaml](vocabulary/umami-vocabulary.yaml) |

## Rules

| Name | URL |
|------|-----|
| Umami Spectral Rules | [rules/umami-spectral-rules.yml](rules/umami-spectral-rules.yml) |

## Maintainers

**FN:** Kin Lane

**Email:** kin@apievangelist.com
