import asyncio
import logging
import os
from aiogram import Bot, Dispatcher, types
from aiogram.filters import Command
from aiogram.types import InlineKeyboardMarkup, InlineKeyboardButton

# ⚠️ ТОКЕНИ ХУДРО ДАР ИН ҶО ГУЗОРЕД
API_TOKEN = '8560757080:AAE3a7-R5hml1tp9W8aOkjVHlBkhd_5HlZo'

# Танзими логҳо
logging.basicConfig(level=logging.INFO)

bot = Bot(token=API_TOKEN)
dp = Dispatcher()

# Функсия барои гирифтани як калид аз файл ва нест кардани он
def get_one_vpn_key():
    file_name = "keys.txt"
    
    # Санҷиши мавҷудияти файл
    if not os.path.exists(file_name) or os.path.getsize(file_name) == 0:
        return None
        
    with open(file_name, "r", encoding="utf-8") as f:
        lines = f.readlines()
    
    if not lines:
        return None
        
    # Линки аввалинро мегирем
    chosen_key = lines[0].strip()
    
    # Сатрҳои боқимондаро аз нав ба файл менависем
    with open(file_name, "w", encoding="utf-8") as f:
        f.writelines(lines[1:])
        
    return chosen_key

# Менюи асосии бот
def get_main_menu():
    btn_buy = InlineKeyboardButton(text="💳 Гирифтани VPN", callback_data="buy_vpn")
    btn_status = InlineKeyboardButton(text="📊 Профили ман", callback_data="my_profile")
    btn_instruction = InlineKeyboardButton(text="📚 Дастурамал", callback_data="instruction")
    btn_support = InlineKeyboardButton(text="👨‍💻 Дастгирӣ", callback_data="support")
    
    keyboard = InlineKeyboardMarkup(inline_keyboard=[
        [btn_buy],
        [btn_status, btn_instruction],
        [btn_support]
    ])
    return keyboard

# Фармони /start
@dp.message(Command("start"))
async def send_welcome(message: types.Message):
    welcome_text = (
        "👋 Салом! Ба боти расмии VPN-и мо хуш омадед.\n\n"
        "🚀 Бо мо шумо метавонед ба интернети тез ва бехатар дастрасӣ пайдо кунед.\n"
        "Барои оғоз тугмаи зерро пахш кунед:"
    )
    await message.answer(welcome_text, reply_markup=get_main_menu())

# Коркарди тугмаҳо (Callback queries)
@dp.callback_query()
async def process_callback(callback_query: types.CallbackQuery):
    code = callback_query.data
    user_id = callback_query.from_user.id
    
    if code == "buy_vpn":
        await callback_query.answer()
        
        # Кӯшиши гирифтани калид аз файл
        vpn_key = get_one_vpn_key()
        
        if vpn_key:
            response_text = (
                "✅ *Линки VPN-и шумо омода аст!*\n\n"
                f"`{vpn_key}`\n\n"
                "☝️ *Бораки болои линк пахш кунед*, то он худкор нусха (копировать) шавад, ва онро ба барнома ворид кунед."
            )
            # Бо истифодаи Markdown линк дар дохили коди махсус кор мекунад, ки бо як пахш нусха мешавад
            await bot.send_message(user_id, response_text, parse_mode="Markdown")
        else:
            # Агар линкҳо дар файл тамом шуда бошанд
            await bot.send_message(
                user_id, 
                "😔 Бубахшед, ҳоло линкҳои озод тамом шудаанд.\n"
                "Лутфан ба дастгирӣ нависед, то линки нав илова кунанд: @your_admin_username"
            )
        
    elif code == "my_profile":
        await callback_query.answer()
        await bot.send_message(user_id, f"👤 Ид: `{user_id}`\n⏳ Статус: Фаъол", parse_mode="Markdown")
        
    elif code == "instruction":
        await callback_query.answer()
        
        instruction_text = (
            "📚 *Дастурамал барои пайваст шудан:*\n\n"
            "1️⃣ *Барномаро боргирӣ кунед:*\n"
            "• Барои Android барномаи *v2rayTun*-ро аз Google Play боргирӣ кунед:\n"
            "👉 https://play.google.com/store/apps/details?id=com.v2raytun.android\n\n"
            "• Барои iOS (iPhone) барномаи *Shadowrocket* ё *v2Box*-ро аз App Store боргирӣ кунед.\n\n"
            "2️⃣ *Пайваст шудан:*\n"
            "• Аз бот линк (калид)-и VPN-ро нусхабардорӣ кунед.\n"
            "• Барномаро кушоед ва аломати ҷамъ *(+)* ё *Import*-ро пахш карда, линкро илова кунед.\n"
            "• Тугмаи пайвастшавиро (Connect) пахш кунед."
        )
        # disable_web_page_preview=True расми калони Google Play-ро пинҳон мекунад, то чат тоза монад
        await bot.send_message(user_id, instruction_text, parse_mode="Markdown", disable_web_page_preview=True)
        
    elif code == "support":
        await callback_query.answer()
        await bot.send_message(user_id, "👨‍💻 Агар саволе доред, ба админ муроҷиат кунед: @your_admin_username")

async def main():
    await dp.start_polling(bot)

if __name__ == '__main__':
    asyncio.run(main())
