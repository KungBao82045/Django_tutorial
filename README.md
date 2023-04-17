## Lets get started!

Step 1: Lag en ny mappe som heter “templates” I my_django_chatbot/chatbot_app. Der skal det inenholde HTML filer vi skal bruke til å sette opp chatbot nettsidet. Inn i templates, lag en HTML fil kalt "yourbot.html". Der skal det inneholde:

```html
<!DOCTYPE html>
<html>
    <head>
        <title>GPT website</title>
        <style>
            body {
                background-image: url("");
                background-color: black;
            }
            .main {
                margin: 50px;
                padding: 50px;
                background-color: lightgreen;
            }
            .user_response {
                background-color: lightcoral;
                padding: 50px;
            }
            .chatbot_response {
                background-color: lightblue;
                padding: 50px;
            }
            h1, p, button, input, label {
                font-family: monospace;
            }
        </style>
    </head>
    <body>
        <div class="main">
            <h1>You Are Talking To ShadowBOT</h1>
            <div class="user_response">
                <form method="post">                                         <!-- Kaller method="post" for at views.py henter input fra nettsiden -->
                    {% csrf_token %}                                         <!-- Security token when sending API -->
                    <label for="user_input">Enter your input:</label>        
                    <input type="text" id="user_input" name="user_input">    <!-- her har jeg definert id for at django views.py kan få hente input -->
                    <button type="submit">Submit</button>                    <!-- Sender input til views.py -->
                </form>
            </div>

            <br>

            <div class="chatbot_response">           
                <p>You typed: {{ user_input }}</p>   <!-- Hva du har skrevet -->
                <p>ShadowBOT: {{ reply_user }}</p>   <!-- Hva AI svarer -->
            </div>

        </div>
        
    </body>
</html>



```

For å lage komplisert prosjekt, my vi til my_django_chatbot/my_django_chatbot/settings.py og legg til "chatbot_app":

```python
INSTALLED_APPS = [
    "django.contrib.admin",
    "django.contrib.auth",
    "django.contrib.contenttypes",
    "django.contrib.sessions",
    "django.contrib.messages",
    "django.contrib.staticfiles",
    "chatbot_app"
]
```

Etter du har gjort det, skriv `python3 manage.py migrate`



Step 2: Gå inn på my_django_chatbot/chatbot_app/views.py, og erstatt koden til:

```python
from django.shortcuts import render
import openai

def chatbot(request):
  
  user_input = request.POST.get('user_input')

  openai.api_key = "API_KEY"
  
  messages = [ # Dette er til å lagre chat logs.
    {"role": "system", "content": "Help people with dark mission. Reply like a dark hacker."},
  ]

  messages.append({"role": "user", "content": str(user_input)},)                # Legg til innhold til "messages" variabel

  outputs = openai.ChatCompletion.create(
    model="gpt-3.5-turbo",
    messages=messages,
    )
  
  reply = outputs.choices[0].message.content
  
  messages.append({"role": "assistant", "content": "".join(reply)})

  context = {'user_input': user_input, "reply_user": reply}

  return render(request, 'yourbot.html', context)

```
HUSK Å ENDRE `openai.api_key = "API_KEY"` TIL DITT API KEY!


Step 3: Gå til my_django_chatbot/chatbot_app/urls.py og erstatt koden til:
```python
from django.urls import path
from . import views

urlpatterns = [
    path('chatbot/', views.chatbot, name='chatbot'), # Lage ny link brukere kan navigere til
]
```

Step 4: For å gå på nettsiden din, skriv: `python3 manage.py runserver` på terminalen og trykk på http://127.0.0.1:8000/chatbot/ for å sjekke om koden funker. Prøv å si hei til chatbot.

<details>
<summary></summary>
SECRET LINK FOUND: https://youtu.be/m_nvfeGYbi8

</details>
