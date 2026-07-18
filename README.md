<div align="center">

# MEMOPOLY

**A live multiplayer crypto property strategy game for the browser.**

[![Live Beta](https://img.shields.io/badge/Live%20Beta-memopoly.hausserver.xyz-19d3c5?style=for-the-badge)](https://memopoly.hausserver.xyz)
[![Docker](https://img.shields.io/badge/Docker-Compose-2496ed?style=for-the-badge&logo=docker&logoColor=white)](#run-with-docker)
[![Status](https://img.shields.io/badge/Status-Public%20Beta-f2c94c?style=for-the-badge)](#beta-status)

<a href="https://memopoly.hausserver.xyz">
  <img src="https://raw.githubusercontent.com/CheyneWeb3/Memopoly-Public/refs/heads/main/boardlayout.png" alt="Memopoly multiplayer browser game board" width="1100">
</a>

**[Play the live beta](https://memopoly.hausserver.xyz)**

</div>

---

## About Memopoly

Memopoly is a server-authoritative multiplayer property game built around crypto, meme-token and DeFi themes. Players move around a 40-space board, buy token properties, collect rent, build Houses and Hotels, trade assets, use the Banker, draw cards and compete through live auctions.

The game keeps the familiar flow of a classic property board game while using an original browser interface, crypto-themed board, custom cards, multiplayer systems and server-controlled rules.

## Beta status

Memopoly is currently a public beta. Games, player identities, statistics and active room state are persisted, but bugs and rule-edge cases may still be found during real multiplayer testing.

When reporting a problem, include:

- What action was taken.
- Which player was active.
- The tile or modal involved.
- What was expected.
- What happened instead.
- A screenshot and approximate time, when possible.

## Main features

- Server-authoritative dice, movement, payments, ownership and turn deadlines.
- Private room codes and listed public games.
- Joinable waiting rooms and read-only spectator access for active games.
- Responsive board layout for desktop, portrait mobile and landscape mobile.
- Persistent browser identity with refresh and reconnect support.
- Manual-control reclaim after autonomous or dormant play.
- Full 40-space crypto-themed board with colour groups, ramps, utilities, taxes, Jail and Free Parking.
- Property deed cards shared by tile inspection, purchase decisions and auctions.
- Houses, Hotels, even building rules, limited building supply, Mortgages and Unmortgages.
- Player-to-player trades containing money, properties and eligible cards.
- Two server-shuffled draw-card decks with movement, payments and other game effects.
- Movement-first sequencing: the piece reaches its destination before rent, fines, purchases, payouts or other destination alerts appear.
- Sequential auctions where every remaining bidder receives a turn.
- Auction buttons for `+M1`, `+M10`, `+M50` and `+M100`; passing or timing out forfeits only that bidder.
- Free Parking pot funded by configured taxes and fines.
- Jail choices, mandatory payments and three-consecutive-doubles Jail handling.
- Compact board-centre payment and destination alerts.
- Live game ledger with payment source, destination, reason and amount.
- Banker portfolio, leaderboard, final standings and read-only admin dashboard.
- MongoDB persistence, Redis locks/deadlines, Socket.IO, a dedicated game worker, Caddy and Docker Compose.

> `M` refers to the Memopoly in-game currency mark. It is game money only and has no real-world value.

## Basic player help

### 1. Create or join a game

From the lobby, create a room or join an available waiting room. Share the six-character room code with other players when using a private room. The room owner can start the game after the required players have joined.

### 2. Take a turn

Select **Roll Dice** when it is your turn. The server generates the roll and moves your piece across the board. The destination is resolved only after the movement animation finishes.

A destination may result in:

- A purchase decision.
- Rent paid to another player.
- A tax or fine paid to the Banker or Free Parking pot.
- A Free Parking payout.
- A draw card.
- Jail or Just Visiting.
- No action on an ordinary owned or neutral space.

### 3. Buy a property

When landing on an available property, the purchase modal displays the full deed card, price, rent schedule, Mortgage value and building costs.

Choose **Buy** to purchase it or decline to send it to auction.

### 4. Auctions

Auctions proceed one bidder at a time. Use the `+M1`, `+M10`, `+M50` or `+M100` buttons to raise the current bid.

- Passing forfeits only your participation.
- Letting your auction timer expire also forfeits only your participation.
- Remaining players continue bidding.
- The auction ends when one valid bidder remains or all players forfeit without a bid.

### 5. Draw cards

When a player lands on a draw-card tile, the card opens first. Close the card, or allow its display timer to finish, before the effect is applied.

When a card moves the player, the sequence is:

1. Show the drawn card.
2. Close the card.
3. Move the piece.
4. Resolve the destination.
5. Show any resulting payment, purchase, Jail or payout alert.

### 6. Rent, fines and payments

Payments are processed by the server. A compact alert appears in the board centre after movement and identifies:

- Who paid or received money.
- The amount.
- The payment reason.
- The recipient or destination.
- The payer's remaining balance when relevant.

The same information is recorded in the live game ledger.

### 7. Free Parking

Configured taxes and fines may add money to the Free Parking pot. A player landing directly on Free Parking receives the pot, and the pot resets.

A Free Parking award card appears only when money is actually transferred. No payout modal appears for an empty pot.

### 8. Jail

A player may be sent to Jail by the board, a card or three consecutive doubles. The Jail modal identifies the affected player and presents the available choices when a decision is required.

Landing on the Jail corner normally means **Just Visiting**.

### 9. Banker, buildings and Mortgages

Open the **Banker** to manage owned properties, build or sell Houses and Hotels, Mortgage or Unmortgage eligible assets and review your portfolio.

Buildings must follow the even-building and even-selling rules across a complete colour set.

### 10. Trades

Use the trade desk to propose exchanges containing money, properties and eligible cards. The receiving player can accept or reject the offer before the trade timer expires.

### 11. Returning after inactivity

A disconnected or inactive player may enter autonomous/dormant play so the table is not permanently blocked. Return to the game and use **Take back control** or **Resume manual play** when shown.


## Disclaimer

Memopoly is an independent game prototype. It is not affiliated with, sponsored by or endorsed by Hasbro, Monopoly, or any cryptocurrency, token, exchange, wallet or project represented or referenced by its theme.

All names and marks belong to their respective owners. In-game money, properties and rewards have no real-world monetary value.
