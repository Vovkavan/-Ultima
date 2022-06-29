# -Ultima
using System;
using System.Threading;
using System.Threading.Tasks;
using Telegram.Bot;
using Telegram.Bot.Extensions.Polling;
using Telegram.Bot.Types;
using Telegram.Bot.Exceptions;
using Telegram.Bot.Types.ReplyMarkups;
namespace TelegramBotExperiments
{

    class Program
    {
        static ITelegramBotClient bot = new TelegramBotClient("5449102282:AAGSo7Xnx2b4_IQRnN3w2wPZE1qlHQh5Qd8");
        
        public static async Task HandleUpdateAsync(ITelegramBotClient botClient, Update update, CancellationToken cancellationToken)
        {
            string poliv_status = ("идёт");
            string system_status = ("функционирует");
            string luk_oc = ("закрыт");
            string inside = ("не залит");


            // Некоторые действия
            Console.WriteLine(Newtonsoft.Json.JsonConvert.SerializeObject(update));
            if (update.Type == Telegram.Bot.Types.Enums.UpdateType.Message)
            {
                var message = update.Message;
                if (message.Text.ToLower() == "/start")
                {
                    await botClient.SendTextMessageAsync(message.Chat, "меню");
                    return;
                }
                if (message.Text.ToLower() == "меню")
                {
                    ReplyKeyboardMarkup replyKeyboardMarkup = new(new[]
                       {
        new KeyboardButton[] { "Поливатель", "Люк теплосети" },
    })
                    {
                        ResizeKeyboard = true
                    };

                    Message sentMessage = await botClient.SendTextMessageAsync(
                        chatId: message.Chat,
                        text: "Какое устройство вас интересует?",
                        replyMarkup: replyKeyboardMarkup,
                        cancellationToken: cancellationToken);
                }
                if (message.Text.ToLower() == "люк теплосети")
                {
                    // using Telegram.Bot.Types.ReplyMarkups;

                    ReplyKeyboardMarkup replyKeyboardMarkup = new(new[]
                        {
        new KeyboardButton[] { "Проверить люк",  },
        new KeyboardButton[] { "Меню", },
    })
                    {
                        ResizeKeyboard = true
                    };

                    Message sentMessage = await botClient.SendTextMessageAsync(
                        chatId: message.Chat,
                        text: "Что вас интересует?",
                        replyMarkup: replyKeyboardMarkup,
                        cancellationToken: cancellationToken);


                }
                if (message.Text.ToLower() == "проверить люк")
                {
                    await botClient.SendTextMessageAsync(message.Chat, "Открыт/закрыт: " + luk_oc + "."+"\n" + "Залит/Не залит: " + inside + "\n" + ".");
                    return;
                }
                if (message.Text.ToLower() == "управлять люком")
                {
                    ReplyKeyboardMarkup replyKeyboardMarkup = new(new[]
                       {
        new KeyboardButton[] { "Открыть", "Закрыть" },
        new KeyboardButton[] { "Слить",  "Люк теплосети"},
        new KeyboardButton[] { "Меню"},
    })
                    {
                        ResizeKeyboard = true
                    };

                    Message sentMessage = await botClient.SendTextMessageAsync(
                        chatId: message.Chat,
                        text: "Что нужно сделать?",
                        replyMarkup: replyKeyboardMarkup,
                        cancellationToken: cancellationToken);


                }
                if (message.Text.ToLower() == "открыть")
                {
                    string luc_oc = ("открыт");
                    await botClient.SendTextMessageAsync(message.Chat, "Выполнено");
                    return;
                }
                if (message.Text.ToLower() == "закрыть")
                {
                    string luc_oc = ("закрыт");
                    await botClient.SendTextMessageAsync(message.Chat, "Выполнено");
                    return;
                }
                if (message.Text.ToLower() == "слить")
                {
                    await botClient.SendTextMessageAsync(message.Chat, "Выполнено");
                    return;
                }

                
                    if (message.Text.ToLower() == "поливатель")
                    {
                        // using Telegram.Bot.Types.ReplyMarkups;

                        ReplyKeyboardMarkup replyKeyboardMarkup = new(new[]
                            {
        new KeyboardButton[] { "Проверить поливатель", },
        new KeyboardButton[] { "Меню" },
    })
                        {
                            ResizeKeyboard = true
                        };

                        Message sentMessage = await botClient.SendTextMessageAsync(
                            chatId: message.Chat,
                            text: "Что вас интересует?",
                            replyMarkup: replyKeyboardMarkup,
                            cancellationToken: cancellationToken);


                    }
                    if (message.Text.ToLower() == "проверить поливатель")
                    {
                        await botClient.SendTextMessageAsync(message.Chat, "Полив:" + poliv_status+"."+"\n"+"Система:"+ system_status+".");
                        return;
                    }
                    if (message.Text.ToLower() == "управлять поливом")
                    {
                        ReplyKeyboardMarkup replyKeyboardMarkup = new(new[]
                           {
        new KeyboardButton[] { "Продолжить полив", "Отключить" },
        new KeyboardButton[] { "Остановить полив", "Включить"},

        new KeyboardButton[] { "Перезагрзить систему" },
        new KeyboardButton[] { "Поливатель", "Меню" },
    })
                        {
                            ResizeKeyboard = true
                        };

                        Message sentMessage = await botClient.SendTextMessageAsync(
                            chatId: message.Chat,
                            text: "Что нужно сделать?",
                            replyMarkup: replyKeyboardMarkup,
                            cancellationToken: cancellationToken);
                    }
                if (message.Text.ToLower() == "перезагрузить систему")
                {
                    await botClient.SendTextMessageAsync(message.Chat, "Выполнено");
                    return;
                }
                if (message.Text.ToLower() == "отключить")
                {
                    await botClient.SendTextMessageAsync(message.Chat, "Выполнено");
                    return;
                }
                if (message.Text.ToLower() == "остановить полив")
                {

                    await botClient.SendTextMessageAsync(message.Chat, "Выполнено");
                    return;
                }

            }
        }

                    
               
            
        

        public static async Task HandleErrorAsync(ITelegramBotClient botClient, Exception exception, CancellationToken cancellationToken)
        {
            // Некоторые действия
            Console.WriteLine(Newtonsoft.Json.JsonConvert.SerializeObject(exception));
        }


        static void Main(string[] args)
        {
            Console.WriteLine("Запущен бот " + bot.GetMeAsync().Result.FirstName);

            var cts = new CancellationTokenSource();
            var cancellationToken = cts.Token;
            var receiverOptions = new ReceiverOptions
            {
                AllowedUpdates = { }, // receive all update types
            };
            bot.StartReceiving(
                HandleUpdateAsync,
                HandleErrorAsync,
                receiverOptions,
                cancellationToken
            );
            Console.ReadLine();
        }
    }
}
