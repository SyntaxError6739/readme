import re
import random
from datetime import datetime

class CryptoBuddy:
    def __init__(self):
        self.name = "CryptoBuddy"
        self.crypto_db = {
            "Bitcoin": {
                "symbol": "BTC",
                "price_trend": "rising",
                "market_cap": "high",
                "energy_use": "high",
                "sustainability_score": 3,
                "max_score": 10,
                "current_price": 67500,
                "risk_level": "medium",
                "use_case": "digital gold, store of value"
            },
            "Ethereum": {
                "symbol": "ETH",
                "price_trend": "stable",
                "market_cap": "high",
                "energy_use": "medium",
                "sustainability_score": 6,
                "max_score": 10,
                "current_price": 3800,
                "risk_level": "medium",
                "use_case": "smart contracts, DeFi ecosystem"
            },
            "Cardano": {
                "symbol": "ADA",
                "price_trend": "rising",
                "market_cap": "medium",
                "energy_use": "low",
                "sustainability_score": 8,
                "max_score": 10,
                "current_price": 0.65,
                "risk_level": "high",
                "use_case": "proof-of-stake blockchain, academic research"
            },
            "Solana": {
                "symbol": "SOL",
                "price_trend": "rising",
                "market_cap": "high",
                "energy_use": "low",
                "sustainability_score": 7,
                "max_score": 10,
                "current_price": 180,
                "risk_level": "high",
                "use_case": "high-speed transactions, NFTs"
            },
            "Polygon": {
                "symbol": "MATIC",
                "price_trend": "stable",
                "market_cap": "medium",
                "energy_use": "low",
                "sustainability_score": 8,
                "max_score": 10,
                "current_price": 0.85,
                "risk_level": "high",
                "use_case": "Ethereum scaling, layer 2 solution"
            }
        }
        
        self.greetings = [
            "Hey there, crypto explorer! 🚀",
            "Welcome to the crypto universe! 🌟",
            "Ready to dive into digital assets? 💎",
            "Let's find your perfect crypto match! 🎯"
        ]
        
        self.disclaimers = [
            "⚠️ Remember: Crypto is volatile - never invest more than you can afford to lose!",
            "🧠 This is educational advice - always do your own research!",
            "📊 Past performance doesn't guarantee future results!",
            "💡 Consider consulting a financial advisor for major investments!"
        ]

    def greet(self):
        greeting = random.choice(self.greetings)
        return f"{greeting}\n\nI'm {self.name}, your friendly crypto advisor! I can help you find coins based on:\n🔸 Profitability trends\n🔸 Sustainability scores\n🔸 Risk levels\n🔸 Use cases\n\nWhat would you like to know about crypto today?"

    def normalize_query(self, query):
        """Convert query to lowercase and remove extra spaces"""
        return re.sub(r'\s+', ' ', query.lower().strip())

    def find_sustainable_crypto(self):
        """Find the most sustainable cryptocurrency"""
        best_crypto = max(self.crypto_db.items(), 
                         key=lambda x: x[1]["sustainability_score"])
        
        name, data = best_crypto
        score = data["sustainability_score"]
        
        return f"🌱 **{name} ({data['symbol']})** is your greenest option!\n\n" \
               f"Sustainability Score: {score}/10 ⭐\n" \
               f"Energy Use: {data['energy_use'].title()}\n" \
               f"Use Case: {data['use_case'].title()}\n" \
               f"Current Trend: {data['price_trend'].title()}\n\n" \
               f"Perfect for eco-conscious investors! 🌍"

    def find_profitable_crypto(self):
        """Find cryptocurrencies with rising trends and high market cap"""
        rising_cryptos = [(name, data) for name, data in self.crypto_db.items() 
                         if data["price_trend"] == "rising"]
        
        if not rising_cryptos:
            return "📉 No cryptos are currently trending upward in our database."
        
        # Prioritize high market cap among rising cryptos
        best_crypto = max(rising_cryptos, 
                         key=lambda x: 1 if x[1]["market_cap"] == "high" else 0)
        
        name, data = best_crypto
        
        response = f"📈 **{name} ({data['symbol']})** is trending up!\n\n"
        response += f"Price Trend: {data['price_trend'].title()} 🚀\n"
        response += f"Market Cap: {data['market_cap'].title()}\n"
        response += f"Current Price: ${data['current_price']:,.2f}\n"
        response += f"Risk Level: {data['risk_level'].title()}\n\n"
        
        if len(rising_cryptos) > 1:
            other_rising = [crypto[0] for crypto in rising_cryptos if crypto[0] != name]
            response += f"Other rising options: {', '.join(other_rising[:2])}"
        
        return response

    def find_balanced_investment(self):
        """Find crypto with good balance of profitability and sustainability"""
        scores = {}
        for name, data in self.crypto_db.items():
            # Calculate composite score
            profit_score = 3 if data["price_trend"] == "rising" else 1
            market_score = 2 if data["market_cap"] == "high" else 1
            sustainability_score = data["sustainability_score"]
            
            composite_score = (profit_score + market_score + sustainability_score) / 3
            scores[name] = (composite_score, data)
        
        best_crypto = max(scores.items(), key=lambda x: x[1][0])
        name, (score, data) = best_crypto
        
        return f"⚖️ **{name} ({data['symbol']})** offers the best balance!\n\n" \
               f"Composite Score: {score:.1f}/10\n" \
               f"Price Trend: {data['price_trend'].title()}\n" \
               f"Sustainability: {data['sustainability_score']}/10\n" \
               f"Market Cap: {data['market_cap'].title()}\n" \
               f"Use Case: {data['use_case'].title()}\n\n" \
               f"Great for well-rounded portfolios! 💼"

    def get_crypto_info(self, crypto_name):
        """Get detailed information about a specific cryptocurrency"""
        # Try to match crypto name or symbol
        matched_crypto = None
        for name, data in self.crypto_db.items():
            if (crypto_name in name.lower() or 
                crypto_name in data["symbol"].lower()):
                matched_crypto = (name, data)
                break
        
        if not matched_crypto:
            available = ", ".join([f"{name} ({data['symbol']}) " 
                                 for name, data in self.crypto_db.items()])
            return f"🤔 I don't have data on '{crypto_name}'. Available options:\n{available}"
        
        name, data = matched_crypto
        
        return f"📊 **{name} ({data['symbol']})** Analysis:\n\n" \
               f"💰 Current Price: ${data['current_price']:,.2f}\n" \
               f"📈 Price Trend: {data['price_trend'].title()}\n" \
               f"🏢 Market Cap: {data['market_cap'].title()}\n" \
               f"🌱 Sustainability: {data['sustainability_score']}/10\n" \
               f"⚡ Energy Use: {data['energy_use'].title()}\n" \
               f"⚠️ Risk Level: {data['risk_level'].title()}\n" \
               f"🎯 Use Case: {data['use_case'].title()}\n"

    def list_all_cryptos(self):
        """List all available cryptocurrencies with basic info"""
        response = "📋 **Available Cryptocurrencies:**\n\n"
        for name, data in self.crypto_db.items():
            trend_emoji = "🚀" if data["price_trend"] == "rising" else "📊"
            eco_emoji = "🌱" if data["sustainability_score"] >= 7 else "⚡"
            
            response += f"{trend_emoji} **{name} ({data['symbol']})**\n"
            response += f"   Price: ${data['current_price']:,.2f} | "
            response += f"Trend: {data['price_trend'].title()} | "
            response += f"Eco-Score: {data['sustainability_score']}/10 {eco_emoji}\n\n"
        
        return response

    def get_investment_strategy(self, strategy_type):
        """Provide investment strategies based on user preference"""
        strategies = {
            "conservative": {
                "description": "Low-risk, stable investments",
                "criteria": lambda data: data["market_cap"] == "high" and data["risk_level"] == "medium"
            },
            "aggressive": {
                "description": "High-risk, high-reward potential",
                "criteria": lambda data: data["price_trend"] == "rising" and data["risk_level"] == "high"
            },
            "eco": {
                "description": "Environmentally conscious investments",
                "criteria": lambda data: data["sustainability_score"] >= 7
            }
        }
        
        if strategy_type not in strategies:
            return "🤷‍♂️ I offer conservative, aggressive, or eco strategies. Which interests you?"
        
        strategy = strategies[strategy_type]
        suitable_cryptos = [(name, data) for name, data in self.crypto_db.items() 
                           if strategy["criteria"](data)]
        
        if not suitable_cryptos:
            return f"😅 No cryptos match the {strategy_type} strategy criteria right now."
        
        response = f"🎯 **{strategy_type.title()} Strategy** - {strategy['description']}\n\n"
        response += "Recommended cryptos:\n"
        
        for name, data in suitable_cryptos:
            response += f"• **{name} ({data['symbol']})** - ${data['current_price']:,.2f}\n"
        
        return response

    def respond(self, user_input):
        """Main response logic"""
        if not user_input.strip():
            return "🤔 I'm here to help! Ask me about crypto trends, sustainability, or specific coins."
        
        query = self.normalize_query(user_input)
        
        # Greeting patterns
        if any(word in query for word in ["hello", "hi", "hey", "start", "begin"]):
            return self.greet()
        
        # Help patterns
        elif any(word in query for word in ["help", "what can you do", "commands"]):
            return """🔧 **Here's what I can help you with:**
            
🌱 Sustainability: "What's the most sustainable crypto?"
📈 Profitability: "Which crypto is trending up?"
⚖️ Balance: "Best balanced investment?"
📊 Specific Info: "Tell me about Bitcoin" or "BTC info"
📋 List All: "Show all cryptos"
🎯 Strategies: "Conservative/aggressive/eco strategy"
            
Just ask naturally - I'll understand! 😊"""
        
        # Sustainability queries
        elif any(word in query for word in ["sustainable", "green", "eco", "environment", "energy"]):
            return self.find_sustainable_crypto()
        
        # Profitability queries
        elif any(phrase in query for phrase in ["trending", "profitable", "rising", "profit", "trending up", "bullish"]):
            return self.find_profitable_crypto()
        
        # Balanced investment
        elif any(phrase in query for phrase in ["balanced", "balance", "best overall", "recommended"]):
            return self.find_balanced_investment()
        
        # List all cryptos
        elif any(phrase in query for phrase in ["list", "all", "available", "show all", "what crypto"]):
            return self.list_all_cryptos()
        
        # Investment strategies
        elif "strategy" in query or "approach" in query:
            if "conservative" in query:
                return self.get_investment_strategy("conservative")
            elif "aggressive" in query:
                return self.get_investment_strategy("aggressive")
            elif "eco" in query:
                return self.get_investment_strategy("eco")
            else:
                return "🎯 What strategy interests you? Conservative, aggressive, or eco-friendly?"
        
        # Specific crypto queries
        else:
            # Check if user is asking about a specific crypto
            for crypto_name in self.crypto_db.keys():
                if crypto_name.lower() in query:
                    return self.get_crypto_info(crypto_name.lower())
            
            # Check for crypto symbols
            for name, data in self.crypto_db.items():
                if data["symbol"].lower() in query:
                    return self.get_crypto_info(data["symbol"].lower())
            
            # Default response with suggestion
            return f"🤔 I'm not sure what you're looking for. Try asking:\n" \
                   f"• 'What's the most sustainable crypto?'\n" \
                   f"• 'Which crypto is trending up?'\n" \
                   f"• 'Tell me about Bitcoin'\n" \
                   f"• 'Show me all available cryptos'\n\n" \
                   f"Or type 'help' for more options!"

    def add_disclaimer(self, response):
        """Add a random disclaimer to responses"""
        disclaimer = random.choice(self.disclaimers)
        return f"{response}\n\n{disclaimer}"

    def chat(self):
        """Main chat interface"""
        print("=" * 60)
        print("🚀 Welcome to CryptoBuddy - Your Crypto Investment Advisor! 🚀")
        print("=" * 60)
        print(self.greet())
        print("\nType 'quit' or 'exit' to end the conversation.")
        print("-" * 60)
        
        while True:
            try:
                user_input = input("\nYou: ").strip()
                
                if user_input.lower() in ['quit', 'exit', 'bye', 'goodbye']:
                    print(f"\n{self.name}: Thanks for chatting! Remember to always DYOR (Do Your Own Research)! 🚀💎")
                    break
                
                response = self.respond(user_input)
                
                # Add disclaimer to investment advice
                if any(word in user_input.lower() for word in 
                      ['invest', 'buy', 'profit', 'sustainable', 'recommend']):
                    response = self.add_disclaimer(response)
                
                print(f"\n{self.name}: {response}")
                
            except KeyboardInterrupt:
                print(f"\n\n{self.name}: Goodbye! Happy investing! 🌟")
                break
            except Exception as e:
                print(f"\n{self.name}: Oops! Something went wrong. Let's try again! 😅")

# Example usage and testing
if __name__ == "__main__":
    # Create chatbot instance
    buddy = CryptoBuddy()
    
    # Interactive mode
    print("🎯 Starting CryptoBuddy in interactive mode...")
    print("🔧 To test specific features, comment out buddy.chat() and use the examples below.\n")
    
    # Uncomment to run interactive chat
    buddy.chat()
    
    # Example queries for testing (uncomment to test specific features)
    """
    print("=" * 50)
    print("🧪 TESTING CRYPTOBUDDY FEATURES")
    print("=" * 50)
    
    test_queries = [
        "What's the most sustainable crypto?",
        "Which crypto is trending up?",
        "Tell me about Bitcoin",
        "Show me all available cryptos",
        "What's the best balanced investment?",
        "Give me a conservative strategy",
        "Tell me about ADA"
    ]
    
    for query in test_queries:
        print(f"\n🔸 Query: {query}")
        print(f"🤖 Response: {buddy.respond(query)}")
        print("-" * 50)
    """
