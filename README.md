import telebot
from telebot import types

import webbrowser


bot = telebot.TeleBot('6396240785:AAGKWA67nov0VT0OILNyc7FvsX12ff8K3m4')


@bot.message_handler(commands=['start'])
def start(message):
    markup = types.ReplyKeyboardMarkup()
    btn1 = types.KeyboardButton('Тест')
    btn2 = types.KeyboardButton('Сборник')
    markup.row(btn1, btn2)

    btn3 = types.KeyboardButton('Карты')
    btn4 = types.KeyboardButton('Сериал')
    markup.row(btn3, btn4)
    bot.send_message(message.chat.id, f'Привет, {message.from_user.first_name} ', reply_markup=markup)
    bot.register_next_step_handler(message, on_click)

def on_click(message):
    if message.text == 'Сборник':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton('Перейти на сайт', url='http://al-hist.ru/Posobija_dla_podgotowki/Баранов%20П.А.%20История.%20Новый%20полный%20справочник%20для%20подготовки%20к%20ЕГЭ.%20-%20М.%20АСТ,%202017.%20-%20495%20с..pdf'))
        bot.reply_to(message, 'Сборник Баранова', reply_markup=markup)
    elif message.text == 'Сериал':
        markup = types.InlineKeyboardMarkup()
        markup.add(types.InlineKeyboardButton('Перейти на сайт',url='https://www.ivi.ru/watch/ryurikovichi?ysclid=lnbqyza59o626453424'))
        bot.reply_to(message, 'Рюриковичи', reply_markup=markup)


@bot.message_handler(commands=['website','site'])
def site(message):
    markup = types.InlineKeyboardMarkup()
    markup.add(types.InlineKeyboardButton('Перейти на сайт', url='https://historyandcats.ru/kurs?utm_source=vk&utm_medium=rassilka&utm_campaign=site'))
    bot.reply_to(message, 'История и котики', reply_markup=markup)


@bot.message_handler(content_types=['photo'])
def get_photo(message):
    markup = types.InlineKeyboardMarkup()
    del_ph = types.InlineKeyboardButton('Удалить фото', callback_data='delete')
    edit_ph = types.InlineKeyboardButton('Изменить фото', callback_data='edit')
    markup.row(del_ph, edit_ph)
    bot.reply_to(message, f'{message.from_user.first_name},прекрасней и красивее девушки я не видел',reply_markup=markup)

@bot.callback_query_handler(func=lambda callback: True)
def callback_message(callback):
    if callback.data == 'delete':
        bot.delete_message(callback.message.chat.id, callback.message.message_id - 1)
    elif callback.data == 'edit':
        bot.edit_message_text('edit',callback.message.chat.id, callback.message.message_id)

@bot.message_handler()
def info(message):
    if message.text.lower() == 'привет':
        bot.send_message(message.chat.id, f'Привет, {message.from_user.first_name}')
    elif message.text.lower() == 'id':
        bot.reply_to(message, f'ID:{message.from_user.id}')

bot.polling(none_stop=True)
