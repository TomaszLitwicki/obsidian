# SpeechRecognition
`pip install SpeechRecognition`
https://ffmpeg.org/
FFmpeg to zestaw narzędzi wiersza poleceń (open source), który potrafi:
- konwertować dowolne formaty audio/wideo na inne (np. .mp3 → .wav),
- wycinać fragmenty, zmieniać bitrate, kodeki, częstotliwość próbkowania, kanały itp.,
- odczytywać nagrania z prawie każdego istniejącego formatu i strumienia.

W kontekście SpeechRecognition:
- jeśli podasz np. plik .mp3, biblioteka wywoła wewnętrznie FFmpeg, aby przekonwertować go na czysty .wav (bo tylko taki może analizować);
- jeśli nie masz zainstalowanego FFmpeg, dostaniesz błąd typu „ffmpeg not found” lub „Error reading audio data from file”.
## OpenAI Whisper API
```Python
import whisper

# Wczytanie modelu (można wybrać: tiny, base, small, medium, large)
model = whisper.load_model("small")

# Transkrypcja pliku audio
result = model.transcribe("Nagranie.m4a", language="pl", fp16=False)

# Wyświetlenie rozpoznanego tekstu
print(result["text"])
```

Można skorzystać GPU i fp16, ale trzeba obsłużyć framework'a biblioteki PyTorch, która potrafi korzystać z GPU NVIDIA (przez CUDA i cuDNN), a nie tylko z CPU.
```Python
from faster_whisper import WhisperModel

# Wczytanie modelu GPU:
model = WhisperModel("small", device="cuda", compute_type="float16")
segments, info = model.transcribe("Nagranie.m4a", language="pl")

print("language:", info.language, "prob:", info.language_probability)
print("".join(s.text for s in segments))
```

Przechwytywanie z mikrofonu i zapisywanie do pliku tymczasowego.
```Python
import speech_recognition as sr
import whisper
import tempfile

r = sr.Recognizer()
mic =sr.Microphone(device_index=1) #wybór mikrofonu

# Nagranie z mikrofonu
with mic as source:
    r.adjust_for_ambient_noise(source, duration=0.5)
    r.pause_threshold = 2
    print("Mów (nagrywam maksymalnie przez 10 sekund lub do pauzy dwóch sekund ciszy)...")
    audio = r.listen(source, phrase_time_limit=10)
    print("Nagranie zakończone!")

# Zapis nagrania do pliku tymczasowego
wav_bytes = audio.get_wav_data(convert_rate=16000, convert_width=2)
with tempfile.NamedTemporaryFile(suffix=".wav", delete=False) as tmp:
    tmp.write(wav_bytes)
    audio_path = tmp.name
    print(f"Plik tymczasowy: {audio_path}")

# Transkrypcja w modelu lokalnym
model = whisper.load_model("base")
result = model.transcribe(audio_path, language='pl', fp16=False)

print("\nTranskrypcja:")
print(result["text"])
```

