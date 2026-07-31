---
name: speech-to-text
description: >-
  Transcribe un archivo de audio a texto con whisper (o whisperx cuando hace
  falta distinguir quién habla). Usar cuando se pasa un audio — .m4a, .mp3,
  .wav, una nota de voz, una grabación de reunión o entrevista — y se pide
  transcribirlo, pasarlo a texto, o resumir lo que se dijo. Si el audio es una
  conversación, pregunta primero si se necesita diarización de speakers.
---

# Speech to text

Cuando se te pase un archivo de audio, y el contexto indique que hay una conversacion, pregunta primero si se necesita distinguir quién habla (diarización de speakers).

- Si no se necesita distinguir hablantes: transcribe usando whisper.
- Si se necesita distinguir hablantes: transcribe usando whisperx (requiere token de HuggingFace para el modelo de diarización de pyannote). El token vive en `~/.config/whisperx/.env` (HF_TOKEN) — cárgalo con `source ~/.config/whisperx/.env` antes de invocar whisperx. Si el archivo no existe o no tiene HF_TOKEN, pide el secreto con `bb secret request HF_TOKEN --write-env ~/.config/whisperx/.env` en vez de preguntarle al usuario el valor.
