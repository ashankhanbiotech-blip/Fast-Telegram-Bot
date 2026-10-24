import os
import random
import threading
import asyncio
from flask import Flask
from telegram import Update
from telegram.ext import Application, CommandHandler, ContextTypes

TOKEN = os.environ.get("BOT_TOKEN")
MY_CHAT_ID = int(os.environ.get("MY_CHAT_ID", "6746993786"))

app = Flask(__name__)

@app.route("/")
def home(): return "Bot Alive!"
@app.route("/health")
def health(): return "OK", 200

def run_flask():
    app.run(host="0.0.0.0", port=int(os.environ.get("PORT", 10000)), use_reloader=False)

async def start(update: Update, context: ContextTypes.DEFAULT_TYPE):
    await update.message.reply_text("✅ Bot ON hai Ashan! /signal likho")

async def signal_cmd(update: Update, context: ContextTypes.DEFAULT_TYPE):
    pair = random.choice(["EURUSD","GBPUSD","USDJPY","GOLD"])
    direction = random.choice(["BUY 🟢","SELL 🔴"])
    conf = random.randint(75,89)
    msg = f"⚡ FAST SIGNAL\n\n{pair} - {direction}\nTime: 1 MIN\nConfidence: {conf}%\nEntry: Abhi Lo!"
    await update.message.reply_text(msg)
    if update.effective_chat.id != MY_CHAT_ID:
        await context.bot.send_message(chat_id=MY_CHAT_ID, text=msg)

async def main():
    if not TOKEN:
        raise RuntimeError("BOT_TOKEN missing!")
    application = Application.builder().token(TOKEN).build()
    application.add_handler(CommandHandler("start", start))
    application.add_handler(CommandHandler("signal", signal_cmd))
    await application.initialize()
    await application.start()
    await application.updater.start_polling(drop_pending_updates=True)
    await asyncio.Event().wait()

if __name__ == "__main__":
    threading.Thread(target=run_flask, daemon=True).start()
    asyncio.run(main())
