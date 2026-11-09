# Privacy Policy

The privacy and data-processing side of an API, read off the provider's own privacy policy and DPA: its role (controller or processor), the data categories and purposes, retention, residency, transfer mechanisms, subprocessors, how to get a DPA, how deletion works, and whether data is sold or shared.

This is the schema behind the [Privacy Policy property](https://apicommons.org/common/privacy-policy/)
in [API Commons](https://apicommons.org).

## Why it matters

API Commons was started to make the business side of API onboarding machine-readable. Finding an API is
the easy part; deciding whether you are allowed to use it — for this purpose, in this country, with this
data, by this kind of client — still means a person reading legal pages. When the answer is a document a
crawler, a catalog or an agent can read, discovery and onboarding can be automated end to end, and a
consumer can compare providers on the terms rather than on the marketing.

## Artifacts

- **[privacy-policy-json-schema.yml](privacy-policy-json-schema.yml)** — the JSON Schema (2020-12).
- **[privacy-policy-example-1.yml](privacy-policy-example-1.yml)** — a standalone document, as written to `all/<slug>/privacy/<slug>-privacy-policy.yml`.
- **[privacy-policy-example-2.yml](privacy-policy-example-2.yml)** — the same document inside the `PrivacyPolicy` apis.yml property envelope.
- **[validate.py](validate.py)** — validates any document against the schema.

Both examples describe **Example Weather API**, a fictional provider. The clauses are illustrative and are
not the terms of any real company.

## Fields

Every document carries the common envelope:

```yaml
privacy_policy: 0.1.0
provider: Example Weather API
aid: example-weather-api
url: <the page it was read from>
source_date: YYYY-MM-DD
x-apis-io-derivation:
  synthetic: false
  method: harvested
  read: YYYY-MM-DD
  tool: engine/business/harvest-business.py
  model: <model id>
```

and then:

| Field | Shape | Meaning |
|---|---|---|
| `role` | `{value, text}` | controller / processor / both / unstated |
| `data_categories[]` | `{category, text}` | W3C DPV top-level category: identifying, contact, financial, location, behavioral, device, health, biometric, communication, professional, demographic, other |
| `purposes[]` | `{purpose, text}` | Purposes of processing |
| `retention[]` | `{scope, period, text}` | Retention periods, copied as written |
| `residency[]` | `{region, text}` | Where data lives |
| `transfers[]` | `{mechanism, text}` | SCC / adequacy / DPF / BCR / other |
| `subprocessors` | `{url, text}` | The subprocessor list |
| `dpa` | `{available, url, text}` | Data processing agreement |
| `deletion` | `{route, text}` | How to get data deleted |
| `rights[]` | `{right, text}` | Data-subject rights |
| `sale_or_sharing` | `{stance, text}` | Whether data is sold or shared |

## The stance enum

Wherever a permission is described: `allowed | prohibited | conditional | unstated`. Every stance other
than `unstated` must carry a verbatim `text` — the schema enforces it. `unstated` means the page says
nothing, which is a valid, recorded result and is never filled in with a guess. Periods ("30 days",
"12 months") are copied as written and never normalized.

## Provenance: verbatim text, never synthetic

Only documents read from the provider's own page are published. There is no template fill. The page is
fetched by code; a model only transcribes rows from it; every row's `text` is checked word for word
against the fetched page and dropped if it does not match. `x-apis-io-derivation.synthetic` is always
`false` and `method` is always `harvested` — the schema rejects anything else.

Sources (apis.yml property types): PrivacyPolicy, Privacy, DataProcessingAgreement, DPA, DataProcessingAddendum, Subprocessors.

## Standard it profiles

[W3C Data Privacy Vocabulary (DPV)](https://w3c.github.io/dpv/) — `data_categories[].category` uses the top level of the DPV personal-data taxonomy, and the controller / processor role, purposes, retention, transfers and rights follow DPV concepts.

## Using it

```
pip install jsonschema pyyaml
python3 validate.py privacy-policy-example-1.yml privacy-policy-example-2.yml
```

Exits non-zero on any failure, so it can gate a pipeline.

## License

Two licenses, by kind of thing:

- **Artifacts** — the schemas and examples — are **[CC BY-NC-SA 4.0](LICENSE)** (Attribution–NonCommercial–ShareAlike).
- **Code** — the validator — is **[Apache-2.0](LICENSE-CODE)**.

## Part of API Commons

A machine-readable building block from **[API Commons](https://apicommons.org)**. Related business-layer blocks:
[terms-of-service](https://github.com/api-commons/terms-of-service) ·
[privacy-policy](https://github.com/api-commons/privacy-policy) ·
[deprecation-policy](https://github.com/api-commons/deprecation-policy) ·
[compliance](https://github.com/api-commons/compliance) ·
[eligibility](https://github.com/api-commons/eligibility) ·
[ai-preferences](https://github.com/api-commons/ai-preferences) ·
[data-license](https://github.com/api-commons/data-license) ·
[service-level-agreement](https://github.com/api-commons/service-level-agreement) ·
[plans](https://github.com/api-commons/plans) ·
[rate-limits](https://github.com/api-commons/rate-limits)
