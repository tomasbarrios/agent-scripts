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

Para audios en español, forzar el idioma con `--language Spanish` (si no, whisper autodetecta y a veces se equivoca).

## Modo local guiado (archivos grandes)

Cuando el archivo es **grande** y whisper está **instalado en la máquina del usuario**, no lo transcribas tú: entrégale el comando listo para que lo corra local. Evita mover/subir un archivo pesado y aprovecha su CPU/GPU.

Solo entra si se cumplen **las dos** condiciones, verificadas con comandos (no asumir):

1. Archivo grande: `stat -c%s "$audio"` supera 100 MB (≈ `104857600` bytes).
2. Instalación confirmada: `command -v whisper` devuelve una ruta **y** `whisper --help` sale con código 0.

Si ambas se cumplen → entrega el comando y pide que pegue el `.txt` resultante:

```
whisper "$audio" --language Spanish --model medium --output_format txt
```

Para diarización, el equivalente con `whisperx` (previa carga de `HF_TOKEN`).

En cualquier otro caso (archivo chico, o whisper no confirmado) → transcribe tú mismo como arriba.
