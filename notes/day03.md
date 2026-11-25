## 1. System Contract Parameters

Network: Lasna Testnet

System Contract:
`0x8888888888888888888888888888888888888888`

RPC: `https://lasna-rpc.rnk.dev/`

### Parameter 1: Gas Price Coefficient

Command:

`cast call 0x8888888888888888888888888888888888888888 "_gasPriceCoeffPer1000()" --rpc-url https://lasna-rpc.rnk.dev/ | cast to-dec`

Output: `1000`

### Parameter 2: Extra Gas

Command:

`cast call 0x8888888888888888888888888888888888888888 "_extraGas()" --rpc-url https://lasna-rpc.rnk.dev/ | cast to-dec`

Output: `100000`

### Parameter 3: Max Charge Gas

Command:

`cast call 0x8888888888888888888888888888888888888888 "_maxChargeGas()" --rpc-url https://lasna-rpc.rnk.dev/ | cast to-dec`

Output: `50000`

## 2. How is the react() fee calculated?

The fee formula is:

Fee = Base Gas Price × Coefficient × (Gas Used + Extra Gas)

The coefficient is 1000 / 1000 = 1, and the extra gas is 100000.

Therefore:

Fee = Base Gas Price × (Gas Used + 100000)

The actual fee depends on the gas price and the gas consumed during execution.

The max charge gas value of 50000 limits the gas budget for payment settlement. It is not added directly to the fee formula.

## 3. What are Balance, Reserves, and Debt?

- Balance: The native tokens held by the contract itself.
- Reserves: Prepaid funds held by the system contract to cover execution fees.
- Debt: Unpaid execution fees.

When debt is greater than zero, Reactive Network stops delivering new reactive transactions to the contract until the debt is fully settled.

This prevents a contract with unpaid fees from continuing to receive execution services.

## 4. Is a reverting react() still charged?

Yes. Even if react() reverts, the execution still consumes gas.

The system contract catches the revert, records a ReactiveContractReverted event, and charges for the gas consumed.

Therefore, developers should minimize unnecessary reverts, validate event data, and test their contracts carefully to avoid wasting funds.

## Reference

https://dev.reactive.network/economy