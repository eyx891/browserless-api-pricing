# browserless api：How Unit-Based Pricing Really Works, the Hidden Costs Nobody Shows You, and When a Managed Scraping API Is the Better Fit

If you're searching for a browserless api, you're usually trying to solve one problem: get pages rendered, scraped, or screenshotted without running your own headless Chrome fleet. Browserless is the best-known product built exactly for that — you connect Puppeteer, Playwright, Selenium, or its REST endpoints over a hosted browser, and it handles the infrastructure. This article breaks down how Browserless's unit-based pricing actually behaves in production, then covers the case where a different kind of API — a managed scraping API like ScraperAPI that hands you back finished data instead of a browser session — saves you both code and money.

> **Quick take:** If your goal is *getting data* — price monitoring, SERP tracking, structured Amazon or Google results — a managed API like ScraperAPI is usually the faster path: a single GET request, no browser code to maintain, plans from **$49/month** (Hobby, 100,000 credits) with a free 5,000-credit, 7-day trial. If your goal is *driving a browser yourself* — logged-in flows, multi-step sessions, your own Puppeteer scripts — Browserless's model fits that better. 👉 [See ScraperAPI's current plans and start the free trial](https://www.scraperapi.com/?fp_ref=coupons)

## What a browserless API actually gives you

A browserless API rents you a browser in the cloud. In Browserless's case, you get three surfaces over one platform: WebSocket connections that behave like a local Puppeteer/Playwright/Selenium browser (across Chromium, Chrome, Edge, Firefox, and WebKit), REST endpoints for common jobs like screenshots, PDFs, and page content, and a GraphQL automation layer called BrowserQL for stealth-heavy work. Your script connects to a hosted endpoint, and the browser runs on their servers.

That model gives you full control — you write the navigation logic, handle cookies, click buttons, wait for elements. The tradeoff is that you own the scraping logic too: link discovery, queues, deduplication, parsing. The API solves infrastructure, not the workflow.

A managed scraping API like ScraperAPI sits at a different layer. You send a target URL plus your API key, and the service handles proxy rotation, retries, CAPTCHA solving, and optional JavaScript rendering behind the scenes, then returns the raw page — or, on supported targets like Amazon product pages, Google Search, and Walmart, already-parsed JSON. There's no browser session for you to script; the "browser" is an implementation detail you toggle with a parameter like `render=true`.

Neither approach dominates the other. They overlap on "render this JS-heavy page," and they diverge everywhere else.

## How Browserless pricing works — and where it surprises people

Browserless sells a monthly allowance of units. One unit is up to 30 seconds of browser time on a single connection. The published tiers:

| Plan | Monthly price | Yearly (per month) | Units included | Concurrent browsers |
| --- | --- | --- | --- | --- |
| Free | $0 | — | 1,000/month | 2 |
| Prototyping | $35 | $25 | 20,000 | 5 (10 yearly) |
| Starter | $200 | $140 | 180,000 | 30 (40 yearly) |
| Scale | $500 | $350 | 500,000 | 80 (100 yearly) |

Enterprise is custom-priced. Yearly billing saves about 30%, which is generous compared to most SaaS plans.

The part that pricing tables don't show: **a page that loads in 4 seconds on its own connection still burns a full unit**. If you open a new connection per page — which is how most straightforward scrapers work — your effective cost is $1.75 per 1,000 pages on Prototyping, $1.11 on Starter, and $1.00 on Scale at monthly rates. The biggest lever on your bill is reusing one connection across multiple navigations, which spreads fast pages across a single 30-second window. That's a code change, not a plan change.

Three more line items live outside the plan cards:

- **Residential proxy traffic costs 6 units per MB; datacenter proxy traffic costs 2 units per MB.** A 2 MB page fetched through the residential proxy costs 13 units once you add browser time — roughly $22.75 per 1,000 pages even on the cheapest paid plan at monthly rates.
- **Each successfully solved CAPTCHA costs an extra 10 units.**
- **Paid plans keep running past their allowance** at $0.0020, $0.0017, or $0.0015 per unit (Prototyping/Starter/Scale), and Browserless bills the accumulated overage whenever it reaches the price of your subscription. A runaway task can therefore generate a mid-month charge equal to an entire plan. Changing plans mid-cycle resets the billing period and bills the new plan at full price.

To be fair to Browserless: the free plan simply rejects requests once 1,000 units are gone — no surprise overage invoice at $0. And for its actual use case (scripted, logged-in, multi-step browser work), unit pricing is more predictable than bandwidth-based proxy billing. Just budget with the per-page math above, not the headline unit count.

## When a managed scraping API makes more sense

Most people who type "browserless api" into a search box aren't building a browser automation platform. They need prices from an e-commerce site, rankings from Google, or product data from Amazon, and they'd rather not write and maintain proxy rotation, retry logic, and CAPTCHA handling. That's where ScraperAPI's model pays off.

You register, get an API key, and a single GET request works within minutes — no proxy list to configure, no browser lifecycle to manage. Behind one endpoint you get a pool of 40+ million IPs across residential, datacenter, and mobile networks in 50+ countries, with parameters instead of infrastructure: `country_code` for geotargeting, `render=true` for JS-heavy pages, `premium=true` or `ultra_premium=true` for targets that fight back, and `session_number` to stick to one IP for up to about an hour.

On structured targets it goes further: Amazon product and search pages, Google Search and Shopping, and Walmart return parsed JSON directly. When the target site redesigns its markup, ScraperAPI's parser takes the hit, not your code.

If you want to compare real prices side by side before committing either way, 👉 [check ScraperAPI's full plan lineup here](https://www.scraperapi.com/?fp_ref=coupons).

## ScraperAPI plans: the complete table

All eight tiers currently shown on ScraperAPI's pricing page. Annual billing saves about 10% across every paid plan.

| Plan | Monthly | Annual (per mo.) | API credits | Threads | Geotargeting | Get it |
| --- | --- | --- | --- | --- | --- | --- |
| Free Trial | Free, 7 days | — | 5,000 | — | — | [Start free](https://www.scraperapi.com/?fp_ref=coupons) |
| Hobby | $49 | $44 | 100,000 | 20 | US & EU | [Get Hobby](https://www.scraperapi.com/?fp_ref=coupons) |
| Startup | $149 | $134 | 1,000,000 | 50 | US & EU | [Get Startup](https://www.scraperapi.com/?fp_ref=coupons) |
| Business | $299 | $269 | 3,000,000 | 100 | Global (country-level) | [Get Business](https://www.scraperapi.com/?fp_ref=coupons) |
| Scaling | $475 | $428 | 5,000,000 | 200 | Global | [Get Scaling](https://www.scraperapi.com/?fp_ref=coupons) |
| Professional | $975 | $878 | 10,500,000 | 300 | Global | [Get Professional](https://www.scraperapi.com/?fp_ref=coupons) |
| Advanced | $1,975 | $1,778 | 21,500,000 | 500 | Global | [Get Advanced](https://www.scraperapi.com/?fp_ref=coupons) |
| Enterprise | Custom quote | — | 22M+ | 500+ | Custom | [Contact sales](https://www.scraperapi.com/?fp_ref=coupons) |

Every paid plan includes premium residential IPs, premium mobile IPs, advanced bypassing, parsing APIs, Ultra Premium access, and full crawler access. Professional and above add priority support and pay-as-you-go overflow; Enterprise adds a dedicated account manager and a Slack channel. Scaling is the plan ScraperAPI itself flags as most popular.

## The credit math most reviews skip

Here's the gap in most comparisons you'll find online: they quote the per-credit price and stop. The credit cost per request varies enormously by what you ask for:

- A standard HTTP request through the datacenter pool: **1 credit**
- `render=true` (headless browser): about **10 credits**
- Premium proxy: about **10 credits**
- Ultra-premium pools (LinkedIn-grade targets): up to **30 credits**
- Google and Bing SERP requests: **25 credits**
- Amazon, Walmart, eBay requests: **5 credits**

Run the numbers on a realistic job and the picture changes. Startup looks like $0.000149 per credit on paper — cheap. But a protected target with `render=true` plus `premium=true` can cost 20–30× that headline figure per successful scrape. If you're scraping Amazon search results with JS rendering enabled, 1,000,000 credits might realistically cover 40,000–100,000 actual pages, not a million.

The practical advice from people who've audited this: budget against your real per-page cost, not the credit count on the pricing page. The 5,000-credit free trial exists precisely for this — run your actual target URLs through it, count the credits consumed, and extrapolate before you pick a plan. 👉 [Test your real targets with the free 5,000-credit trial](https://www.scraperapi.com/?fp_ref=coupons)

## Limits you should know before picking a plan

Two restrictions sit in the fine print rather than on the pricing cards:

**Geotargeting is gated by tier.** Hobby and Startup ($49 and $149) only target US and EU. Country-level targeting anywhere else in the world starts at Business, $299/month. If your use case requires scraping from, say, Asian or South American markets at entry-level budget, this is the constraint that will force an upgrade.

**Sticky sessions top out around an hour.** `session_number` keeps you on one IP for a single scraping session. It works fine for collecting a paginated catalog. It does not work for ad verification or account workflows that need the same identity across days — if that's your actual job, a dedicated residential or mobile proxy provider is a better tool category, and forcing a scraping API into that role will frustrate you.

Support is also tiered: Hobby and Startup get email and ticket support only, no live chat. If you scrape targets that change their anti-bot rules without warning and you need fast turnaround, factor that in or look at Business and above.

## Head-to-head: Browserless vs. ScraperAPI

Pure information comparison — different tools for different layers of the same problem:

| Dimension | Browserless | ScraperAPI |
| --- | --- | --- |
| What you get | A hosted browser you drive with Puppeteer/Playwright/Selenium, plus REST endpoints | Finished pages or parsed JSON from one request |
| Billing unit | 30-second browser units | API credits (1–30+ per request depending on options) |
| Entry paid price | $35/month (20,000 units) | $49/month (100,000 credits) |
| Free tier | 1,000 units/month, ongoing | 7-day trial, 5,000 credits |
| CAPTCHA handling | 10 units per successful solve | Included, billed via credit multipliers |
| Proxy traffic | Extra units per MB (residential 6/MB) | Included via premium/ultra-premium credit tiers |
| Who writes the scraping logic | You | The API does rotation, retries, parsing |
| Best fit | Logged-in, multi-step, custom browser workflows | Data extraction from e-commerce, SERP, and standard web targets |

If you genuinely need to script a browser — log in, click through a funnel, maintain state — Browserless is purpose-built for that and you should use something like it. If you need data and want the anti-bot layer handled for you, ScraperAPI's model removes most of the code. Many teams end up using both at different layers; there's no rule that says you must pick one.

## FAQ

**Is there a free plan for a browserless API?**
Browserless offers a genuine ongoing free tier of 1,000 units per month. ScraperAPI offers a 7-day trial with 5,000 credits instead of a permanent free plan — enough to measure your real per-page credit burn before paying.

**How much does ScraperAPI cost per month?**
Paid plans run $49 (Hobby), $149 (Startup), $299 (Business), $475 (Scaling), $975 (Professional), and $1,975 (Advanced) per month, with roughly 10% off on annual billing. Enterprise is custom-quoted.

**Does ScraperAPI handle JavaScript-heavy pages?**
Yes — set `render=true` and the request runs through a headless browser. It costs about 10 credits instead of 1, so reserve it for pages that need it.

**What if my targets are extremely well protected?**
Sites behind aggressive anti-bot systems (LinkedIn, Instagram, heavily-defended e-commerce) will push you into the ultra-premium tiers, where per-page costs climb sharply. At sustained high volume against those specific targets, a bandwidth-billed residential proxy setup can come out cheaper. For standard e-commerce and search targets, the default pool usually clears most requests without the premium multiplier.

## Choosing between them, concretely

- **You need structured data from Amazon, Google, or Walmart, monthly volume of roughly 20,000–200,000 pages, and no desire to maintain proxy code** → ScraperAPI's Startup plan at $149/month (1M credits, 50 threads) is the workhorse pick; Hobby at $49 works for smaller or mostly-static targets.
- **You need to drive a browser yourself for logged-in or multi-step flows** → Browserless's model fits; start on the free 1,000 units and do the per-page math before committing to a year.
- **Your targets are outside US/EU** → remember ScraperAPI gates global geotargeting at Business ($299); budget accordingly instead of discovering it after checkout.
- **You need cross-day session identity (ad verification, account work)** → neither tool is the right category; look at dedicated residential or mobile proxy providers.

One last check before you commit to anything: run your three most important target URLs through the trial and read the actual credit consumption per request. That single number decides which plan — and which tool — is genuinely cheapest for your workload. 👉 [Start the ScraperAPI free trial and measure your real costs](https://www.scraperapi.com/?fp_ref=coupons)
