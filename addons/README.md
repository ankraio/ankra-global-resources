# Addons

`kind: Addon` manifests live here, one YAML file per addon, named after the slug in `spec.name` (`grafana.yaml` → `name: grafana`). See the repository README for the full schema.

Manifests in this directory are ingested by the Ankra platform and appear in the add-on catalog of every organisation. Most files here arrive through the platform's publish flow, which opens a pull request on behalf of the publishing organisation — merging that pull request is the review gate.

This README is not a manifest; the sync only reads `.yaml`/`.yml` files.
