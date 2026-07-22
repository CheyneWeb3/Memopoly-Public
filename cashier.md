# Universal Multi-Chain USDC Cashier Platform

## System Design and Architecture Overview

## 1. Executive Summary

The Universal Multi-Chain USDC Cashier Platform is a custodial financial infrastructure layer designed for games, applications and digital platforms that need to accept, hold, allocate, settle and withdraw USDC across multiple blockchain networks.

The platform provides users with one unified internal USDC balance regardless of the chain used to deposit funds. Integrated games and applications interact with the cashier through controlled internal accounting operations rather than managing blockchain wallets, private keys, gas, bridges or treasury liquidity themselves.

Blockchain activity is limited primarily to:

* Receiving user and application deposits.
* Sweeping deposit wallets.
* Processing withdrawals.
* Moving treasury liquidity between supported chains.
* Funding chain-specific native gas balances.
* Reconciling controlled assets against internal liabilities.

All gameplay, application purchases, prize pools, entry fees, refunds, winnings and transfers are settled instantly within an immutable double-entry ledger.

The system must remain fully backed on a one-to-one basis. Every USDC recorded as belonging to a user, game, application, prize pool or other liability must be backed by controlled and verifiable USDC.

The first application using the cashier may be Memopoly, but the cashier is designed as an independent reusable platform capable of supporting many games and applications simultaneously.

---

## 2. Core System Principles

The platform is built around the following principles.

### 2.1 One universal USDC balance

A user may deposit USDC on one supported chain and later use or withdraw that value through another supported chain.

The user's internal balance is not chain-specific after the deposit has been confirmed and credited.

For example:

1. A user deposits 100 USDC on Solana.
2. The cashier confirms and credits 100 internal USDC.
3. The user spends 10 USDC in Memopoly.
4. The user wins 25 USDC in another integrated game.
5. The user withdraws the remaining balance on Polygon.

The internal accounting system handles ownership. The treasury system handles the physical location of USDC.

### 2.2 One-to-one backing

Every internal USDC liability must be backed by real controlled USDC.

The fundamental solvency rule is:

```text
Eligible controlled USDC assets
must be greater than or equal to
total USDC liabilities
```

Internal credits must never be created without corresponding backing assets, except where a separately funded and recorded application or platform account is being debited.

### 2.3 Blockchain is not the gameplay ledger

Games and applications must not settle each individual action on-chain.

Blockchain networks are used for custody movement, while the internal ledger records ownership and application activity.

This provides:

* Immediate game settlement.
* No repeated wallet signatures.
* No bridge delays during gameplay.
* No gas costs for each game action.
* Consistent user balances across applications.
* Clear auditing and reconciliation.

### 2.4 Treasury liquidity is separate from ownership

The blockchain chain on which USDC is held does not determine which user or application owns that USDC.

Physical custody may be pooled across operational wallets, but ledger ownership must remain isolated.

### 2.5 Games never control blockchain keys

Integrated games and applications cannot access:

* Treasury private keys.
* Deposit-wallet master keys.
* Withdrawal-signing keys.
* Bridge-signing keys.
* Circle Gateway credentials.
* Gas treasury wallets.

Applications communicate with the cashier through authenticated and authorised internal operations.

---

## 3. High-Level Architecture

```text
Users and Application Operators
             │
             ▼
     Deposit and Withdrawal Layer
             │
             ▼
  Universal Multi-Chain USDC Cashier
             │
   ┌─────────┼──────────┬─────────────┐
   ▼         ▼          ▼             ▼
User       Double-     Application   Treasury and
Balances   Entry       Accounting    Liquidity Manager
           Ledger
   │                       │             │
   └──────────────┬────────┘             │
                  ▼                      ▼
         Game and Application API   Chain Treasuries
                  │                      │
       ┌──────────┼──────────┐           ├─ Circle Gateway
       ▼          ▼          ▼           ├─ LI.FI
   Memopoly     Poker    Other Apps      ├─ deBridge
                                           └─ Approved Providers
```

---

## 4. User Account Model

Each user has one universal cashier account.

The internal balance is divided into separately controlled states.

### 4.1 User balance categories

* **Available balance**
  Funds the user can spend, reserve or withdraw.

* **Reserved balance**
  Funds temporarily locked for an intended action that has not yet begun.

* **In-game balance**
  Funds committed to an active game, tournament, market or application session.

* **Withdrawal-pending balance**
  Funds reserved for an active withdrawal request.

* **Restricted balance**
  Funds blocked from normal use because of compliance review, dispute, security concern or another authorised restriction.

* **Refund-pending balance**
  Funds allocated to a refund operation that has not yet completed.

### 4.2 User activity totals

The system may additionally maintain derived reporting totals such as:

* Lifetime deposited.
* Lifetime withdrawn.
* Lifetime entry fees.
* Lifetime prizes and winnings.
* Lifetime application spending.
* Lifetime refunds.
* Lifetime fees paid.
* Current exposure by application.
* Current pending withdrawal amount.

These totals are reporting outputs. They do not replace the underlying ledger.

---

## 5. Application and Game Integration Model

The cashier is an independent platform serving multiple games and applications.

Each integration receives:

* A unique application identity.
* Secure API credentials.
* Configured permissions.
* Application-specific treasury accounts.
* Settlement limits.
* Fee policies.
* Gas and routing-cost policies.
* Webhook or event-delivery configuration.
* Independent financial reporting.
* Independent audit history.

### 5.1 Application isolation

Each application operates inside its own accounting namespace.

Examples include:

```text
application.memopoly
application.poker
application.tournaments
application.marketplace
application.rewards
```

One application must not be able to access or spend:

* Another application's treasury.
* Another application's prize pool.
* Another application's jackpot.
* Another application's fees.
* User funds not validly committed to that application.
* Platform reserves.
* Withdrawal reserves.
* Gas treasury assets.

### 5.2 Typical application accounts

Every application may have accounts for:

* Operator-funded treasury.
* Player funds reserved.
* Active-game liabilities.
* Prize liabilities.
* Jackpot liabilities.
* Refund liabilities.
* Promotional balances.
* Fee revenue.
* Rake revenue.
* Gas expenses.
* Bridge expenses.
* Cashier service fees.
* Settlement clearing.
* Net amount payable to the operator.
* Net amount receivable from the operator.

Different game types may use additional specialised accounts.

### 5.3 Application-owned treasury balances

Games and applications may hold their own internally recorded treasury entitlement within the cashier.

These funds can be used for:

* Guaranteed prizes.
* Promotional credits.
* Tournament overlays.
* Player rewards.
* Refund funding.
* Jackpot funding.
* Operational subsidies.
* Withdrawal-fee subsidies.
* Gas-cost subsidies.

Application funds may be physically pooled with other USDC held by the cashier, but their ownership remains separately recorded.

---

## 6. Example Game Accounting

Suppose two players enter a 10 USDC Memopoly game.

### 6.1 Reservation

For each player:

```text
Debit:  User available liability
Credit: User Memopoly reserved liability
Amount: 10 USDC
```

### 6.2 Game start

For each player:

```text
Debit:  User Memopoly reserved liability
Credit: Memopoly active-game pot liability
Amount: 10 USDC
```

The game pot now contains 20 USDC.

### 6.3 Game settlement

Assume the application charges a 1 USDC game fee and pays 19 USDC to the winner.

```text
Debit:  Memopoly active-game pot liability
Amount: 20 USDC

Credit: Winner available liability
Amount: 19 USDC

Credit: Memopoly fee revenue
Amount: 1 USDC
```

No USDC is created or destroyed. Ledger ownership is reassigned.

### 6.4 Cancelled game

If the game is cancelled before valid settlement:

```text
Debit:  Memopoly active-game pot liability
Credit: Each participating user's available liability
```

The settlement must be idempotent so that a game cannot be paid, cancelled or refunded more than once.

---

## 7. Double-Entry Ledger

The immutable double-entry ledger is the authoritative record of ownership, obligations and financial activity.

Every valid operation must:

* Debit one or more accounts.
* Credit one or more accounts.
* Balance to zero.
* Have a unique idempotency reference.
* Record the responsible user, application or system actor.
* Record timestamps and status.
* Include supporting external references when applicable.
* Never be silently altered or deleted.

Corrections must be made through new compensating entries.

### 7.1 Ledger event categories

Typical events include:

* User deposit.
* Application deposit.
* Deposit reversal.
* Balance reservation.
* Reservation release.
* Game entry.
* Game payout.
* Game refund.
* Application purchase.
* Application reward.
* Jackpot contribution.
* Jackpot payout.
* User withdrawal.
* Withdrawal reversal.
* Cashier fee.
* Application fee.
* Gas cost.
* Bridge cost.
* Treasury rebalance.
* Treasury sweep.
* Platform revenue.
* Operator settlement.
* Administrative adjustment.
* Reconciliation correction.

### 7.2 Idempotency

Every externally initiated operation must contain a unique reference.

Examples include:

* Application ID plus game ID plus settlement version.
* Withdrawal request ID.
* Deposit transaction and instruction identifier.
* Bridge operation ID.
* Application funding reference.

Repeated requests with the same idempotency key must return the existing result rather than create a second financial movement.

---

## 8. One-to-One Backing and Solvency

### 8.1 Eligible backing assets

Backing assets may include:

* Confirmed USDC in controlled Polygon wallets.
* Confirmed USDC in controlled Solana wallets.
* Confirmed USDC on other supported chains.
* USDC available through controlled Circle Gateway balances.
* Destination-final bridge receipts awaiting treasury classification.
* Other approved controlled USDC holdings.

### 8.2 Assets not immediately considered spendable

The following must not be treated as payout-ready liquidity:

* Unconfirmed deposits.
* Submitted but unconfirmed bridge transfers.
* Failed transfers.
* Recoverable but unresolved transfers.
* Unsupported bridged-token variants.
* Frozen or restricted assets.
* Funds in wallets outside platform control.
* Funds represented only by a provider quote.
* Native gas assets valued in dollars.

### 8.3 Liability categories

USDC liabilities include:

* User available balances.
* User reserved balances.
* User in-game balances.
* Pending withdrawals.
* Application treasury balances.
* Active prize pools.
* Jackpots.
* Refund obligations.
* Promotional balances.
* Operator balances payable.
* Other redeemable claims.

### 8.4 Platform equity

The following are not customer liabilities once correctly earned and settled:

* Cashier service revenue.
* Realised application fees belonging to the platform.
* Realised withdrawal-fee margin.
* Realised routing margin.
* Platform-funded reserves.
* Retained earnings.

They remain separately accounted for and must not be confused with player or application funds.

---

## 9. Multi-Chain Deposit System

Supported blockchains act as deposit and withdrawal rails.

A user may have a uniquely attributable deposit address or deposit instruction for each supported chain.

### 9.1 Deposit process

```text
User sends canonical USDC
        │
        ▼
Chain-specific deposit watcher
        │
        ▼
Validate destination, token and amount
        │
        ▼
Wait for required confirmation policy
        │
        ▼
Record controlled asset
        │
        ▼
Credit internal user balance
        │
        ▼
Optionally sweep deposit wallet
```

### 9.2 Deposit validation

The watcher must verify:

* The transaction succeeded.
* The destination is controlled by the cashier.
* The token is the approved canonical USDC contract or mint.
* The received amount is calculated correctly.
* The deposit has not already been credited.
* Required confirmation or finality thresholds have been reached.
* The transfer has not been reverted or invalidated.
* The receiving wallet or token account is valid.
* The deposit is associated with the correct user or application.

### 9.3 Canonical USDC policy

The initial automated-credit policy should accept only explicitly configured canonical USDC contracts or mints.

Token names and symbols are not sufficient.

Tokens such as the following must not automatically be assumed equivalent:

* USDC.
* USDC.e.
* Bridged USDC.
* Wormhole-wrapped USDC.
* Axelar-wrapped USDC.
* Other derivative or synthetic USDC representations.

Known noncanonical assets may be detected and placed into a review or conversion workflow, but they should not be credited as normal USDC without a controlled conversion.

---

## 10. Treasury Structure

The cashier holds chain-specific USDC treasuries and separate native-gas treasuries.

### 10.1 USDC treasuries

Examples include:

* Polygon USDC treasury.
* Solana USDC treasury.
* Avalanche USDC treasury.
* Ethereum USDC treasury.
* Arbitrum USDC treasury.
* Base USDC treasury.
* Other supported-chain treasuries.
* Circle Gateway-controlled balance.

### 10.2 Treasury roles

A chain may have multiple wallet roles:

* Deposit collector.
* Sweep wallet.
* Hot withdrawal wallet.
* Warm operational wallet.
* Cold reserve wallet.
* Bridge source wallet.
* Bridge destination wallet.
* Recovery wallet.

These roles should be separated based on security, transaction frequency and operational needs.

### 10.3 Polygon as an operating foundation

Polygon may serve as the preferred operational reserve and consolidation chain because of its suitability for frequent USDC operations.

However, Polygon must not be treated as the mandatory source for every payout.

The system should use existing destination-chain liquidity first.

Funds deposited on Solana may remain in the Solana treasury until reserve policies justify movement. The same applies to other chains.

Unnecessary bridging should be avoided.

---

## 11. Circle Gateway Role

Circle Gateway is treasury infrastructure rather than a player-facing balance system.

Its purpose is to help the cashier coordinate and move controlled USDC liquidity across supported chains.

Users and games do not directly interact with Gateway.

The treasury manager may use Gateway for:

* Supported-chain reserve replenishment.
* Liquidity relocation.
* Fast withdrawal shortfall funding.
* Treasury consolidation.
* Emergency liquidity recovery.
* Maintaining a unified operational liquidity position.

Gateway balances and operations must still be reconciled against the internal treasury ledger.

---

## 12. Third-Party Bridge Integration

When Circle Gateway is unavailable, unsuitable or slower than an approved alternative, the system may use third-party providers such as:

* LI.FI.
* deBridge.
* Other reviewed providers added later.

Third-party providers are used by the treasury routing layer. They are not directly exposed as uncontrolled player actions.

### 12.1 Provider approval requirements

A route may be used only when it satisfies configured requirements for:

* Supported source chain.
* Supported destination chain.
* Canonical destination USDC.
* Maximum fee.
* Maximum slippage.
* Minimum expected output.
* Route expiry.
* Provider reliability.
* Security approval.
* Transaction simulation where supported.
* Destination verification.
* Recovery and tracking capability.

---

## 13. Treasury Routing Policies

Every treasury movement is assigned a routing policy.

The core routing policies are:

* **Fast**
* **Balanced**
* **Lowest cost**

### 13.1 Fast policy

Fast mode prioritises:

1. Estimated completion time.
2. Reliability.
3. Destination certainty.
4. Cost.

Fast mode is used for:

* User withdrawal liquidity shortfalls.
* Critical destination-chain reserve shortages.
* Emergency replenishment.
* Time-sensitive application settlements.
* Operational incidents requiring immediate liquidity.

A more expensive route may be selected when it is materially faster and remains within configured safety and cost limits.

### 13.2 Balanced policy

Balanced mode considers:

* Estimated completion time.
* Total fee.
* Reliability.
* Slippage.
* Destination liquidity.
* Provider health.

Balanced mode is suitable for:

* Routine reserve replenishment.
* Normal operational movement.
* Medium-priority application funding.
* Nonurgent treasury adjustments.

### 13.3 Lowest-cost policy

Lowest-cost mode prioritises the lowest safe total cost.

It may accept a longer completion time.

It is suitable for:

* Scheduled treasury rebalancing.
* Excess-liquidity consolidation.
* Nightly sweeps.
* Large nonurgent movements.
* Application-operator settlements with no immediate payout requirement.

### 13.4 Route selection is dynamic

The routing engine should not permanently assume that one provider is always fastest or cheapest.

It evaluates available routes using current provider quotes, operational health, expected destination amount and reliability.

---

## 14. Withdrawal System

User withdrawals are treated as time-sensitive financial obligations.

### 14.1 Withdrawal rules

* The full requested amount is reserved internally before execution.
* A user cannot spend the reserved amount.
* The system validates the destination chain and address.
* Canonical USDC must be delivered.
* Local destination-chain liquidity is used first.
* A withdrawal should normally be sent as one complete payment.
* Partial payouts should not occur unless specifically authorised through an exceptional recovery process.
* Withdrawal execution must be idempotent.
* Failed payouts must remain recoverable and visible.

### 14.2 Withdrawal flow

```text
User requests withdrawal
        │
        ▼
Validate account and destination
        │
        ▼
Reserve full internal amount
        │
        ▼
Check spendable destination liquidity
   ┌────┴────┐
   │         │
Enough     Insufficient
   │         │
   ▼         ▼
Send      Calculate shortfall
payout         │
               ▼
       Use Fast routing policy
               │
               ▼
       Confirm treasury receipt
               │
               ▼
          Send full payout
               │
               ▼
        Finalise ledger entry
```

### 14.3 Withdrawal shortfall policy

When the destination treasury lacks sufficient USDC, the system automatically prioritises speed.

The user should not be forced to wait for the cheapest treasury route.

The priority order is conceptually:

1. Spendable USDC already available in the destination hot wallet.
2. Spendable USDC elsewhere on the same chain.
3. Fast Circle Gateway movement.
4. Fastest safe approved LI.FI route.
5. Fastest safe direct deBridge route.
6. Another approved route.
7. Manual intervention when all automated routes fail.

Actual selection should depend on current quotes and route health rather than a permanently fixed provider order.

### 14.4 Shortfall example

A user requests 1,000 USDC on Solana.

The Solana treasury has 700 USDC available.

```text
Requested withdrawal: 1,000 USDC
Local liquidity:         700 USDC
Liquidity shortfall:     300 USDC
```

The system should:

1. Reserve the entire 1,000 USDC user balance.
2. Route enough USDC to cover the shortfall and applicable buffer.
3. Confirm the destination treasury has received spendable funds.
4. Send one 1,000 USDC withdrawal.
5. Record fees, gas and routing expenses separately.
6. Mark the withdrawal complete only after destination confirmation.

### 14.5 Aggregating withdrawal shortfalls

Multiple withdrawals requiring replenishment should be coordinated.

For example:

```text
Pending Solana withdrawals: 850 USDC
Spendable Solana liquidity: 300 USDC
Combined shortfall:         550 USDC
```

The liquidity controller may execute one replenishment operation rather than several competing bridge transactions.

It must lock the expected incoming liquidity so multiple workers cannot allocate it twice.

---

## 15. Liquidity Reserve Management

Each chain treasury has configurable reserve levels.

* **Critical reserve**
* **Minimum reserve**
* **Target reserve**
* **Maximum reserve**

### 15.1 Reserve behaviour

Below critical reserve:

* Trigger immediate fast replenishment.
* Prioritise user withdrawals.
* Restrict nonessential sweeps.
* Alert operators.
* Consider pausing new large withdrawals on that chain until liquidity is secured.

Below minimum reserve:

* Queue replenishment.
* Use balanced mode unless forecast withdrawals require fast mode.

At target reserve:

* No movement required.

Above maximum reserve:

* Sweep excess using lowest-cost mode.
* Move excess to Gateway, Polygon or another approved reserve location.

### 15.2 Forecasting

The liquidity controller should consider:

* Current treasury balance.
* Pending withdrawals.
* Recently confirmed deposits.
* Application payout schedules.
* Historical chain demand.
* Pending inbound treasury transfers.
* Pending outbound treasury transfers.
* Gas availability.
* Provider health.

The system should replenish before a treasury becomes empty rather than waiting for the first failed payout.

---

## 16. Funds in Transit

Treasury transfers require an explicit lifecycle.

Recommended states include:

* Quoted.
* Approved.
* Submitted.
* Source confirmed.
* Provider acknowledged.
* Destination observed.
* Destination confirmed.
* Destination final.
* Failed.
* Recoverable.
* Recovered.
* Cancelled.
* Manually resolved.

### 16.1 Clearing-account treatment

When funds leave the source treasury:

```text
Debit:  Bridge or treasury clearing asset
Credit: Source-chain USDC treasury asset
```

When destination funds become final:

```text
Debit:  Destination-chain USDC treasury asset
Credit: Bridge or treasury clearing asset
```

Provider fees, routing fees and delivery shortfalls are recorded separately.

### 16.2 Spendability

Submitted or source-confirmed transfers must not be treated as spendable destination liquidity.

Only destination-confirmed funds meeting the configured finality policy may be allocated to withdrawals.

---

## 17. Native Gas Treasury System

USDC treasuries and native gas treasuries are separate accounting systems.

Supported gas assets may include:

* POL for Polygon.
* SOL for Solana.
* ETH for Ethereum and supported Ethereum rollups.
* AVAX for Avalanche.
* Other native chain assets.

### 17.1 Gas usage

Native gas treasuries may fund:

* Deposit-wallet sweeps.
* User withdrawals.
* Circle Gateway transactions.
* Bridge transactions.
* Token-account creation.
* Treasury consolidation.
* Recovery transactions.
* Emergency wallet operations.

### 17.2 Gas reserve levels

Each chain has:

* Critical gas balance.
* Minimum gas balance.
* Target gas balance.
* Maximum gas balance.

Below critical:

* Alert operators.
* Pause nonessential on-chain activity.
* Prioritise user withdrawals and recovery operations.
* Refill immediately.
* Prevent new automated work that could strand funds.

### 17.3 Gas accounting

Actual gas expenditure must be recorded separately from USDC principal.

The system must distinguish:

* Gas paid by the platform.
* Gas charged to an application.
* Gas charged to a user.
* Gas subsidised by an application.
* Gas absorbed as an operational expense.
* Gas recovered through a withdrawal fee.

### 17.4 Application cost policies

Each application may use one of the following cost policies:

* Platform pays.
* Application pays.
* User pays.
* Shared cost.
* Configurable subsidy.

The policy may differ by operation.

Example:

```text
Deposits: platform funded
Game ledger activity: no blockchain gas
Withdrawals: user fee with platform subsidy
Promotional payouts: application funded
Treasury rebalancing: platform funded
```

---

## 18. Application-Level Accounting and Reporting

Each application must receive an independent statement showing its financial activity.

A typical statement includes:

```text
Opening operator balance
+ Operator deposits
+ Game fees earned
+ Other application revenue
- Application-funded prizes
- Promotional rewards
- Refunds
- Gas expenses
- Bridge and routing expenses
- Cashier service fees
- Operator withdrawals or settlements
=
Closing operator balance
```

Applications should also be able to review:

* Active player liabilities.
* Current game pots.
* Reserved entry fees.
* Pending prize settlements.
* Jackpots.
* Refund obligations.
* Daily fees.
* Daily player volume.
* Deposit and withdrawal attribution.
* Gas and routing expenses.
* Cashier charges.
* Reconciliation status.

---

## 19. Platform Accounts

The universal cashier needs its own accounts, separate from user and application funds.

Platform accounts may include:

* Cashier fee revenue.
* Withdrawal-fee revenue.
* Application-service revenue.
* Bridge-fee revenue.
* Gas-expense accounts.
* Bridge-expense accounts.
* Security reserves.
* Insurance or loss reserves.
* Operational treasury.
* Platform profit.
* Reconciliation suspense.
* Recovery suspense.

Platform funds may not be represented as user or application balances.

---

## 20. Reconciliation

Reconciliation confirms that internal ownership records match controlled external assets.

### 20.1 Reconciliation layers

The system must reconcile:

* User ledger balances.
* Application ledger balances.
* Platform accounts.
* Chain wallet USDC balances.
* Circle Gateway balances.
* Funds in transit.
* Pending deposits.
* Pending withdrawals.
* Native gas balances.
* Bridge-provider operation records.
* Application settlement records.

### 20.2 Global solvency

```text
Total eligible USDC assets
-
Total USDC liabilities
=
Global surplus or deficit
```

A deficit must trigger a critical incident.

### 20.3 Per-application solvency

```text
Application-funded entitlement
+ valid application revenue
-
application obligations
=
Application surplus or deficit
```

An underfunded application must not draw value from another application's funds or from user funds.

### 20.4 Per-chain liquidity

```text
Spendable chain USDC
-
pending chain withdrawals
-
required operational reserve
=
Available chain liquidity
```

The platform may be globally solvent while temporarily short of liquidity on a particular chain. The treasury router solves this through replenishment.

### 20.5 Reconciliation exceptions

Differences may result from:

* Unrecorded transfer.
* Duplicate deposit credit.
* Failed withdrawal finalisation.
* Bridge fee.
* Gas expense.
* Unsupported token receipt.
* Manual wallet transfer.
* Provider shortfall.
* Accounting error.
* Security compromise.

Differences must be investigated and assigned. They must never be silently absorbed.

---

## 21. Security and Operational Controls

### 21.1 Key separation

Keys should be separated by role and risk.

Examples:

* Deposit derivation authority.
* Hot withdrawal signer.
* Treasury movement signer.
* Gateway signer.
* Bridge signer.
* Recovery signer.
* Cold reserve signer.

### 21.2 Limits

The platform should enforce:

* Per-user withdrawal limits.
* Per-application settlement limits.
* Per-chain hot-wallet limits.
* Daily treasury-movement limits.
* Provider route limits.
* Maximum automatic bridge amount.
* Manual approval thresholds.
* Gas-spend limits.
* Velocity and anomaly limits.

### 21.3 Application authentication

Applications must use secure authentication and request signing.

The cashier must verify:

* Application identity.
* Request signature.
* Timestamp.
* Nonce.
* Idempotency key.
* Allowed operation.
* Amount limits.
* User-authorisation context.
* Application treasury sufficiency.

### 21.4 Administrative actions

Administrative balance adjustments require:

* Explicit reason.
* Authorised administrator.
* Supporting reference.
* Double-entry accounting.
* Permanent audit history.
* Optional multi-approval for large adjustments.

No administrator should directly edit stored balance totals without ledger entries.

---

## 22. Reliability and Recovery

The platform must assume that providers, RPC endpoints, chains and workers can fail.

### 22.1 At-most-once financial finalisation

The system must prevent:

* Duplicate deposit credits.
* Duplicate withdrawals.
* Duplicate game payouts.
* Duplicate refunds.
* Duplicate application settlements.
* Duplicate treasury routes.

Operations may be retried operationally, but financial finalisation must remain idempotent.

### 22.2 Recovery workers

Recovery processes should identify:

* Submitted but unresolved withdrawals.
* Source-confirmed bridges with no destination receipt.
* Destination receipts not entered in the ledger.
* Confirmed deposits not credited.
* Credited deposits not swept.
* Low gas balances.
* Application settlements stuck in progress.
* Mismatched treasury balances.

### 22.3 Manual recovery

Operators require a controlled recovery interface showing:

* Current operation state.
* Transaction references.
* Expected and observed amounts.
* Source and destination chain.
* Provider.
* Associated user or application.
* Recommended safe next action.
* Whether another transaction is safe.
* Whether funds are already spendable.

Recovery tools must not create additional transactions blindly.

---

## 23. User Experience

Users should see:

* One USDC balance.
* Deposit options by supported chain.
* Withdrawal options by supported chain.
* Clear pending and completed status.
* Application activity.
* Games and purchases.
* Fees charged.
* Withdrawal transaction reference.
* Any temporary restriction or review state.

Users do not need to understand:

* Treasury wallets.
* Gateway balances.
* Route providers.
* Rebalancing.
* Internal clearing accounts.
* Gas treasury operations.

When a withdrawal needs liquidity replenishment, the user may see:

```text
Withdrawal processing

The destination network is being funded using fast settlement.
Your withdrawal will be sent after destination liquidity is confirmed.
```

They should not be asked to select a cheaper, slower route for a normal withdrawal. The cashier automatically prioritises speed.

---

## 24. Operator Experience

Cashier administrators need a unified dashboard showing:

* Total controlled USDC.
* Total liabilities.
* Global surplus or deficit.
* Assets by chain.
* User liabilities.
* Application liabilities.
* Platform equity.
* Pending withdrawals.
* Pending deposits.
* Funds in transit.
* Gas balances.
* Reserve warnings.
* Provider health.
* Reconciliation exceptions.
* Application-level profitability.
* Routing fees and execution times.
* Stuck or recoverable operations.

Application operators receive access only to their own accounting, reports and authorised actions.

---

## 25. System Boundaries

The universal cashier is responsible for:

* Custody.
* Deposit detection.
* User balances.
* Application balances.
* Internal settlement.
* Withdrawals.
* Treasury routing.
* Gas funding.
* Reconciliation.
* Financial audit records.

Integrated games are responsible for:

* Game logic.
* Determining valid results.
* Producing authorised settlement requests.
* Enforcing their own gameplay rules.
* Supplying application-level references.
* Handling player-facing game interaction.

The cashier must not blindly trust a game's reported result. Each integration needs defined permissions, limits and settlement rules.

---

## 26. Supported Use Cases

The cashier can support:

* Multiplayer games.
* Tournament platforms.
* Poker and table games.
* Fantasy competitions.
* Skill-based games.
* Prediction applications.
* Digital marketplaces.
* Creator rewards.
* Social tipping.
* Trading competitions.
* Prize pools.
* Membership platforms.
* Other USDC-denominated applications.

Each use case can retain independent treasury and liability accounting while sharing the same custody and treasury infrastructure.

---

## 27. Recommended Initial Scope

The first production release should prioritise a controlled foundation.

### Phase 1

* Universal user USDC balance.
* Polygon and Solana deposits and withdrawals.
* Canonical USDC validation.
* Double-entry ledger.
* Memopoly integration.
* Separate Memopoly treasury accounts.
* Chain-specific USDC treasuries.
* Chain-specific gas treasuries.
* Local-liquidity-first withdrawals.
* Fast withdrawal shortfall routing.
* One-to-one solvency monitoring.
* Deposit, withdrawal and application reconciliation.

### Phase 2

* Additional Circle Gateway-supported chains.
* Automated reserve targets.
* Circle Gateway treasury liquidity.
* Application onboarding system.
* Application-level reporting.
* Operator settlement.
* Aggregated withdrawal replenishment.
* Advanced recovery tooling.

### Phase 3

* LI.FI and deBridge fallback routing.
* Lowest-cost scheduled rebalancing.
* Dynamic provider scoring.
* Additional games and applications.
* Advanced risk controls.
* Treasury forecasting.
* Broader compliance and reporting integrations.

---

## 28. Final Architecture Statement

The Universal Multi-Chain USDC Cashier Platform is a custodial, double-entry, one-to-one-backed financial operating system for games and digital applications.

Users maintain one universal internal USDC balance regardless of deposit chain. Games and applications settle activity instantly within the internal ledger while maintaining independent treasury balances, liabilities, revenues, prize pools, fees and operating expenses.

Physical USDC may be pooled across chain treasuries, Circle Gateway and approved custody wallets, but every unit remains attributed to a specific user, application, platform account or obligation.

Withdrawals use existing destination-chain liquidity first. When destination liquidity is insufficient, the system automatically uses the fastest safe route to replenish the destination treasury before making one complete payout to the user. Routine treasury movement may instead use balanced or lowest-cost routing.

Native gas assets are maintained in separate chain-specific gas treasuries and are independently accounted for. Gas, bridge and routing costs are attributed to the platform, application or user according to configured policies.

The internal ledger records ownership. Blockchain balances prove controlled assets. Continuous reconciliation ensures the two remain aligned.

Memopoly is the first application integrated with the platform, but the cashier is designed as a reusable financial layer for multiple independent games and applications.
