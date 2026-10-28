data class Medicine(
    val id: String,
    val name: String,
    val category: String,
    val price: Double,
    val stock: Int,
    val description: String
)
from kivy.app import App
from kivy.uix.boxlayout import BoxLayout
from kivy.uix.label import Label
from kivy.uix.textinput import TextInput
from kivy.uix.button import Button
from kivy.uix.scrollview import ScrollView
from kivy.core.window import Window
from kivy.metrics import dp


class ZaidAI(App):

    def build(self):
        Window.clearcolor = (0.06, 0.09, 0.15, 1)

        root = BoxLayout(
            orientation="vertical",
            padding=dp(12),
            spacing=dp(10)
        )

        title = Label(
            text="[b]Welcome to Zaid AI[/b]",
            markup=True,
            font_size="25sp",
            size_hint_y=None,
            height=dp(55),
            color=(0.3, 0.85, 1, 1)
        )
        root.add_widget(title)

        scroll = ScrollView()
        self.chat = BoxLayout(
            orientation="vertical",
            size_hint_y=None,
            spacing=dp(10),
            padding=dp(5)
        )
        self.chat.bind(
            minimum_height=self.chat.setter("height")
        )
        scroll.add_widget(self.chat)
        root.add_widget(scroll)
        self.scroll = scroll

        bottom = BoxLayout(
            size_hint_y=None,
            height=dp(55),
            spacing=dp(8)
        )

        self.entry = TextInput(
            hint_text="Type your message...",
            multiline=False,
            font_size="16sp"
        )
        self.entry.bind(on_text_validate=self.send)

        button = Button(
            text="Send",
            size_hint_x=0.28
        )
        button.bind(on_press=self.send)

        bottom.add_widget(self.entry)
        bottom.add_widget(button)
        root.add_widget(bottom)

        self.add_message(
            "Zaid AI",
            "Hello! Welcome to Zaid AI. Ask me anything."
        )
        return root

    def add_message(self, sender, message):
        label = Label(
            text=f"[b]{sender}:[/b] {message}",
            markup=True,
            size_hint_y=None,
            halign="left",
            valign="top",
            font_size="16sp"
        )
        label.bind(
            width=lambda obj, value:
            setattr(obj, "text_size", (value, None))
        )
        label.bind(
            texture_size=lambda obj, value:
            setattr(obj, "height", value[1] + dp(15))
        )
        self.chat.add_widget(label)

    def send(self, instance):
        message = self.entry.text.strip()
        if not message:
            return

        self.add_message("You", message)
        self.entry.text = ""

        answer = self.get_answer(message)
        self.add_message("Zaid AI", answer)
        self.scroll.scroll_y = 0

    def get_answer(self, message):
        q = message.lower()

        if "hello" in q or "hi" in q:
            return "Hello! How can I help you?"
        if "your name" in q or "naam" in q:
            return "My name is Zaid AI."
        if "python" in q:
            return "Python is a programming language."
        if "pharmacy" in q or "medicine" in q:
            return "Ask me about pharmacy learning and medicines."
        if "help" in q:
            return "Type your question in the box below."

        return (
            "I am a demo chatbot. Real AI answers "
            "will be added using an AI API."
        )


if __name__ == "__main__":
    ZaidAI().run()

