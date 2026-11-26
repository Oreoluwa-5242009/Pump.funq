
import random
from dataclasses import dataclass

@dataclass
class Position:
    token: str
    entry_price: float
    amount: float

class PaperBot:
    def __init__(self, capital=10.0):
        self.balance = capital
        self.positions = {}
        self.trades = []
        self.equity_history = []

    def buy(self, token, price, amount):
        if amount <= 0 or amount > self.balance:
            return

        if token in self.positions:
            return

        quantity = amount / price
        self.balance -= amount

        self.positions[token] = Position(
            token, price, quantity
        )

        print(f"BUY {token} at {price:.6f}")

    def sell(self, token, price):
        if token not in self.positions:
            return

        position = self.positions.pop(token)
        proceeds = position.amount * price
        profit = proceeds - (
            position.amount * position.entry_price
        )

        self.balance += proceeds
        self.trades.append(profit)

        print(
            f"SELL {token} | P/L: {profit:.4f} SOL"
        )

    def portfolio_value(self, prices):
        value = self.balance

        for token, position in self.positions.items():
            value += position.amount * prices[token]

        return value

    def summary(self, prices):
        equity = self.portfolio_value(prices)
        pnl = equity - 10.0

        print("\n--- PAPER BOT RESULTS ---")
        print(f"Balance: {self.balance:.4f} SOL")
        print(f"Portfolio: {equity:.4f} SOL")
        print(f"Net P/L: {pnl:.4f} SOL")
        print(f"Closed trades: {len(self.trades)}")


# Example simulated market
bot = PaperBot(capital=10.0)

prices = {
    "TOKEN_A": 0.001,
    "TOKEN_B": 0.002
}

bot.buy("TOKEN_A", prices["TOKEN_A"], 1.0)

# Simulated price movement
prices["TOKEN_A"] *= 1.15

bot.sell("TOKEN_A", prices["TOKEN_A"])

bot.summary(prices)
# Pump.funq
