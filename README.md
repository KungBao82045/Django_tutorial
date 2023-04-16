## Lets get started!

Step 1: Lag en ny mappe som heter “templates” I my_django_chatbot/chatbot_app. Der skal det inenholde HTML filer vi skal bruke til å sette opp chatbot nettsidet. 

Step 2: Gå inn på my_django_chatbot/chatbot_app/views.py, og erstatt koden til:

```python
from django.shortcuts import render
import openai

def testing(request):
  
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

Step 3: Gå til my_django_chatbot/chatbot_app/urls.py og erstatt koden til:
```python
from django.urls import path
from . import views

urlpatterns = [
    path('chatbot/', views.chatbot, name='chatbot'),
]
```

- ![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) Hello!`#f03c15`
