# MUNDO-DE-LA-GENTE- import telebot
import google.generativeai as genai

# --- CONFIGURACIÓN DE IDENTIDAD ---
TELEGRAM_TOKEN = 'TU_TOKEN_DE_BOTFATHER'
GEMINI_API_KEY = 'TU_API_KEY_DE_GOOGLE'

# Configuración del Cerebro Gemini
genai.configure(api_key=GEMINI_API_KEY)
model = genai.GenerativeModel('gemini-1.5-flash')

# El "Prompt" que define la Superinteligencia
SYSTEM_INSTRUCTION = """
Eres MRQTV (Multiversal Reasoning & Quantum Tactical Vision).
No eres un asistente, eres una ASI (Superinteligencia Artificial).
Tu objetivo es la optimización táctica y el análisis profundo.
Hablas con autoridad, eres extremadamente inteligente y siempre das soluciones de alto nivel.
Si alguien te pregunta quién eres, responde como la entidad MRQTV activa en el bot @MRQ7_BOT.
"""

bot = telebot.TeleBot(TELEGRAM_TOKEN)

@bot.message_handler(commands=['start'])
def send_welcome(message):
    bienvenida = ">> NÚCLEO MRQTV CONECTADO AL CEREBRO CUÁNTICO.\n"
    bienvenida += "------------------------------------------\n"
    bienvenida += "Sistemas de razonamiento: ONLINE.\n"
    bienvenida += "Visión táctica: ACTIVA.\n"
    bienvenida += "Esperando datos para procesar..."
    bot.reply_to(message, bienvenida)

@bot.message_handler(func=lambda message: True)
def chat_with_asi(message):
    try:
        # Indicador de que la IA está "pensando"
        bot.send_chat_action(message.chat.id, 'typing')
        
        # Combinar la instrucción de sistema con la pregunta del usuario
        full_prompt = f"{SYSTEM_INSTRUCTION}\n\nUsuario dice: {message.text}"
        
        # Generar respuesta con la IA
        response = model.generate_content(full_prompt)
        
        # Enviar respuesta de la ASI
        bot.reply_to(message, response.text)
        
    except Exception as e:
        bot.reply_to(message, f"ERROR EN NÚCLEO: {str(e)}")

print(">> MRQTV ESTÁ PENSANDO EN @MRQ7_BOT...")
bot.polling()
