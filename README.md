import telebot


API_TOKEN = '6013962347:AAHxSASftcK1PxfvGZ3p9cYTff2xhYvKUHI'
BOT_ADMIN_ID = 556432321

bot = telebot.TeleBot(API_TOKEN)

# Language Selector
language_keyboard = telebot.types.ReplyKeyboardMarkup(row_width=1,resize_keyboard=True )
uzb_button = telebot.types.KeyboardButton('🇺🇿 O\'zbekcha')
rus_button = telebot.types.KeyboardButton('🇷🇺 Русский')
eng_button = telebot.types.KeyboardButton('🇬🇧 English')
language_keyboard.add(uzb_button, rus_button, eng_button)


@bot.message_handler(commands=['start'])
def send_welcome(message):
    user_id = message.chat.id
    user_name = message.chat.first_name

    #Welcome message with language selection menu
    welcome_message =f"🇺🇿 Assalomu alaykum {user_name.upper()} !\n" \
                    f"Iltimos tilni tanlang: \n \n" \
                    f"🇬🇧 Hello, {user_name.upper()}! \n" \
                    f"Please choose your language."

    bot.reply_to(message, welcome_message, reply_markup=language_keyboard)

@bot.message_handler(regexp="🇺🇿 O\'zbekcha")
def set_language_uzb(message):
    # set language session variable to Uzbek
    bot.reply_to(message, "Til muvaffaqiyatli o'rnatildi.", reply_markup=telebot.types.ReplyKeyboardRemove())
    bot.reply_to(message, "Savolingiz bo'lsa marhamat yozib qoldiring",)


@bot.message_handler(regexp="🇷🇺 Русский")
def set_language_rus(message):
    # set language session variable to Russian
    bot.reply_to(message, "Язык успешно изменен.", reply_markup=telebot.types.ReplyKeyboardRemove())
    bot.reply_to(message, "Если у вас есть вопрос, пожалуйста, напишите здесь")
# send technical works message
    bot.send_message(message.chat.id, "В настоящее время ведутся технические работы над ботом. "
                                      "Следите за обновлениями, мы скоро вернемся 😊")
@bot.message_handler(regexp="🇬🇧 English")
def set_language_eng(message):
    # set language session variable to English
    bot.reply_to(message, "Language changed successfully.", reply_markup=telebot.types.ReplyKeyboardRemove())
    bot.reply_to(message, "If you have a question please write here", )

@bot.message_handler(func=lambda message: True)
def reply_to_user(message):
    bot.reply_to(message,"📩Xabaringiz rahbariyatga yuborildi.\n"
                         "Hozirda bot ustida texnik ishlarni olib borayapmiz.\n"
                         "Yangilanishlarni kuzatib boring,tez orada biz qaytamiz 😊")

    #Foydalanuvchida kelgan xabarlarni admin id manziliga yuborish funksiyasi
    user_id = message.from_user.id
    user_name = message.from_user.username
    message_text = message.text
    output_text = f"📩Yangi xabar keldi \n👤Foydalanuvchi: {user_name} " \
                  f"\n🆔Foydalanuvchi ID si: {user_id} " \
                  f"\n📝Xabar: {message_text}"
    bot.send_message(chat_id=BOT_ADMIN_ID, text=output_text)

if __name__ == '__main__':
    bot.polling(none_stop=True)
