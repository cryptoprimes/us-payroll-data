# US payroll data, 2026

Machine-readable reference data for United States payroll withholding: federal
parameters, state income tax at a glance, state pay rules, local income taxes
and reciprocity agreements.

**Every record carries the document it came from and the date it was checked.**
That is the point of this repository. Payroll data is easy to find and hard to
trust: rates get copied between blog posts until nobody can say which year they
belong to. Here each figure links to the authority that published it - an IRS
publication, a state revenue department, a city's own tax page.

The data runs the calculators at [paycheckledger.com](https://paycheckledger.com).

## Files

| File | What is in it | Records |
|---|---|---|
| `data/federal.json` | 2026 withholding schedules, FICA rates and wage base, standard deduction, supplemental rate | - |
| `data/states.json` | Income tax by state: flat or banded rates, notes, what changed for 2026, sales tax, bordering states | 50 |
| `data/pay-rules.json` | Minimum wage, tipped wages, overtime, required pay frequency, final paycheck rules | 50 |
| `data/local-taxes.json` | City and county income taxes charged on top of the state rate | 5 |
| `data/reciprocity.json` | Agreements under which wages are taxed only where the employee lives, with the exemption form | 6 |

`local-taxes.json` and `reciprocity.json` are deliberately short. A state is
listed only where its own documents confirm the detail; a partial list would be
worse than none, because a reader whose state is missing would conclude the rule
does not exist. More states are added as they are checked.

## Shape

```json
{
  "taxYear": 2026,
  "ratesChecked": "2026-09-01",
  "license": "CC BY 4.0",
  "states": [
    {
      "key": "alabama",
      "name": "Alabama",
      "checked": "2026-09-15",
      "minimumWage": {
        "rate": 7.25,
        "note": "There is no state minimum wage law; employers covered by federal law pay the federal minimum.",
        "source": {
          "url": "https://www.dol.gov/agencies/whd/minimum-wage/state",
          "label": "U.S. Department of Labor state minimum wage table"
        }
      }
    }
  ]
}
```

Keys are stable lowercase slugs (`california`, `new-jersey`), so files join on
`key` without any normalising.

## Using it

```bash
curl -sL https://raw.githubusercontent.com/cryptoprimes/us-payroll-data/main/data/pay-rules.json \
  | jq '.states[] | select(.minimumWage.rate > 15) | {name, rate: .minimumWage.rate}'
```

No API, no key, no rate limit: these are static files. Pin a commit if you need
the figures to stay still - the files change when the law does.

## What is not here

The withholding formulas themselves. Each state turns gross pay into tax its own
way - allowances, exemptions, low-income thresholds, deductions that taper - and
that logic lives in the site's code rather than in a data file, because it is
code. What is here is the published, checkable part.

This data is a reference, not tax advice, and it describes withholding rather
than what you finally owe.

## Corrections

If a figure does not match the source, or a source has moved, open an issue with
the document you are reading from. Corrections are checked against the original
and then fixed everywhere the figure is used.

## Licence

[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) - use it anywhere,
including commercially, with attribution to
[Paycheck Ledger](https://paycheckledger.com).
