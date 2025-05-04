import telebot
from telebot import types # для указание типов
import config
import logik
from config import TOKEN
from logik import detect_material
from logik import Image
bot = telebot.TeleBot(config.TOKEN)
fraf = 0



@bot.message_handler(commands=['start','help']) #создаем команду
def start(message):
    bot.send_message(message.chat.id, "привет!Меня зовут ECObot я помогаю с ЭКО проблемами мои комманды : /globalhot")

@bot.message_handler(commands=['globalhot'])
def gh(message):
    bot.send_message(message.chat.id, "вот что главное в глобальном потеплением:Длительное повышение средней температуры климатической системы Земли для подробностей вводи: /temreture.|| Влияние на уровень моря вводи для подробностей: /sea.|| Экстремальные погодные явления для подробностей вводи: /weather. || Деградация экосистем для подробностей вводи: /ecosistem. || Угроза продовольственной безопасности для подробностей вводи: /foodsave. || Угроза для здоровья людей для подробностей вводи: /humansave.")

@bot.message_handler(commands=['temreture'])
def hh(message):
    bot.send_message(message.chat.id,'ссылка на сайт : https://ru.wikipedia.org/wiki/Глобальное_потепление а так вкратце : Длительное повышение средней температуры климатической системы Земли. Происходит уже более века, основной причиной, по мнению большинства учёных, является человеческая деятельность (антропогенный фактор).')

@bot.message_handler(commands=['sea'])
def fh(message):
    bot.send_message(message.chat.id,'ссылка на сайт : https://www.kp.ru/family/ecology/globalnoe-poteplenie/ а так вкратце : Влияние на уровень моря: ледники тают, из-за чего в океан попадает дополнительная вода. За последние 100 лет уровень Мирового океана повысился примерно на 20 см.')

@bot.message_handler(commands=['weather'])
def dh(message):
    bot.send_message(message.chat.id,'ссылку на сайт можешь взять с инфы о температуре комманда : /temreture а так вкратце : Экстремальные погодные явления: увеличение частоты волн жары, засух и ливней.')

@bot.message_handler(commands=['ecosistem'])
def qh(message):
    bot.send_message(message.chat.id,'ссылка на сайт : https://kz.kursiv.media/2024-10-07/global-warming/ а так вкратце : Деградация экосистем: из-за меняющегося климата и увеличения кислотности океанов животные и растения будут мигрировать и вымирать.')

@bot.message_handler(commands=['foodsave'])
def wh(message):
    bot.send_message(message.chat.id,'ссылку на сайт можешь взять с инфы о температуре комманда : /temreture а так вкратце : Угроза продовольственной безопасности: негативное влияние на урожайность (особенно в Азии и Африке).')

@bot.message_handler(commands=['humansave'])
def wh(message):
    bot.send_message(message.chat.id,'ссылка на сайт : https://www.un.org/ru/climatechange/science/causes-effects-climate-change а так вкратце : Угроза для здоровья людей: загрязнение воздуха, распространение заболеваний, экстремальные погодные явления увеличивают смертность и затрудняют работу систем здравоохранения.')

@bot.message_handler(content_types=['photo'])
def handle_photo(message):
    file_info = bot.get_file(message.photo[-1].file_id)
    file_name = file_info.file_path.split('/')[-1]
    downloaded_file = bot.download_file(file_info.file_path)
    with open(file_name, 'wb') as new_file:
        new_file.write(downloaded_file)
    image = Image.open(file_name)
    result, score = detect_material(image)
    bot.reply_to(message, result + '\n' + str(score))
    if result.startswith('дерево'):
        bot.send_message(message.chat.id,'Этот предмет сделан из дерева')
    elif result.startswith('пластик'):
        bot.send_message(message.chat.id,'Этот предмет сделан из пластика')
    elif result.startswith('метал'):
        bot.send_message(message.chat.id,'Этот предмет сделан из пластика')
    else:
        bot.send_message(message.chat.id,'я не знаю что это за материал!')

bot.infinity_polling()
