# Mistral API Pricing in 2026: Rates, Cache, and Request Costs

![Abstract three-stream token-pricing cover.](cover.png)

A team can read a Mistral model price and still misbudget the feature it powers. The API lists rates **per million tokens**, while a request has its own mix of input, cached input, and output. Output tokens carry their own rate. Repeated prompt text gets the cached rate only when the API actually reports a cache hit.

For a first estimate, choose an exact model and multiply your assumed token counts by its published rates. The table below gives the direct Mistral text rates visible on November 19, 2026. A worked Mistral Large 4 example then turns those rates into a per-request and 10,000-request scenario. Neither scenario is a customer bill. If you later use Batch or buy through another provider, check that route's conditions separately.

## Current direct Mistral text API rates

The [Mistral API pricing page](https://docs.mistral.ai/inference/pricing) showed **Standard**, **USD**, and **Regional inference off** when checked on November 19, 2026. These are published direct rates in US dollars per **1 million tokens**. The cached-input column is a different meter from ordinary input, not a discount that every repeated request receives.

| Mistral model | Input / 1M tokens | Cached input / 1M tokens | Output / 1M tokens |
|---|---:|---:|---:|
| Mistral Large 4 | $0.70 | $0.07 | $2.10 |
| Mistral Small 4 | $0.15 | $0.015 | $0.60 |
| Ministral 3 3B | $0.10 | $0.01 | $0.10 |
| Ministral 3 8B | $0.15 | $0.015 | $0.15 |
| Ministral 3 14B | $0.20 | $0.02 | $0.20 |

Model generation matters: a Large 3 figure found in an older guide is not a Large 4 rate. So do the controls on the pricing page. A regional inference setting, a different currency, or Batch rather than Standard processing may change the quote you see. Check the live page again before committing a budget.

This table is deliberately about text-model token charges. The same pricing page also lists products with other billing units, such as OCR per 1,000 pages and some audio services per minute or character. Those numbers cannot be dropped into a text-token calculation. A Le Chat subscription is a separate consumer product, not an allowance to assume for API requests.

## What a Mistral Large 4 request could cost

Suppose a feature sends **8,000 ordinary input tokens** and receives **600 output tokens** per call. Assume the direct Mistral Large 4 Standard USD rates above, no cached input, no Batch processing, and no other charge. The published-rate estimate is:

`(8,000 ÷ 1,000,000 × $0.70) + (600 ÷ 1,000,000 × $2.10) = $0.00560 + $0.00126 = $0.00686 per request.`

At **10,000 identical requests**, that is **$68.60**. Replace the two assumed token counts with a sample from your workload; the same arithmetic then answers your feature-budget question. It is not a forecast of actual usage: output length varies, prompts can grow, and a route may have separate terms. To estimate your own feature, log or otherwise determine input, cached-input, and output token counts for the chosen model and apply the rate for each category. Keep the model and mode beside the result so a later price check is possible.

## When caching and Batch change the number

Mistral's [prompt caching documentation](https://docs.mistral.ai/inference/conversations/advanced/prompt-caching) describes compatible cached prefixes billed at **10% of ordinary input**. A `prompt_cache_key` can improve the chance of reuse, but setting it does not guarantee a cache hit. Use the cached-input meter only for tokens the API actually counts as cached; otherwise they remain ordinary input.

For example, if **4,000** of the assumed 8,000 input tokens in the Large 4 request really are cached, the remaining **4,000** use the ordinary input rate. With the same 600 output tokens, the conditional estimate becomes:

`(4,000 ÷ 1,000,000 × $0.70) + (4,000 ÷ 1,000,000 × $0.07) + (600 ÷ 1,000,000 × $2.10) = $0.00280 + $0.00028 + $0.00126 = $0.00434 per request.`

For 10,000 calls with that *same actual token mix*, the arithmetic gives **$43.40**. That lower figure is a scenario, not a promised saving. Cache eligibility, prefix stability, the tokens reported as hits, and the provider's billing rules determine whether it applies.

Mistral's [Batch Processing documentation](https://docs.mistral.ai/inference/batch-processing), checked November 19, 2026, describes a **50% discount for eligible asynchronous large workloads**. Batch is a different way to submit work, with its own job and endpoint requirements. Do not halve a Standard request estimate merely because you expect many calls; confirm that the intended workload can run through Batch and check its current terms. Cache and Batch should be modeled as explicit conditions, not stacked into an unexplained “best price.”

## Compare a provider quote before choosing a route

The name of the seller belongs next to the model name. On November 19, 2026, the public [Moduck](https://moduck.ai/models/c/mistral/mistral-large-4?lang=en) Mistral Large 4 page published **$0.544 input, $0.056 cached input, and $1.672 output per million tokens**. The direct [Mistral](https://docs.mistral.ai/inference/pricing) Standard USD, regional-off Large 4 row published **$0.70, $0.07, and $2.10** for those three meters. Both are public quoted rates; neither is an invoice observed in this research.

| Published route and named model | Input / 1M | Cached input / 1M | Output / 1M | What still needs checking |
|---|---:|---:|---:|---|
| [Mistral](https://docs.mistral.ai/inference/pricing), direct Large 4 Standard USD, regional-off | $0.70 | $0.07 | $2.10 | Your account's access, selected mode and any other applicable terms |
| [Moduck](https://moduck.ai/models/c/mistral/mistral-large-4?lang=en), published Mistral Large 4 listing | $0.544 | $0.056 | $1.672 | Whether the listed model ID is callable for your account, configuration and full charges |

The Moduck listing gives `mistral/mistral-large-4` as a quickstart model ID, but its own FAQ says that this example is **not confirmed callable** and that no verified API call for the model has been made there. The lower published row is a useful quote to investigate; it does not establish the same service configuration, entitlement, latency, or final bill as direct Mistral. Other aggregators also have their own terms: the [Requesty](https://www.requesty.ai/models/mistral) listing, for instance, has conflicting markup language on the same page, so an account-specific quote would need confirmation before comparison.

For a decision, write down the seller, exact model version and ID, currency, billable unit, processing mode, region, cache behavior, and any added fees or taxes. Then apply your measured or assumed token mix. That makes a lower-looking row a question you can verify, rather than a guarantee of savings.

If Moduck is one route you are considering, [Read the documentation](https://docs.moduck.ai/) and confirm the current model availability and account terms before using an API key. Validate the route and its full terms for your account before relying on the estimate; then rerun it with actual usage counts.
