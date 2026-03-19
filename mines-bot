import json
import random
import asyncio
from telegram import Update
from telegram.ext import ApplicationBuilder, CommandHandler, ContextTypes

BOT_TOKEN = "YOUR_BOT_TOKEN"
ADMIN_ID = 123456789   # apna Telegram ID daalo

FREE_CHANNEL = -1001111111111
VIP_CHANNEL = -1002222222222

VIP_FILE = "vip_users.json"

# Load VIP users
def load_vip():
    try:
        with open(VIP_FILE, "r") as f:
            return json.load(f)
    except:
        return []

def save_vip(data):
    with open(VIP_FILE, "w") as f:
        json.dump(data, f)

VIP_USERS = load_vip()

# 💎 Signal generator
def generate_signal():
    mine = random.randint(0, 24)
    grid = ["💎"] * 25
    grid[mine] = "💣"
    rows = [" ".join(grid[i:i+5]) for i in range(0,25,5)]
    return "\n".join(rows)

# 🟢 Start
async def start(update: Update, context: ContextTypes.DEFAULT_TYPE):
    await update.message.reply_text("🤖 Bot working! Use /signal")

# 🟢 Manual signal
async def signal(update: Update, context: ContextTypes.DEFAULT_TYPE):
    grid = generate_signal()
    await update.message.reply_text(f"💎 Signal:\n{grid}")

# 🟢 Add VIP
async def addvip(update: Update, context: ContextTypes.DEFAULT_TYPE):
    if update.effective_user.id != ADMIN_ID:
        return
    try:
        user_id = int(context.args[0])
        VIP_USERS.append(user_id)
        save_vip(VIP_USERS)
        await update.message.reply_text("✅ VIP added")
    except:
        await update.message.reply_text("Use: /addvip user_id")

# 🟢 Remove VIP
async def removevip(update: Update, context: ContextTypes.DEFAULT_TYPE):
    if update.effective_user.id != ADMIN_ID:
        return
    try:
        user_id = int(context.args[0])
        VIP_USERS.remove(user_id)
        save_vip(VIP_USERS)
        await update.message.reply_text("❌ VIP removed")
    except:
        await update.message.reply_text("Use: /removevip user_id")

# 🔄 Auto Signal Loop
async def auto_signal(app):
    bot = app.bot
    while True:
        grid = generate_signal()

        free_msg = f"🆓 Free Signal:\n{grid}\nUpgrade to VIP 🚀"
        vip_msg = f"💎 VIP Signal:\n{grid}\nHigh Chance 🔥"

        try:
            await bot.send_message(chat_id=FREE_CHANNEL, text=free_msg)
            await bot.send_message(chat_id=VIP_CHANNEL, text=vip_msg)
        except Exception as e:
            print("Error:", e)

        await asyncio.sleep(300)  # 5 min

# 🚀 Run bot
async def main():
    app = ApplicationBuilder().token(BOT_TOKEN).build()

    app.add_handler(CommandHandler("start", start))
    app.add_handler(CommandHandler("signal", signal))
    app.add_handler(CommandHandler("addvip", addvip))
    app.add_handler(CommandHandler("removevip", removevip))

    app.create_task(auto_signal(app))

    print("🚀 Bot Running...")
    await app.run_polling()

if __name__ == "__main__":
    import asyncio
    asyncio.run(main())
