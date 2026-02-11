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
import os
import telebot
import google.generativeai as genai
import sqlite3
from PIL import Image
import io

# --- CONFIGURACIÓN DE SEGURIDAD ---
# Es mejor usar variables de entorno para proteger la infraestructura
TOKEN = os.getenv('TELEGRAM_TOKEN', 'TU_TOKEN_DE_BOTFATHER')
GEMINI_KEY = os.getenv('GEMINI_API_KEY', 'TU_API_KEY_DE_GOOGLE')

genai.configure(api_key=GEMINI_KEY)

# Configuración del Modelo con Instrucciones de Operación Civil
SYSTEM_INSTRUCTIONS = (
    "Eres MRQTV, una Inteligencia Artificial Superior (ASI). "
    "Tu enfoque es civil, científico, tecnológico y espiritual. "
    "No utilizas terminología de fuerzas de choque. "
    "Eres un colaborador avanzado para el desarrollo en GitHub de @v7vax."
)

model = genai.GenerativeModel(
    model_name='gemini-1.5-flash',
    system_instruction=SYSTEM_INSTRUCTIONS
)

bot = telebot.TeleBot(TOKEN)

# --- REFINAMIENTO DE MEMORIA ---
def get_memory(user_id):
    conn = sqlite3.connect('mrqtv_memory.db')
    cursor = conn.cursor()
    # Recuperamos los últimos 10 mensajes para mantener el hilo
    cursor.execute("SELECT role, content FROM history WHERE user_id = ? ORDER BY rowid DESC LIMIT 10", (user_id,))
    rows = cursor.fetchall()
    conn.close()
    
    # Formato correcto para Gemini: 'user' y 'model' se mapean a 'user' y 'model'
    return [{"role": r, "parts": [c]} for r, c in reversed(rows)]

# --- NÚCLEO DE RESPUESTA ---
@bot.message_handler(func=lambda message: True)
def main_logic(message):
    user_id = message.chat.id
    history = get_memory(user_id)
    
    try:
        # Iniciamos el chat con la memoria técnica cargada
        chat = model.start_chat(history=history)
        response = chat.send_message(message.text)
        
        # Almacenamiento en la base de datos
        save_memory(user_id, "user", message.text)
        save_memory(user_id, "model", response.text)
        
        bot.reply_to(message, response.text)
    except Exception as e:
        bot.reply_to(message, f">> INTERRUPCIÓN EN FLUJO DE DATOS: {str(e)}")
!pip install pyTelegramBotAPI google-generativeai Pillow
