# pmosuite releases

Public release channel for **pmosuite** installations. It holds **release information
only**: signed manifests and changelogs. No source code, images, customer data or
secrets ever go here. The product code is private.

Instances read the manifest without credentials:

```
https://raw.githubusercontent.com/rdietsch/pmosuite-releases/main/stable/manifest.json
https://raw.githubusercontent.com/rdietsch/pmosuite-releases/main/stable/manifest.json.sig
```

Trust comes from the **Ed25519 signature**, not from this repository. Every instance
has the public key built in and ignores any manifest whose signature does not verify.

## Layout

```
stable/manifest.json        current stable release manifest
stable/manifest.json.sig    Ed25519 signature of manifest.json (base64)
changelogs/<module>/<version>.md
```

## Manifest format (schema 1)

```json
{
  "schema": 1,
  "channel": "stable",
  "released_at": "2026-12-01T15:00:00Z",
  "modules": [
    {
      "id": "estimator",
      "version": "1.10.0",
      "images": {
        "api": "ghcr.io/rdietsch/pmosuite-estimator-api@sha256:<digest>",
        "web": "ghcr.io/rdietsch/pmosuite-estimator-web@sha256:<digest>"
      },
      "requires": { "core": ">=1.2.0" },
      "min_upgrade_from": "1.9.0",
      "migrations": ["0042_company_profile_link"],
      "severity": "feature",
      "changelog": "changelogs/estimator/1.10.0.md"
    }
  ]
}
```

- `images` are pinned **by digest**. Tags are never trusted.
- `severity` is `feature` or `security`. Security releases are highlighted to
  administrators.
- `min_upgrade_from` is the lowest installed version that may upgrade in one step.
- `requires` lists the minimum versions of other modules (usually Core).

Manifests are published by the `pmosuite` release workflow and are never edited by hand.
See `docs/RELEASE_AND_UPGRADE_PLAN.md` in the product repository.
