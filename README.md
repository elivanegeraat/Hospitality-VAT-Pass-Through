# Did Hospitality VAT Changes Reach Customers' Prices?

I measured how much of Ireland's hospitality VAT changes were passed on to consumer prices, and whether VAT rises were passed on more than VAT cuts.

## Question

When the Irish government changed the VAT rate on restaurants, hotels and hairdressing, how much of the change showed up in the prices customers paid?

## Background

The hospitality VAT rate has moved between 13.5% and 9% several times:

| Date | Change | Reason |
|---|---|---|
| 1 July 2011 | Cut from 13.5% to 9% | Jobs Initiative after the financial crisis |
| 1 January 2019 | Raised from 9% to 13.5% | End of the temporary cut |
| 1 November 2020 | Cut from 13.5% to 9% | COVID support |
| 1 September 2023 | Raised from 9% to 13.5% | End of the temporary cut |
| 1 July 2026 | Cut from 13.5% to 9% | Budget 2026 (food-led hospitality and hairdressing only, not hotels) |

Moving between 9% and 13.5% VAT changes a VAT-inclusive price by about 4%. **Pass-through** is the share of that 4% that actually shows up in prices. 100% means customers get the full change, and 0% means businesses absorb or keep it.

## Data

- **Consumer Price Index by Detailed Sub Indices** from the CSO's PxStat database (data.cso.ie), monthly, 2012 to 2026.
- **Affected services:** restaurants and cafés, canteens, hairdressing and accommodation.
- **Unaffected services (controls):** services from the same CPI whose VAT rate did not change, such as car repairs, dental and veterinary services, clothing cleaning and repair, and domestic services.

## Method

For each VAT change, I ran an event study using the 6 months before and after:

- The regression includes a fixed effect for each price series and each month. The month effects remove anything that hits all prices at once, like general inflation.
- For each month around the change, it estimates how far affected prices moved relative to unaffected ones, compared with the month just before the change.
- **Pass-through** is the average effect over the 6 months after the change, divided by the full 4% effect.
- **Placebo test:** I reran each event pretending each control series was the affected one. If the real effect is larger than most of these fake effects, it is unlikely to be chance.
- I also estimated pass-through for each type of service on its own.

## Results

Pass-through for the two cleanest categories:

| VAT change | Restaurants, cafés and the like | Hairdressing salons |
|---|---|---|
| 2011 cut | 9% | 10% |
| 2019 rise | 30% | 74% |
| 2020 cut | not reliable (COVID) | 5% |
| 2023 rise | 45% | 76% |
| 2026 cut | 7% (2 months of data) | 7% (2 months of data) |

Key findings:

- **Rises were passed on much more than cuts.** For both restaurants and hairdressing, VAT rises showed pass-through of 30% to 76%, while cuts showed 10% or less. This "rockets and feathers" pattern means a temporary VAT cut followed by a reversal can leave prices higher than they would otherwise have been.
- **Hairdressing passed on more than restaurants.** This fits the fact that the restaurant index also includes alcohol, which stays at the 23% standard rate, so part of the restaurant bill was never affected by the change.
- **My results line up with the Irish Fiscal Advisory Council's 2025 study** of the same changes, which used more detailed price data. They found hairdressing pass-through of 74% in 2019 and 70% in 2023, and concluded that rises are passed on more than cuts.
- **The 2026 cut shows little pass-through so far,** but with only two months of data this is an early reading.

When all affected services are pooled together, including hotels, the results are much noisier (34% for the 2011 cut, 70% for the 2019 rise, and around zero or negative for 2020, 2023 and 2026). This is because hotel prices are strongly seasonal, as explained below.

## Limitations

- **Hotel prices are highly seasonal.** They rise into summer and fall afterwards, which is mistaken for a VAT effect in a 6 month window. This gives impossible hotel estimates (above 100% or well below zero), so I focus on restaurants and hairdressing. The Fiscal Council seasonally adjusted hotel prices for the same reason.
- **Public CPI categories are broad.** The restaurant index mixes food with alcohol, which pulls its pass-through down. The Fiscal Council used item-level prices supplied by the CSO to avoid this.
- **The controls are other Irish services,** which may face different cost pressures. The Fiscal Council used UK prices of the same services instead.
- **2019:** the minimum wage also rose on 1 January 2019, at the same time as the VAT increase.
- **2020:** COVID lockdowns disrupted price collection, and the standard VAT rate, which applies to several control services, was cut from 23% to 21% between September 2020 and February 2021.
- **2023:** high general inflation and months of uncertainty over whether the rise would go ahead may have spread price changes out over time.

## Reference

Carroll, K. (2025). "VAT rate changes and pass-through: Evidence from the Irish hospitality and tourism industry." Irish Fiscal Advisory Council Working Paper No. 27.

## How to run it

1. The CSO blocks downloads from Google Colab, so download the data manually. On data.cso.ie, open **Consumer Price Index by Detailed Sub Indices**, select the index statistic (not the percentage changes), all months and all sub-indices, and download it as a CSV.
2. Open `hospitality_vat_pass_through.ipynb` in Google Colab and run the cells in order.
3. When the upload button appears, select the CSV you downloaded.

## Tools

Python (pandas, numpy, matplotlib). I wrote the regressions directly in numpy so each step is visible.
