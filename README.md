# Retry & Replan Controller

Bounded state machine for agent recovery, retries, and evidence-backed FINISH.

Standalone ES module. No npm install required. Works in **Node.js 20+** and modern browsers (serve over HTTP for browser demos).

## Included files

This repository vendors local dependencies needed by the module:

- `retry-replan-controller.js` (main)
- `dod-contract.js`

## Install / import

Copy the files into your project, or clone this repository:

```js
import { createContract } from './dod-contract.js';
import { createController } from './retry-replan-controller.js';
const contract = createContract({id:'invoice', task:'Produce invoice total 42',
  criteria:[{id:'total',description:'JSON total equals 42'}]});
const controller=createController({contract,maxRetries:0});
controller.transition('BUILD');
controller.transition('VERIFY');
console.log(controller.transition('RETRY')); // ESCALATE: retry-limit
```

## Use cases

### 1. Recover without looping forever

- **Scenario:** A generated invoice fails the total check.
- **Outcome:** Allow one bounded repair cycle; escalate if the retry allowance is exhausted.

### 2. Stop a task that overruns its limits

- **Scenario:** A builder repeatedly proposes another attempt.
- **Outcome:** Enforce time, retry, and step limits independently of model requests.

## Exports

- `createController`

## Notes

- Keep API keys and secrets out of this module and out of browser storage.
- Treat verification callbacks, ledger writers, and permission checks as trusted application code.
- This library is intentional local reliability plumbing, not a claim of cryptographic attestation unless stated in the source comments.

## License

MIT
