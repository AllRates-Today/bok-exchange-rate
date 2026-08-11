# Bank of Korea Exchange Rate API client

Official **Bank of Korea** (South Korea) daily exchange rates in Node.js / TypeScript — 43 currencies against the KRW, with history back to 1964. Zero dependencies, works in Node 18+, Bun, Deno, and edge runtimes (uses global `fetch`).

These are the *published central bank rates* required for tax filings, customs valuations, audits, and compliant invoicing — not moving market rates. Every response carries the publisher's own publication date.

Powered by [AllRatesToday](https://allratestoday.com/central-bank-rates-api/bok/). Get a free API key at [allratestoday.com/register](https://allratestoday.com/register) — no credit card required.

## Install

```bash
npm install bok-exchange-rate
```

## Quick start

```js
import { getRate, getLatestRates } from 'bok-exchange-rate';

// One pair at the official Bank of Korea rate
const pair = await getRate('USD', 'KRW', { apiKey: 'art_live_...' });
console.log(pair.rate, pair.rate_date); // e.g. USD -> KRW on the bank's own date

// The bank's full published table
const table = await getLatestRates({ apiKey: 'art_live_...' });
console.log(table.rate_date, table.rates.length);
```

## Historical data (paid plans)

```js
import { getRatesForDate, getHistory } from 'bok-exchange-rate';

// The official table for an invoice date — weekends/holidays return the
// most recent published date, flagged via published_on_requested_date.
const day = await getRatesForDate('2026-06-30', { apiKey: 'art_live_...' });

// Daily series for one pair
const series = await getHistory(
  { source: 'USD', target: 'KRW', from: '2026-01-01' },
  { apiKey: 'art_live_...' }
);
```

## Currencies covered

Bank of Korea currently publishes rates covering **44 currencies** (as of the latest table):

`AED` · `ARS` · `AUD` · `BDT` · `BHD` · `BND` · `BRL` · `CAD` · `CHF` · `CNY` · `CZK` · `DKK` · `EGP` · `EUR` · `GBP` · `HKD` · `HUF` · `IDR` · `ILS` · `INR` · `JOD` · `JPY` · `KRW` · `KWD` · `KZT` · `MNT` · `MXN` · `MYR` · `NOK` · `NZD` · `PHP` · `PKR` · `PLN` · `QAR` · `RUB` · `SAR` · `SEK` · `SGD` · `THB` · `TRY` · `TWD` · `USD` · `VND` · `ZAR`

Pairs the central bank does not print directly are resolved from this table (see below).

## Published vs derived rates

If Bank of Korea does not print a pair directly, the API resolves it from the bank's table (inverse, or a cross rate via KRW) and flags it `derived: true` with the `method` — so official and computed values are never confused.

## Notes

- Every request counts toward your AllRatesToday monthly quota. Rates change once per business day — cache a day's table locally and a small quota goes a long way.
- Latest rates are on every plan (including free); historical dates and time series need a [paid plan](https://allratestoday.com/pricing/).
- Full API reference: [allratestoday.com/docs#central-bank](https://allratestoday.com/docs/#central-bank) · All covered sources: [central bank rates API](https://allratestoday.com/central-bank-rates-api/)

## License

MIT
