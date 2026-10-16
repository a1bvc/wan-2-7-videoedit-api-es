# Wan 2.7 Video Edit API (`wan2.7-videoedit`)

<!-- conv-kit:v1 -->

<p align="center">
  <img src="assets/badges/price.svg" alt="observed unit price"> <img src="assets/badges/billing.svg" alt="billing model"> <img src="assets/badges/compat.svg" alt="OpenAI-compatible endpoint">
</p>

<p align="center">
  <img src="assets/01-product-ad-ceramic-mug-thumb.jpg" width="820" alt="Sample video frame generated through APIMart">
</p>

> **from $0.0664 per second** on the APIMart route — un único endpoint compatible con OpenAI en `https://api.apimart.ai/v1`, sin plan mensual. *(observado el 2026-10-12)*

**[Obtener una clave de API](https://go.apimart.ai/k-e15d44)** · **[Precios en vivo](https://go.apimart.ai/k-0428bb)** · **[Página del modelo](https://go.apimart.ai/k-d77ae0)** · [⚡ Inicio rápido en 60 segundos](#quickstart)

**Por qué los equipos usan Wan 2.7 Video Edit (`wan2.7-videoedit`) a través de APIMart**

- **Una clave, todo el catálogo.** La misma base URL `https://api.apimart.ai/v1` y la misma cabecera `Authorization` dan acceso a Wan 2.7 Video Edit (`wan2.7-videoedit`) y a más de 300 modelos de imagen, vídeo y lenguaje: cambia el campo `model`, no tu cliente.
- **Mínimo de $1, pago por uso.** Sin suscripción ni plan prepago que dimensionar por adelantado: recarga desde $1 y úsalo en llamadas.
- **El cargo vuelve en la respuesta.** Cada tarea completada informa del importe facturado (`cost` / `credits_cost`); una tarea marcada como `failed` se reembolsa.
- **Asíncrono por diseño.** Envía, toma el `task_id`, consulta `GET /v1/tasks/{id}`: el batching y los reintentos son trabajo de cola normal, no una integración a medida.

<!-- /conv-kit:v1 -->



WAN 2.7 Video Edit API on APIMart is the latest generation AI video model, offering superior visual quality, high motion coherence, and professional-grade performance. Use wan2.7-videoedit to edit an existing source video, with optional instructions and up to four reference images for the target style or appearance.

## Model id and routes

| Route | `model` value | Billing | Notes |
| --- | --- | --- | --- |
| Text-to-video / image-to-video | `wan2.7-videoedit` | per generated second, by resolution | see the [model page](https://go.apimart.ai/k-d77ae0) for the exact tiers |

Endpoint: `POST https://api.apimart.ai/v1/videos/generations`, then poll `GET /v1/tasks/{task_id}`.

## Pricing

<!-- pricing:model:start -->
| Resolution | List price | Effective price |
| --- | --- | --- |
| default | 0.083 | 0.0664 |


<!-- pricing:model:end -->

Prices are a snapshot; the [pricing page](https://go.apimart.ai/k-0428bb) and [`data/model.json`](data/model.json) are refreshed by CI, and a completed task reports the exact amount in its `cost` field.

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

The table lists the fields accepted by the APIMart `videos/generations` endpoint. Check the [model page](https://go.apimart.ai/k-d77ae0) for anything this model adds on top.

## Quickstart

```bash
curl --request POST --url https://api.apimart.ai/v1/videos/generations \
  --header "Authorization: Bearer $APIMART_API_KEY" --header 'Content-Type: application/json' \
  -d '{"model":"wan2.7-videoedit","prompt":"A bamboo forest at sunrise, slow push-in","duration":5,"resolution":"720P","size":"16:9"}'
```

```python
import os, time, requests

BASE = "https://api.apimart.ai/v1"
HEADERS = {"Authorization": f"Bearer {os.environ['APIMART_API_KEY']}", "Content-Type": "application/json"}

created = requests.post(f"{BASE}/videos/generations", headers=HEADERS, timeout=60, json={
    "model": "wan2.7-videoedit", "prompt": "A bamboo forest at sunrise, slow push-in", "duration": 5,
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

**¿Cuál es el id del modelo Wan 2.7 Video Edit?**

`wan2.7-videoedit`. Consulta la tabla de rutas de arriba.

**¿Cómo se factura?**

**from $0.0664 per second** on the APIMart route. La tarea completada informa del importe exacto en su campo `cost`; una tarea fallida se reembolsa.

**How do I get a key?**

Use the [Get an API key](https://go.apimart.ai/k-e15d44) link — $1 minimum top-up, no plan required.

<!-- related-repos:auto -->
## Related repositories

- **Same model, this language** — the other half of this pair: [wan-2-7-videoedit-prompts-es](https://github.com/a1bvc/wan-2-7-videoedit-prompts-es)
- **Same model, other languages** — 9 more language builds: 中文 `wan-2-7-videoedit-api-cn` · Deutsch [wan-2-7-videoedit-api-de](https://github.com/p0tabmmad84qix/wan-2-7-videoedit-api-de) · Français [wan-2-7-videoedit-api-fr](https://github.com/sg40e98dutpek/wan-2-7-videoedit-api-fr) · Relay/gateway `wan-2-7-videoedit-api-gateway` · Bahasa [wan-2-7-videoedit-api-id](https://github.com/k7v530p627/wan-2-7-videoedit-api-id) · 日本語 [wan-2-7-videoedit-api-ja](https://github.com/a04qbov86mrd/wan-2-7-videoedit-api-ja) · 한국어 `wan-2-7-videoedit-api-ko` · Português `wan-2-7-videoedit-api-pt` · Русский [wan-2-7-videoedit-api-ru](https://github.com/f60ozqblr0/wan-2-7-videoedit-api-ru)
- **More models in this language** — [midjourney-api-es](https://github.com/xbuu0/midjourney-api-es) · [gemini-2-5-flash-image-preview-api-es](https://github.com/ekw3w/gemini-2-5-flash-image-preview-api-es) · [nano-banana-api-es](https://github.com/gbm5mfl1/nano-banana-api-es) · [gemini-3-1-flash-image-preview-api-es](https://github.com/x3kcczm8l29xn/gemini-3-1-flash-image-preview-api-es) · [nano-banana-2-api-es](https://github.com/d63hy9t-4pp/nano-banana-2-api-es) · [gemini-3-pro-image-preview-api-es](https://github.com/uv9d8/gemini-3-pro-image-preview-api-es)
- **Topic hubs** — every build of this model, all languages: [wan-2-7-videoedit](https://github.com/topics/wan-2-7-videoedit) · [wan-2-7-videoedit-api](https://github.com/topics/wan-2-7-videoedit-api)

This repository is one build in a cross-linked set of API guides for the same model family; each language build links back to the others.
<!-- /related-repos:auto -->

## Attributed links (how this repository is measured)

| Purpose | Attributed link | Target |
| --- | --- | --- |
| Model page | <https://go.apimart.ai/k-d77ae0> | `apimart.ai/es/model/wan-2-7-videoedit` |
| Live pricing | <https://go.apimart.ai/k-0428bb> | `apimart.ai/es/pricing` |
| Get an API key | <https://go.apimart.ai/k-e15d44> | `apimart.ai/es/keys` |

Outbound APIMart links are minted through the promo link API; hand-made tracking parameters are rejected by
`tools/check_links.py` in CI.

## Disclosure

Wan 2.7 Video Edit is a third-party model served through APIMart; this repository documents how to call it and publishes
model ids and prices, and does not claim official status. Model names, prices and documentation belong to their
respective owners.

## License

MIT — see [LICENSE](LICENSE).
