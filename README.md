# Seedance 1.5 Pro API (`seedance-1.5-pro`)

<!-- conv-kit:v1 -->

<p align="center">
  <img src="assets/badges/price.svg" alt="observed unit price"> <img src="assets/badges/billing.svg" alt="billing model"> <img src="assets/badges/compat.svg" alt="OpenAI-compatible endpoint">
</p>

<p align="center">
  <img src="assets/01-product-ad-ceramic-mug-thumb.jpg" width="820" alt="Sample video frame generated through APIMart">
</p>

> **from $0.0104 per second** on the APIMart route (billed by resolution tier) — ein einziger OpenAI-kompatibler Endpoint unter `https://api.apimart.ai/v1`, kein Monatsabo nötig. *(erfasst am 2026-10-12)*

**[API-Key erhalten](https://go.apimart.ai/k-2791c0)** · **[Live-Preise](https://go.apimart.ai/k-835cf5)** · **[Modellseite](https://go.apimart.ai/k-04a3eb)** · [⚡ Quickstart in 60 Sekunden](#quickstart)

**Warum Teams Seedance 1.5 Pro (`seedance-1.5-pro`) über APIMart nutzen**

- **Ein Key, der gesamte Katalog.** Dieselbe Base-URL `https://api.apimart.ai/v1` und derselbe `Authorization`-Header erreichen Seedance 1.5 Pro (`seedance-1.5-pro`) und über 300 weitere Bild-, Video- und Sprachmodelle — Sie wechseln das `model`-Feld, nicht Ihren Client.
- **$1 Minimum, Pay-as-you-go.** Kein Abo und kein Prepaid-Tarif, den Sie vorab dimensionieren müssen: ab $1 aufladen und in Calls verbrauchen.
- **Die Abrechnung kommt in der Antwort zurück.** Jede abgeschlossene Aufgabe meldet den berechneten Betrag (`cost` / `credits_cost`); eine als `failed` markierte Aufgabe wird erstattet.
- **Asynchron by Design.** Absenden, `task_id` entgegennehmen, `GET /v1/tasks/{id}` pollen — Batching und Retries sind gewöhnliche Queue-Arbeit, keine Sonderintegration.

<!-- /conv-kit:v1 -->



Seedance 1.5 Pro is ByteDance's AI video model with smoother motion, 4K quality, stability and precise control.

## Model id and routes

| Route | `model` value | Billing | Notes |
| --- | --- | --- | --- |
| Text-to-video / image-to-video | `seedance-1.5-pro` | per generated second, by resolution | see the [model page](https://go.apimart.ai/k-04a3eb) for the exact tiers |

Endpoint: `POST https://api.apimart.ai/v1/videos/generations`, then poll `GET /v1/tasks/{task_id}`.

## Pricing

<!-- pricing:model:start -->
| Resolution | List price | Effective price |
| --- | --- | --- |
| per generated second | — | see the live pricing page |


<!-- pricing:model:end -->

Prices are a snapshot; the [pricing page](https://go.apimart.ai/k-835cf5) and [`data/model.json`](data/model.json) are refreshed by CI, and a completed task reports the exact amount in its `cost` field.

## Request parameters

| Field | Type | Default | Notes |
| --- | --- | --- | --- |
| `model` | string | required | the model id shown in the route table above |
| `prompt` | string | required | the shot description; camera and subject motion belong here |
| `duration` | integer | route default | clip length in seconds — billing is per generated second |
| `resolution` | string | route default | 480P / 720P / 1080P or the route’s own tiers |
| `size` / `aspect_ratio` | string | `16:9` | output framing |
| `image_urls` | string[] | — | first frame, first + last frame, or style references, where supported |
| `seed` | integer | — | fix the seed to reproduce a take |

The table lists the fields accepted by the APIMart `videos/generations` endpoint. Check the [model page](https://go.apimart.ai/k-04a3eb) for anything this model adds on top.

## Quickstart

```bash
curl --request POST --url https://api.apimart.ai/v1/videos/generations \
  --header "Authorization: Bearer $APIMART_API_KEY" --header 'Content-Type: application/json' \
  -d '{"model":"seedance-1.5-pro","prompt":"A bamboo forest at sunrise, slow push-in","duration":5,"resolution":"720P","size":"16:9"}'
```

```python
import os, time, requests

BASE = "https://api.apimart.ai/v1"
HEADERS = {"Authorization": f"Bearer {os.environ['APIMART_API_KEY']}", "Content-Type": "application/json"}

created = requests.post(f"{BASE}/videos/generations", headers=HEADERS, timeout=60, json={
    "model": "seedance-1.5-pro", "prompt": "A bamboo forest at sunrise, slow push-in", "duration": 5,
}).json()
task_id = created["data"]["id"]

while True:
    task = requests.get(f"{BASE}/tasks/{task_id}", headers=HEADERS, timeout=60).json()["data"]
    if task["status"] in ("completed", "failed"):
        break
    time.sleep(5)
print(task.get("cost"), task["result"]["videos"][0]["url"])
```

Runnable versions: [`examples/`](examples). The task lifecycle is `pending → processing → completed | failed`.

## FAQ

**Wie lautet die Modell-id von Seedance 1.5 Pro?**

`seedance-1.5-pro`. Siehe Routentabelle oben.

**Wie wird abgerechnet?**

**from $0.0104 per second** on the APIMart route (billed by resolution tier). Die abgeschlossene Aufgabe meldet den genauen Betrag im Feld `cost`; eine fehlgeschlagene Aufgabe wird erstattet.

**How do I get a key?**

Use the [Get an API key](https://go.apimart.ai/k-2791c0) link — $1 minimum top-up, no plan required.

<!-- related-repos:auto -->
## Related repositories

- **Same model, this language** — the other half of this pair: [doubao-seedance-1-5-pro-prompts-de](https://github.com/jrn50qhjibv4/doubao-seedance-1-5-pro-prompts-de)
- **Same model, other languages** — 9 more language builds: 中文 `doubao-seedance-1-5-pro-api-cn` · Español [doubao-seedance-1-5-pro-api-es](https://github.com/j1684-htxd147q/doubao-seedance-1-5-pro-api-es) · Français [doubao-seedance-1-5-pro-api-fr](https://github.com/k2t3e2-a/doubao-seedance-1-5-pro-api-fr) · Relay/gateway [doubao-seedance-1-5-pro-api-gateway](https://github.com/mttflvbkw/doubao-seedance-1-5-pro-api-gateway) · Bahasa [doubao-seedance-1-5-pro-api-id](https://github.com/vi21di4j/doubao-seedance-1-5-pro-api-id) · 日本語 [doubao-seedance-1-5-pro-api-ja](https://github.com/kizlin5j9/doubao-seedance-1-5-pro-api-ja) · 한국어 [doubao-seedance-1-5-pro-api-ko](https://github.com/drvscj09me9h/doubao-seedance-1-5-pro-api-ko) · Português [doubao-seedance-1-5-pro-api-pt](https://github.com/e9rto/doubao-seedance-1-5-pro-api-pt) · Русский [doubao-seedance-1-5-pro-api-ru](https://github.com/wdnfucs9xu/doubao-seedance-1-5-pro-api-ru)
- **More models in this language** — [midjourney-api-de](https://github.com/h5fd0cofy/midjourney-api-de) · [gemini-2-5-flash-image-preview-api-de](https://github.com/kn6f69s2b8j9/gemini-2-5-flash-image-preview-api-de) · [nano-banana-api-de](https://github.com/uh6cob8op/nano-banana-api-de) · [gemini-3-1-flash-image-preview-api-de](https://github.com/k8wq8pd46e5ku/gemini-3-1-flash-image-preview-api-de) · [nano-banana-2-api-de](https://github.com/mocpl3/nano-banana-2-api-de) · [gemini-3-pro-image-preview-api-de](https://github.com/cmpxx2w/gemini-3-pro-image-preview-api-de)
- **Topic hubs** — every build of this model, all languages: [doubao-seedance-1-5-pro](https://github.com/topics/doubao-seedance-1-5-pro) · [doubao-seedance-1-5-pro-api](https://github.com/topics/doubao-seedance-1-5-pro-api)

This repository is one build in a cross-linked set of API guides for the same model family; each language build links back to the others.
<!-- /related-repos:auto -->

## Attributed links (how this repository is measured)

| Purpose | Attributed link | Target |
| --- | --- | --- |
| Model page | <https://go.apimart.ai/k-04a3eb> | `apimart.ai/de/model/doubao-seedance-1-5-pro` |
| Live pricing | <https://go.apimart.ai/k-835cf5> | `apimart.ai/de/pricing` |
| Get an API key | <https://go.apimart.ai/k-2791c0> | `apimart.ai/de/keys` |

Outbound APIMart links are minted through the promo link API; hand-made tracking parameters are rejected by
`tools/check_links.py` in CI.

## Disclosure

Seedance 1.5 Pro is a third-party model served through APIMart; this repository documents how to call it and publishes
model ids and prices, and does not claim official status. Model names, prices and documentation belong to their
respective owners.

## License

MIT — see [LICENSE](LICENSE).
