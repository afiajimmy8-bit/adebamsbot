import os
import logging
import httpx
from urllib.parse import quote
from telegram import Update
from telegram.ext import ApplicationBuilder, CommandHandler, MessageHandler, ContextTypes, filters

logging.basicConfig(
    format="%(asctime)s - %(name)s - %(levelname)s - %(message)s",
    level=logging.INFO,
)
logger = logging.getLogger(__name__)

BOT_TOKEN = os.environ.get("TELEGRAM_BOT_TOKEN")
if not BOT_TOKEN:
    raise RuntimeError("TELEGRAM_BOT_TOKEN environment variable is not set")


async def start(update: Update, context: ContextTypes.DEFAULT_TYPE):
    await update.message.reply_text(
        "👋 Welcome to @adebamsbot!\n\n"
        "Send me any text prompt and I'll generate an AI image for you.\n\n"
        "Example:\n"
        "a cat riding a skateboard in space, digital art"
    )


async def generate_image(update: Update, context: ContextTypes.DEFAULT_TYPE):
    prompt = (update.message.text or "").strip()
    if not prompt:
        await update.message.reply_text("Please send a text prompt describing the image you want.")
        return

    await update.message.reply_chat_action("upload_photo")
    status_msg = await update.message.reply_text("🎨 Generating your image, hold on...")

    try:
        encoded_prompt = quote(prompt)
        image_url = (
            f"https://image.pollinations.ai/prompt/{encoded_prompt}"
            f"?width=1024&height=1024&nologo=true"
        )

        async with httpx.AsyncClient(timeout=90.0) as client:
            response = await client.get(image_url)
            response.raise_for_status()
            image_bytes = response.content

        await update.message.reply_photo(photo=image_bytes, caption=f"✨ {prompt}")
        await status_msg.delete()
    except Exception as e:
        logger.error(f"Image generation failed: {e}")
        await status_msg.edit_text("❌ Sorry, something went wrong generating that image. Please try again.")


def main():
    app = ApplicationBuilder().token(BOT_TOKEN).build()
    app.add_handler(CommandHandler("start", start))
    app.add_handler(MessageHandler(filters.TEXT & ~filters.COMMAND, generate_image))
    logger.info("Bot starting...")
    app.run_polling()


if __name__ == "__main__":
    main()
