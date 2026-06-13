# Temporal API

📋 Docs: [Temporal (mdn)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Temporal)

💡 Quiz: [jsdate.wtf/](https://jsdate.wtf/)

______

- There is a Iso Day: 24h, but no ISO month or year

- This means that you can only add up to one day (unit) to *durations* and up to hours (unit) to *instants*. 

- Temporal-polyfill (Safari)

- Migration (use polyfill!) - effort/experience: 1400 vs 1000 lines before (more verbose) but catched 1 bug and 1 suboptimal Date usage

- <input type='date' /> doesn‘t support it yet
