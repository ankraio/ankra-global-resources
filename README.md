# Ankra's Global Resource Repository
This repository contains global resources that appear in the Ankra platform, such as helm registries and addons.

The Ankra platform syncs this repository continuously: whatever is on `master` is what every organisation on the platform sees. Changes land through pull requests — either opened by hand or opened automatically by the platform's publish flow.

## Helm registries (`helm_registries/`)

One YAML file per registry. These are the public chart registries every organisation can browse without connecting anything themselves.

```yaml
apiVersion: v1
kind: HTTPRegistry          # or OCIRegistry for oci:// chart paths
spec:
  name: prometheus-community
  url: https://prometheus-community.github.io/helm-charts
```

| Field | Notes |
| --- | --- |
| `apiVersion` | Always `v1`. |
| `kind` | `HTTPRegistry` for an `https://` chart index, `OCIRegistry` for an `oci://` URL. |
| `spec.name` | Registry name shown in the platform. |
| `spec.url` | Index URL (`https://`) or OCI chart path (`oci://`). |
| `spec.exclude_charts` | Optional list of chart names to hide (HTTP registries only). |

The filename is the kebab-case registry name and matches `spec.name`. Never add credentials — everything in this catalog must be publicly reachable.

## Addons (`addons/`)

One YAML file per addon manifest. An `Addon` is a curated catalog entry describing a software product (for example Grafana) independent of any single Helm chart; the platform links charts to it through the match rules. Once a manifest lands on `master`, the platform ingests it as a global addon definition and it appears in the add-on catalog of **every organisation**.

```yaml
apiVersion: v1
kind: Addon
spec:
  name: grafana                      # lowercase slug; also the filename (grafana.yaml)
  display_name: Grafana
  description: The open observability platform.
  website_url: https://grafana.com
  documentation_url: https://grafana.com/docs/
  icon_url: https://grafana.com/logo.svg
  category: Data Visualization
  match_chart_names:
    - grafana
  match_keywords: []
  match_home_url_prefixes:
    - https://grafana.com/oss
```

| Field | Notes |
| --- | --- |
| `apiVersion` | Always `v1`. |
| `kind` | `Addon`. |
| `spec.name` | Required. Lowercase slug (letters, digits, hyphens); the unique key across the catalog and the filename stem. |
| `spec.display_name` | Optional; defaults to the slug. |
| `spec.description`, `spec.website_url`, `spec.documentation_url`, `spec.icon_url`, `spec.category` | Optional metadata shown in the add-on catalog. |
| `spec.match_chart_names` | Optional list of exact chart names that resolve to this addon. |
| `spec.match_keywords` | Optional list of Chart.yaml keywords that resolve to this addon. |
| `spec.match_home_url_prefixes` | Optional list of Chart.yaml home-URL prefixes that resolve to this addon. |

Most `Addon` manifests are **published from the platform**: an organisation curates an addon definition in its own catalog and publishes it, which opens a pull request here on the requester's behalf. Merging the pull request is the review gate — once merged, the sync ingests the manifest, the publish request flips to *live*, and the addon becomes available to all organisations. Removing a manifest retires the global addon again (organisation-scoped copies are unaffected; an organisation's own definition with the same slug always shadows the global one).

---
#### [Website](https://ankra.ai) | [Docs](https://docs.ankra.ai) | [Slack](https://join.slack.com/t/ankra-community/shared_invite/zt-3a5rem8f8-cUho4epX2MoLT83bFf~VSA) | [LICENSE](LICENSE)
