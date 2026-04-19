import openai
import whisper
from gtts import gTTS
import os

# Defina sua chave da OpenAI
openai.api_key = "SUA_API_KEY_AQUI"

# Carrega o modelo do Whisper
model = whisper.load_model("base")

def transcrever_audio(caminho_audio):
    resultado = model.transcribe(caminho_audio)
    return resultado["text"]

def perguntar_chatgpt(texto):
    resposta = openai.ChatCompletion.create(
        model="gpt-3.5-turbo",
        messages=[{"role": "user", "content": texto}]
    )
    return resposta["choices"][0]["message"]["content"]

def gerar_audio(texto):
    tts = gTTS(text=texto, lang="pt")
    arquivo_saida = "resposta.mp3"
    tts.save(arquivo_saida)
    
    # Abre o áudio automaticamente no Windows
    os.system(f"start {arquivo_saida}")

if __name__ == "__main__":
    caminho_audio = "audio.wav"

    print("Processando áudio...")
    texto = transcrever_audio(caminho_audio)
    print("Texto reconhecido:", texto)

    print("Consultando o modelo...")
    resposta = perguntar_chatgpt(texto)
    print("Resposta gerada:", resposta)

    print("Gerando áudio da resposta...")
    gerar_audio(resposta)
