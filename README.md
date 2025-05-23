from datetime import datetime
from distutils.cmd import Command
from email.mime import audio
from unittest import result
from googlesearch import search
import webbrowser
import pyttsx3
import speech_recognition as sr
import datetime
import wikipedia
import os
import smtplib
import sys
    
engine = pyttsx3.init('sapi5')
voices = engine.getProperty('voices')
#print(voices[1].id)
engine.setProperty('voice',voices[0].id)


def speak(audio):
    engine.say(audio)
    engine.runAndWait()

def wishMe():
    hour = int(datetime.datetime.now().hour)
    if hour>=0 and hour<12:
        speak("Good Morning!") 

    elif hour>=12 and hour<18:
        speak("Good Afternoon!") 

    else:
        speak("Good Evening!") 

    speak(" What can i do for you sir?")    


def takeCommand():
    r = sr.Recognizer()
    with sr.Microphone() as source:
            print("Listening...")
            r.pause_threshold = 4
            audio = r.listen(source)        
    try:
        print("Recongnizing...")
        query = r.recognize_google(audio, language='en-US')
        print(f"User said: {query}\n")
    except Exception as e:
            speak("Sorry, I didn't catch that. Could you please repeat?")
            return "None"
    return query
            
if __name__== "__main__":
    wishMe()
    while True:
    #if 1:
        query = takeCommand().lower()
        
    #Logic for executing task based on query
        if 'wikipedia' in query:
           speak('Searching Wikipedia...')
           query = query.replace("wikipedia", "")
           result = wikipedia.summary(query, sentences=5)
           speak("According to wikipedia")
           speak(result)
                
        elif 'open youtube' in query:
            webbrowser.open("youtube.com")

        elif 'open google' in query:
            webbrowser.open("google.com") 
        
        elif 'open map' in query:
            webbrowser.open("https://www.google.com/maps")
            
        elif 'open news' in query:
            webbrowser.open("https://news.google.com/")
            
        elif 'open poki' in query:
            webbrowser.open("poki.com")    

        elif 'open stackoverflow' in query:
            webbrowser.open("stackoverflow.com")
            
        elif 'search' in query:
            speak("What do you want to search for?")
            search_query = takeCommand().lower()
            if search_query != "None":
                url = f"https://www.google.com/search?q={search_query}"
                webbrowser.open(url)
                speak(f"Here are the search results for {search_query}")
            else:
                continue
            
        elif 'play music' in query:
            music_dir = 'C:\\Users\\aryan\\Music\\Fav music'
            Music = os.listdir(music_dir)
            print(Music)
            os.startfile(os.path.join(music_dir, Music [0]))

        elif 'time' in query:
            strTime = datetime.datetime.now().strftime("%H:%M:%S")
            speak(f"Sir, the time is {strTime}")

        elif 'open code' in query:
            codePath = "C:\\Users\\aryan\\AppData\\Local\\Programs\\Microsoft VS Code\\Code.exe"
            os.startfile(codePath)

#        elif 'email to aryan' in query:
#            try:
#                speak("What should I say?")
#                content = takeCommand()
#                to = "aryan55ch@gmail.com"
#                sendEmail(to, content)
#                speak("Email has been sent")
#           except Exception as e:
#                print(e)
#                speak("Sir, the Email was unable to send")    
               
        elif 'thank you' in query:
            speak(f"Happy to Help you Sir!")
            
        elif 'introduce yourself' in query:
            speak(f"Hi I am Jarvis, your personal Assistant")
            
        elif 'jarvis quit' in query:
            speak("Shutting down SIR, Goodbye!")
            sys.exit(0)
            
