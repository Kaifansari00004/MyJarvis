# MyJarvis
My First Repository
<br>
Developer Name : Mohammad Kaif
<br>
import kivy
<br>
from kivy.app import App
<br>
from kivy.uix.boxlayout import BoxLayout
<br>
from kivy.uix.button import Button
<br>
from kivy.uix.label import Label
<br>
from kivy.core.window import Window
<br>
import speech_recognition as sr
<br>
import pyttsx3
<br>
import random
<br>
# Set app window size (for PC testing only)
<br>
Window.size = (400, 600)
<br>
# Initialize TTS engine
<br>
engine = pyttsx3.init()
<br>
engine.setProperty("rate", 150)
<br>
# Replies
<br>
replies = {
    "how are you": "I'm fine, thank you! How about you?",
    "what is your name": "I'm your English practice assistant!",
    "hello": "Hello! Let's practice English.",
    "bye": "Goodbye! See you soon.",
}
<br>
# TTS
<br>
def speak(text):
    engine.say(text)
    engine.runAndWait()
<br>
# STT
def listen():
    recognizer = sr.Recognizer()
    with sr.Microphone() as source:
        print("Listening...")
        recognizer.adjust_for_ambient_noise(source)
        audio = recognizer.listen(source)
<br>
    try:
        command = recognizer.recognize_google(audio)
        print("You said:", command)
        return command.lower()
    except sr.UnknownValueError:
        return "Sorry, I didn’t understand that."
    except sr.RequestError:
        return "Speech service error."
<br>
# Kivy Layout
class AssistantLayout(BoxLayout):
    def __init__(self, **kwargs):
        super().__init__(orientation='vertical', padding=20, spacing=20, **kwargs)

        self.label = Label(text="Tap to Speak", font_size=24)
        self.add_widget(self.label)

        self.btn = Button(text="Start Speaking", font_size=24, size_hint=(1, 0.3))
        self.btn.bind(on_press=self.process_speech)
        self.add_widget(self.btn)

    def process_speech(self, instance):
        user_input = listen()
        self.label.text = f"You said: {user_input}"
        if "bye" in user_input:
            response = "Goodbye Kaif! Keep practicing."
        else:
            response = replies.get(user_input, random.choice([
                "That's interesting! Tell me more.",
                "Can you say that again?",
                "Let's keep talking in English.",
                "Good sentence! What else can you say?"
            ]))
        speak(response)

# Kivy App
class AssistantApp(App):
    def build(self):
        return AssistantLayout()

if __name__ == "__main__":
    AssistantApp().run()
