
Aris is a Virtual assistant that I made in python(In development)

## Source Code

```python
# This was a git project made by @Aadyant2
#It is a voice assistant named A.R.I.S(Another.Rather.Intelligent.System)
#It is currently under development

#import necessary libraries
from asyncore import read
from cgitb import text
from distutils import command
from email.mime import application
from platform import architecture
import random
from time import strftime
import pyttsx3
import datetime
import speech_recognition as sr
import wikipedia
import webbrowser
import smtplib
import pywhatkit
import pyowm

#initialising sapi5, engine and stuff
engine = pyttsx3.init('sapi5')
voices = engine.getProperty('voices')
engine.setProperty('voice', voices[0].id)

#makes the machine able to speak
def speak(audio):
    engine.say(audio)
    engine.runAndWait()

#fetches weather of a certain city
def weatherFunc():
    speak("Which city should I get of?")
    city = takeCommand()
    APIKEY='Your api key'
    OpenWMap=pyowm.OWM(APIKEY)
    Weather=OpenWMap.weather_at_place(city)
    Data=Weather.get_weather()
    temp = Data.get_temprature(unit='celcius')
    speak("Currently Temprature in", city, "is ", temp['temp'])

#wishes the user according to the time at the start of program
def wishMe():
    hour = int(datetime.datetime.now().hour)
    if hour>= 0 and hour<12:
        speak("Good Morning Sir")

    elif hour>=12 and hour<18:
        speak("Good Afternoon Sir")

    else:
        speak("Good Evening Sir")
    speak("I am aris! How may I help you")

#It takes mic input and returns string output
def takeCommand():
    r = sr.Recognizer()
    with sr.Microphone() as source:
        print("Listening...")
        r.pause_threshold = 1
        r.energy_threshold = 200
        audio = r.listen(source)

    try:
        print("Recognizing...")
        query = r.recognize_google(audio, language='en-in')
        print(f"User said: {query}\n")

    except Exception as e:
        print("Say that again please...")
        return "None"
    return query

#It takes an email and password to login and send email to another email with user-defined content
def sendEmail(to, content):
    with open('path of txt file containing pass',"r") as file:
        definitelyNotMyPass = file.read()
    server = smtplib.SMTP("smtp.gmail.com", 587)
    server.ehlo()
    server.starttls()
    server.login('your email here', definitelyNotMyPass)
    server.sendmail('your email here', to, content)
    server.close()

#Plays song(or a video) on youtube
def playSong(command):
        speak("Enter Your Song: ")
        command = takeCommand()
        speak("Enjoy your song Sir!")
        pywhatkit.playonyt(command)
        
# Before executing code, Python interpreter reads source file and define few special variables/global variables. 
# If the python interpreter is running that module (the source file) as the main program, it sets the special __name__ variable to have a value “__main__”.
# If this file is being imported from another module, __name__ will be set to the module’s name.
# Module’s name is available as value to __name__ global variable. 
if __name__ == "__main__":
    wishMe()
    while True:#while loop for listening to user
        #query = user input
        query = takeCommand().lower()
        #Searches wikipedia
        if 'wikipedia' in query:
            speak('Searching Wikipedia...')
            query = query.replace("wikipedia", "")
            results = wikipedia.summary(query, sentences=2)
            speak("According to Wikipedia")
            print(results)
            speak(results)
            speak("Done! What can I do now?")

        #Opens youtube
        elif 'open youtube' in query:
            speak("Opening youtube")
            webbrowser.open("youtube.com")
            speak("Opened! What can I do now?")

        #Opens google
        elif 'open google' in query:
            speak("Opening google")
            webbrowser.open("google.com")
            speak("Opened! What can I do now?")

        #Opens Stackoverflow
        elif 'open stack overflow' in query:
            speak("Opening stack overflow")
            webbrowser.open("stackoverflow.com")
            speak("Opened! What can I do now?")

        #Tells the time
        elif 'time' in query:
            strTime = datetime.datetime.now().strftime("%H:%M")
            speak(f"Sir, the time is {strTime}")
            speak("what can i do now?")
        
        #Terminates the program
        elif 'exit' in query:
            if datetime.datetime.now().hour < 17:
                speak("Ok Sir! I hope you have a good day")
            else:
                speak("ok Sir! Good Night")
            exit()
            
        elif 'email to my brother' in query:#Sends email (to my brother in this case)
            try:
                with open('path of the txt file containing the recipient email adress', "r") as file1:
                    definitelyNotMyEmail = file1.read()
                speak("what should i say")
                content = takeCommand()
                to = definitelyNotMyEmail
                sendEmail(to, content)
                speak("Email has been sent")
            except Exception as e:
                print(e)
                speak("Sorry Sir! I am not able to send this email")
                speak("what can i do now?")
        
        #Plays song
        elif 'play a song' in query:
            try:
                playSong(command)
            except Exception as e:
                print(e)
                speak("Sorry Sir! I am not able to play this song")
                speak("what can i do now?")
        
        #General speech
        elif 'how are you' in query:
            speak("I am Fine sir! what about you")
            stateOfUser = takeCommand().lower()
            if 'i am also fine' in stateOfUser:
                speak("Thats very good! I hope you keep smiling")
                speak("what can i do now?")
            elif 'i am fine' in stateOfUser:
                speak("Thats very good! I hope you keep smiling")
                speak("what can i do now?")
            else:
                speak("I pray the best for you! What should I do to uplift your mood!")

        #General speech
        elif 'hello' in query:
            speak("Hey!")
            speak("what can i do for you?")

        #General speech
        elif 'nothing' in query:
            speak("There must be something that I can do?")
            speak("what can i do now?")

        #General speech
        elif 'mad' in query:
            speak("Sometimes my wires get crossed but I dont think i know how to be angry. Thanks for asking though")
            speak("what can i do now?")

        #General speech
        elif 'relationship' in query:
            speak("We can talk about this another time")
            speak("what can i do now?")

        #General speech
        elif 'what can you do' in query:
            speak("Well, I can do a lot of things. For example, opening websites, playing songs, sending emails or maybe even, uplifting your mood")

        #General speech
        elif 'you are good' in query:
            speak("Thank you sir!")
        
        #General speech
        elif 'tell a joke' in query:
            jokes = ['Where do frogs keep there money? ,The river bank','What do you call a cavemans fart? A blast from the past','How do you make a squid laugh? With Ten-tickles','What happened when the goose fell down the stairs? He got goose bumps','What does thor call his underpants? Thunderwear']
            speak(random.choice(jokes))

        #Tells the weather
        elif 'weather' in query:
            weatherFunc()
        
        #When the user says something incomprehensible
        else:
            speak("I am not quite sure I understand")
            speak("what can i do now?")
```

## Contributing
Donations are welcome

## License
GNU Affero General Public License v3.0
