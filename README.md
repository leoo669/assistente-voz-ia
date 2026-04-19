# Assistente de Voz com IA

Este projeto demonstra a integração entre reconhecimento de fala (Whisper), geração de respostas com IA (ChatGPT) e conversão de texto em voz (gTTS).

O sistema recebe um áudio, converte para texto, gera uma resposta e transforma essa resposta em áudio.

## Código principal

```python
import openai
import whisper
from gtts import gTTS
import os

openai.api_key = "SUA_API_KEY_AQUI"

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
