<div align="center">

# MEMOPOLY

**A live multiplayer crypto property strategy game with independent EVM and Solana USDC deployments.**

[![EVM Live Beta](https://img.shields.io/badge/EVM%20Live%20Beta-Play%20Now-627EEA?style=for-the-badge)](https://memopoly.hausserver.xyz)
[![Solana Live Beta](https://img.shields.io/badge/Solana%20Live%20Beta-Play%20Now-14F195?style=for-the-badge&logo=solana&logoColor=white)](https://memopoly-svm.hausserver.xyz)
[![Docker](https://img.shields.io/badge/Docker-Compose-2496ED?style=for-the-badge&logo=docker&logoColor=white)](#system-design)
[![USDC](https://img.shields.io/badge/USDC-Staked%20Prize%20Pools-2775CA?style=for-the-badge&logo=usdcoin&logoColor=white)](#usdc-staked-play)
[![Status](https://img.shields.io/badge/Status-Public%20Beta-F2C94C?style=for-the-badge)](#beta-status)

<img src="https://raw.githubusercontent.com/CheyneWeb3/Memopoly-Public/refs/heads/main/desktop.png" alt="Memopoly multiplayer browser game board" width="1100">

**[Play the EVM live beta](https://memopoly.hausserver.xyz)**

**[Play the Solana live beta](https://memopoly-svm.hausserver.xyz)**

Free Play uses game-only currency. Staked Play uses the supported USDC rail of the selected deployment.

</div>

---

## About Memopoly

Memopoly is a server-authoritative multiplayer property strategy game built around crypto, meme-token and DeFi themes. Players move around a 40-space board, buy token properties, collect rent, build Houses and Hotels, trade assets, use the Banker, draw cards and compete through live auctions.

Memopoly is available as two independently deployed live beta products:

- **EVM live beta** — the original EVM implementation with signed EVM-wallet authentication and an EVM token cashier.
- **Solana live beta** — an independent Solana `mainnet-beta` implementation using canonical Circle USDC and Solana wallet signatures.

Both versions retain the same core game, lobby, account, Banker, ledger, reservation, settlement, history and administration concepts. Their blockchain integrations and runtime infrastructure are deliberately separate.

Within either deployment, players can choose:

- **Free Play** — no token entry, prize pool or on-chain settlement.
- **Staked Play** — every funded seat contributes the configured USDC entry to a protected table prize pool.

The familiar flow of a property board game is delivered through an original browser interface, crypto-themed board, custom cards, persistent player profiles, multiplayer rooms and server-controlled rules.

<div align="center">
  <img src="docs/images/welcome-screen.png" alt="Memopoly welcome screen" width="560">
</div>

---

## Play the live betas

| Version | Play | Wallet rail | Funded-play asset |
|---|---|---|---|
| **EVM live beta** | [memopoly.hausserver.xyz](https://memopoly.hausserver.xyz) | Signed EVM wallet authentication and EVM token transfers | Deployment-configured USDC-compatible token |
| **Solana live beta** | [memopoly-svm.hausserver.xyz](https://memopoly-svm.hausserver.xyz) | Ed25519 Solana wallet authentication and SPL-token transfers | Canonical Circle USDC on Solana |

The two live betas are not merely different network selections inside one backend. They are concurrent, independent systems.

They do **not** share:

- player accounts or login sessions;
- linked wallets or deposit addresses;
- Banker balances, reservations or ledger entries;
- active rooms, game state or settlement records;
- MongoDB or Redis state;
- Docker Compose projects, containers, networks or volumes;
- Cloudflare tunnels;
- treasury, profits or fee-payer wallets; or
- deposit, sweep, withdrawal, settlement or profit workers.

A balance deposited into one version is not visible or spendable in the other. Players must sign in, activate funding where required and deposit separately on the version they choose to play.

---

## Deployment identity

| Item | EVM live beta | Solana live beta |
|---|---|---|
| Repository | `CheyneWeb3/Memopoly` | `CheyneWeb3/Memopoly-Solana` |
| Public URL | `https://memopoly.hausserver.xyz` | `https://memopoly-svm.hausserver.xyz` |
| Runtime | Independent EVM stack | Independent Solana stack |
| Compose project | EVM-specific Compose project | `memopoly-solana` |
| Database | EVM-specific Mongo database | `memopoly_solana` |
| Wallet signature | EVM signed message | Solana Ed25519 signed message |
| Token transfer model | EVM token contract events | Finalized SPL-token instructions |
| Funded-play asset | Configured USDC-compatible token | Canonical Solana USDC |
| Runtime state shared | **None** | **None** |

The Solana version was created from the working EVM game baseline while replacing the chain-specific cashier and wallet rail. It does not reference the EVM deployment's containers, volumes, databases, Redis state, tunnel, secrets, treasury or workers.

---

## Beta status

Both versions are public betas. Games, linked identities, player statistics, funding records and room state are persisted within their respective deployments, but bugs and rule-edge cases may still be found during multiplayer and funded-flow testing.

### EVM beta

The EVM deployment uses its configured chain and token contract. Some public screenshots may show `tUSDC` on chain `97`, which is a test deployment. Test tokens must not be presented as production Circle USDC.

Before real-value operation, the configured chain, token contract, treasury, watcher confirmations, settlement worker and withdrawal path must be independently reviewed and tested.

### Solana beta

The Solana deployment targets Solana `mainnet-beta` and canonical Circle USDC:

```text
Mint:     EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v
Decimals: 6
```

Arbitrary SPL tokens are rejected while canonical-USDC enforcement is enabled. Deposits, sweeps, withdrawals and profit transfers require exact finalized SPL-token transfer validation.

### Reporting a problem

Include:

- which live beta was used: EVM or Solana;
- the action that was taken;
- which player was active;
- whether the table was Free Play or Staked Play;
- the tile, modal, deposit, withdrawal or settlement step involved;
- what was expected;
- what happened instead; and
- a screenshot, transaction hash or signature and approximate time when relevant.

Never include private keys, mnemonics, seed phrases, OAuth secrets, RPC credentials or admin tokens in a report.

---

## Main features

### Multiplayer game engine

- Server-authoritative dice, movement, payments, ownership and turn deadlines.
- Private room codes and listed public games.
- Joinable waiting rooms and read-only spectator access for active games.
- Responsive board layout for desktop, portrait mobile and landscape mobile.
- Persistent player identity with refresh and reconnect support inside each deployment.
- Manual-control reclaim after autonomous or dormant play.
- Full 40-space crypto-themed board with colour groups, ramps, utilities, taxes, Jail and Free Parking.
- Property deed cards shared by tile inspection, purchase decisions and auctions.
- Houses, Hotels, even-building rules, limited building supply, Mortgages and Unmortgages.
- Player-to-player trades containing game money, properties and eligible cards.
- Two server-shuffled draw-card decks with movement, payments and other effects.
- Movement-first sequencing: the piece reaches its destination before rent, fines, purchases, payouts or destination alerts appear.
- Sequential auctions where every remaining bidder receives a turn.
- Auction buttons for `+M1`, `+M10`, `+M50` and `+M100`.
- Passing or timing out forfeits only that bidder; the remaining auction continues.
- Free Parking pot funded by configured taxes and fines.
- Jail choices, mandatory payments and three-consecutive-doubles Jail handling.
- Compact board-centre payment and destination alerts.
- Live game ledger with payment source, destination, reason and amount.
- Banker portfolio, Hall of Fame, final standings and protected admin tools.

### Accounts, identity and access

Both deployments support persistent player profiles and linked authentication methods, but account data is not shared between them.

The EVM version provides:

- signed EVM wallet login using a server-issued message;
- multiple linked EVM wallets;
- a designated primary EVM wallet for protected payment actions; and
- signed-wallet authorization for protected administration.

The Solana version provides:

- Reown Solana wallet connection;
- Ed25519 signed account, wallet-link and admin authentication;
- multiple linked Solana wallets where supported by the account flow; and
- a designated primary Solana wallet for protected funding and withdrawal actions.

Both versions provide:

- Google OAuth login for returning players;
- permanent player profiles;
- editable table name and player icon;
- protected account linking; and
- logout protection requiring at least one usable linked login method.

### Web3 funding and settlement

Each deployment maintains its own protected cashier ledger.

Common behaviour includes:

- a unique custodial deposit destination per player;
- strict network and asset validation;
- confirmed-transfer deposit crediting;
- idempotent deposit processing;
- Available, Reserved and In-game balance accounting;
- fixed Staked Play entry amounts;
- protected seat reservations;
- projected prize-pool calculation before room creation;
- seat-funding checks before a Staked Play table starts;
- winner settlement from server-authoritative game state;
- withdrawal only to the linked primary wallet;
- exact destination and amount validation;
- failed-transfer recovery from Reserved to Available when safe; and
- transaction and balance history in the Banker.

> `M` is Memopoly game currency only. It is used for board purchases, rent, taxes, trades and buildings. It is separate from USDC deposits, table entries, prize pools and withdrawals.

---

## Accounts and sign-in

Players sign in separately on the EVM and Solana deployments.

A wallet login signs a server-issued authentication message only. Signing in does **not** itself transfer tokens or spend gas. On-chain actions are presented separately and require their own explicit approval.

### EVM sign-in

The EVM version verifies ownership through a signed EVM authentication message. The primary linked EVM wallet is used for protected funding and withdrawal requests.

### Solana sign-in

The Solana version verifies ownership through an Ed25519 signature over a server-issued authentication message. Transaction approval is separate from account authentication.

<div align="center">
  <img src="docs/images/sign-in-screen.png" alt="Memopoly wallet and Google sign-in screen" width="560">
</div>

The account page lets a player update their table name and icon, review linked wallets, link another wallet, confirm their Google identity and open protected admin tools when authorized.

<div align="center">
  <img src="docs/images/account-linked-identities.png" alt="Memopoly account page with linked wallet and Google identity" width="620">
</div>

The player menu provides direct access to the Banker, account settings, Hall of Fame, server status, protected Admin Desk and logout.

<div align="center">
  <img src="docs/images/player-account-menu.png" alt="Memopoly player account menu" width="320">
</div>

---

## USDC staked play

Staked Play uses a fixed entry amount for every funded seat. The host selects the entry amount, player count, room access and ordinary gameplay settings before the room opens.

The projected player pot is:

```text
entry per player × funded player seats = table prize pool
```

For example, six funded seats at `10 USDC` create a projected `60 USDC` prize pool.

<div align="center">
  <img src="docs/images/create-staked-game.png" alt="Create a public staked Memopoly game with token entry and projected prize pool" width="560">
</div>

### Network selection

Players choose the network by opening the corresponding deployment:

- [EVM live beta](https://memopoly.hausserver.xyz)
- [Solana live beta](https://memopoly-svm.hausserver.xyz)

A table cannot combine EVM and Solana players or balances. Every seat in a funded game belongs to the same deployment and cashier ledger.

### Staked table lifecycle

1. The player opens either the EVM or Solana live beta.
2. The player signs in to an account on that deployment.
3. The player links the correct network wallet.
4. The player activates the Banker when required.
5. The player deposits the asset supported by that deployment.
6. The host creates a Staked Play room with a fixed entry per player.
7. Joining players must have enough Available balance to fund their seats.
8. Seat funds move into a protected Reserved or In-game state before play starts.
9. The game proceeds using `M` game currency; USDC is not used for rent or board purchases.
10. The server finalizes the winner from authoritative game state.
11. The protected prize pool is credited through the settlement ledger.
12. The winner may request withdrawal to the linked primary wallet on the same deployment.

Staked Play should remain disabled on any installation until the configured asset, network, treasury liquidity, deposit watcher, settlement worker and withdrawal path have passed deployment-specific testing.

---

## Banker: deposits, balances and withdrawals

### Deposit funds

Every player receives a unique custodial deposit destination within the selected deployment. The Banker can initiate a transfer from the linked wallet, display a QR code, copy the deposit destination or check an already-submitted transaction.

The EVM deposit watcher validates the configured chain, token contract, destination and matching transfer event.

The Solana deposit watcher validates the canonical USDC mint, destination token account, token-account authority, instruction amount and finalized transaction result. Deposit crediting is idempotent by transaction signature and instruction index.

<div align="center">
  <img src="docs/images/banker-deposit.png" alt="Memopoly Banker deposit screen with custodial address and linked-wallet transfer" width="700">
</div>

### Balance states

The cashier ledger separates funds into:

- **Available** — may fund a new Staked Play seat or be requested for withdrawal.
- **Reserved** — locked for a pending withdrawal, table entry or protected settlement operation.
- **In game** — committed to an active funded table until the authoritative result is finalized.

This prevents one balance from being withdrawn and committed to a table at the same time.

### Withdraw funds

Withdrawals return only to the linked primary wallet on the same deployment. The player authorizes the exact destination and amount before the request is accepted.

The requested amount moves to Reserved immediately. It returns to Available only when the transfer fails or is safely cancelled under the relevant settlement and reconciliation rules.

<div align="center">
  <img src="docs/images/banker-withdraw.png" alt="Memopoly Banker withdrawal screen with linked primary wallet destination" width="700">
</div>

---

## Solana Banker activation

Opening the Solana Banker does not automatically create a token account or spend treasury SOL. Activation is explicit.

The production default is user-funded activation:

```env
BANKER_ACCOUNT_FUNDING_MODE=user
BANKER_TREASURY_FUNDING_ENABLED=false
BANKER_ACTIVATION_FEE_USD_CENTS=50
```

The linked primary wallet signs one atomic Solana transaction that:

1. creates the custodial canonical-USDC associated token account when required;
2. pays the live token-account rent;
3. pays the network transaction fee; and
4. transfers the configured `$0.50` worth of SOL directly to the treasury as an initial future gas contribution.

The server verifies the exact quoted transaction and finalized on-chain result before activating the Banker.

Treasury-funded creation is an explicit operational override and must remain disabled by default:

```env
BANKER_ACCOUNT_FUNDING_MODE=treasury
BANKER_TREASURY_FUNDING_ENABLED=true
```

Development may deliberately set:

```env
BANKER_ACTIVATION_FEE_USD_CENTS=0
```

Production should restore:

```env
BANKER_ACTIVATION_FEE_USD_CENTS=50
```

The operational procedure is documented in [`docs/BANKER_OPERATIONS.md`](docs/BANKER_OPERATIONS.md).

---

## Basic player help

### 1. Choose a version

Open the deployment matching the wallet and network you intend to use:

- [Play the EVM live beta](https://memopoly.hausserver.xyz)
- [Play the Solana live beta](https://memopoly-svm.hausserver.xyz)

Do not send EVM assets to a Solana address or Solana assets to an EVM address.

### 2. Create or join a game

From the lobby, create a room or join an available waiting room. Share the six-character room code when using a private room.

The room owner can start after the required players have joined and, for Staked Play, every selected seat is funded.

### 3. Choose Free Play or Staked Play

Use **Free Play** for games with no token entry or settlement.

Use **Staked Play** only after checking the selected deployment, network, asset, entry amount and projected prize pool. Funding a seat commits USDC balance independently of the `M` balance used during gameplay.

### 4. Take a turn

Select **Roll Dice** when it is your turn. The server generates the roll and moves your piece across the board. The destination resolves after the movement animation finishes.

A destination may result in:

- a purchase decision;
- rent paid to another player in `M`;
- a tax or fine paid to the Banker or Free Parking pot in `M`;
- a Free Parking payout in `M`;
- a draw card;
- Jail or Just Visiting; or
- no action on an ordinary owned or neutral space.

### 5. Buy a property

When landing on an available property, the purchase modal displays the full deed card, price, rent schedule, Mortgage value and building costs.

Choose **Buy** to purchase it or decline to send it to auction.

### 6. Auctions

Auctions proceed one bidder at a time. Use the `+M1`, `+M10`, `+M50` or `+M100` buttons to raise the current bid.

- Passing forfeits only your participation.
- Letting your auction timer expire also forfeits only your participation.
- Remaining players continue bidding.
- The auction ends when one valid bidder remains or all players forfeit without a bid.

### 7. Draw cards

When a player lands on a draw-card tile, the card opens first. Close the card, or allow its display timer to finish, before the effect is applied.

When a card moves the player, the sequence is:

1. show the drawn card;
2. close the card;
3. move the piece;
4. resolve the destination; and
5. show any resulting payment, purchase, Jail or payout alert.

### 8. Rent, fines and payments

Ordinary game payments are processed by the server in `M`. A compact alert appears in the board centre after movement and identifies:

- who paid or received money;
- the amount;
- the payment reason;
- the recipient or destination; and
- the payer's remaining balance when relevant.

The same information is recorded in the live game ledger.

### 9. Free Parking

Configured taxes and fines may add `M` to the Free Parking pot. A player landing directly on Free Parking receives the pot, and the pot resets.

A Free Parking award card appears only when money is actually transferred. No payout modal appears for an empty pot.

### 10. Jail

A player may be sent to Jail by the board, a card or three consecutive doubles. The Jail modal identifies the affected player and presents the available choices when a decision is required.

Landing on the Jail corner normally means **Just Visiting**.

### 11. Banker, buildings and Mortgages

Open the in-game **Banker** to manage owned properties, build or sell Houses and Hotels, Mortgage or Unmortgage eligible assets and review your portfolio.

Buildings must follow the even-building and even-selling rules across a complete colour set.

The account-level Banker handles USDC deposits, Staked Play balances, history and withdrawals. It is separate from the in-game property-management controls.

### 12. Trades

Use the trade desk to propose exchanges containing `M`, properties and eligible cards. The receiving player can accept or reject the offer before the trade timer expires.

USDC balances and prize-pool claims are not player-to-player trade items.

### 13. Returning after inactivity

A disconnected or inactive player may enter autonomous or dormant play so the table is not permanently blocked. Return to the game and use **Take back control** or **Resume manual play** when shown.

---

## Network-specific financial rails

### EVM rail

The EVM cashier is based on:

- signed EVM wallet ownership;
- configured EVM chain validation;
- configured USDC-compatible token-contract validation;
- confirmed matching token transfer events;
- custodial deposit addresses;
- treasury sweeping;
- protected withdrawals to the linked primary EVM wallet;
- idempotent ledger operations; and
- settlement and reconciliation workers.

The exact EVM network, token contract, confirmation policy and treasury configuration are deployment settings and must be verified before funded play is enabled.

### Solana rail

The Solana cashier uses:

- Solana `mainnet-beta`;
- internal chain ID `101`;
- canonical Circle USDC mint `EPjFWdd5AufqSSqeM2qN1xzybapC8G4wEGGkZwyTDt1v`;
- required token decimals of `6`;
- canonical associated token accounts;
- finalized signature scanning;
- exact SPL-token transfer parsing;
- deterministic per-player custodial owner derivation;
- player-account-to-treasury USDC sweeps;
- an independently funded fee payer;
- treasury-to-player USDC withdrawals;
- treasury-to-profits-wallet transfers; and
- multi-RPC failover.

Arbitrary SPL tokens are rejected while:

```env
SOLANA_ENFORCE_CANONICAL_USDC=true
```

---

## Solana transaction safety model

Every outbound Solana financial intent stores the complete signed transaction bytes, signature, recent blockhash and last valid block height before broadcast.

A retry rebroadcasts the same signed bytes and therefore the same signature.

A replacement transaction is allowed only when:

1. the previous signature is finalized with an error; or
2. the blockhash has expired and at least two configured RPC endpoints unanimously report that the signature is absent.

A failed signature that is only processed or confirmed remains locked until finality.

A finalized successful transaction that fails local expectation checks is retained for reconciliation and is never automatically replaced.

Deposits are credited inside a MongoDB transaction and protected by a unique idempotency key. The Solana Compose stack initializes an authenticated single-member `rs0` replica set before application and financial workers start.

---

## System design

Memopoly separates game rules from token custody and settlement.

Common service responsibilities include:

- **Game server** — authoritative rooms, turns, dice, movement, property state, `M` payments and results.
- **Game worker** — deadlines, dormant actions, reconnect-safe progression and scheduled resolution.
- **Account service** — persistent players, friendly profiles and linked login identities.
- **Cashier ledger** — Available, Reserved and In-game token balances with auditable entries.
- **Deposit worker** — validates confirmed transfers to assigned custodial destinations.
- **Sweep worker** — consolidates supported deposited assets into the deployment treasury.
- **Withdrawal worker** — broadcasts and verifies protected player withdrawals.
- **Settlement worker** — finalizes prize-pool outcomes and recovery operations.
- **Profit worker** — transfers configured fees to the deployment's profits wallet.
- **MongoDB** — persistent users, rooms, games, balances, ledgers and transaction records.
- **Redis** — distributed locks, deadlines and short-lived coordination state.
- **Socket.IO** — real-time room and game updates.
- **Caddy and Cloudflare Tunnel** — public ingress.
- **Docker Compose** — isolated service deployment.

A browser client is never authoritative for token balance, seat funding, dice results, winner selection or prize settlement.

### Solana services

The independent Solana Compose stack defines:

- `app`
- `game-worker`
- `deposit-worker`
- `sweep-worker`
- `withdrawal-worker`
- `settlement-worker`
- `profit-worker`
- `mongo-keyfile`
- `mongo`
- `mongo-init`
- `redis`
- `caddy`
- `cloudflared`

Its deployment and operational documentation is located in:

- [`docs/SOLANA_ARCHITECTURE.md`](docs/SOLANA_ARCHITECTURE.md)
- [`docs/SOLANA_IMPLEMENTATION_MAP.md`](docs/SOLANA_IMPLEMENTATION_MAP.md)
- [`docs/SOLANA_DEPLOYMENT.md`](docs/SOLANA_DEPLOYMENT.md)
- [`docs/BANKER_OPERATIONS.md`](docs/BANKER_OPERATIONS.md)

---

## Security model

The funded game paths are designed around:

- server-issued nonces for signed-wallet authentication;
- linked-wallet ownership proven by the relevant network signature;
- one protected primary withdrawal wallet per account;
- backend-managed custodial deposit destinations;
- deposit credit based on confirmed on-chain transfers, not user-submitted amounts;
- idempotent transaction processing and unique deposit references;
- separate Available, Reserved and In-game balances;
- server-authoritative seat funding and winner selection;
- exact withdrawal destination and amount validation;
- protected admin actions requiring an authorized signed wallet;
- treasury liquidity and risk checks;
- reconciliation visibility for unexpected finalized outcomes;
- no private keys, mnemonics, OAuth secrets, RPC credentials or admin tokens in the browser bundle or repository; and
- strict isolation between the EVM and Solana stacks.

Real-value operation requires independent security review, protected secret storage, treasury controls, monitoring, incident procedures and jurisdiction-specific legal assessment.

---

## Solana local verification and deployment

From the independent Solana repository:

```bash
npm install --no-audit --no-fund
npm run typecheck
npm test
npm run build
docker compose -p memopoly-solana config >/dev/null
```

Prepare the private environment:

```bash
cp .env.example .env
chmod 600 .env
```

The intended activation order is:

1. infrastructure and health checks;
2. account and wallet authentication;
3. read-only token and treasury verification;
4. finalized deposit watching;
5. low-value sweep smoke testing;
6. withdrawal preparation without broadcast;
7. live withdrawal broadcast;
8. settlement recovery and profit transfers; and
9. value-game enablement.

Redeploy the complete independent Solana stack with:

```bash
cd /opt/memopoly-solana
bash ops/solana/redeploy-full-stack.sh
```

Do not place private keys or mnemonics in Git, shell history, issue trackers, logs or chat.

---

## Disclaimer

Memopoly is an independent game prototype. It is not affiliated with, sponsored by or endorsed by Hasbro, Monopoly, Circle, Google, Solana, any EVM network, or any cryptocurrency, token, exchange, wallet or project represented or referenced by its theme.

`M` game currency, properties, Houses, Hotels, cards and ordinary board rewards have no real-world monetary value.

Staked Play is different: it may use a configured blockchain token for table entry, prize-pool accounting and withdrawal.

The EVM deployment may use a test or production USDC-compatible token depending on its configuration. Public screenshots showing `tUSDC` on chain `97` represent a test deployment and must not be treated as production Circle USDC.

The Solana deployment targets canonical Circle USDC on Solana `mainnet-beta`. Users must still verify the network, mint, destination, amount and table terms before approving a transaction.

Blockchain transactions may be irreversible. Do not deposit unsupported assets or use funds you cannot afford to lose.

The software is beta software and is provided without a guarantee of uninterrupted operation, successful settlement or fitness for a particular purpose. Real-value public operation should not begin without security review, treasury controls, monitoring, incident procedures and legal advice.

All third-party names and marks belong to their respective owners.
