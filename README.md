import pyttsx3 
import speech_recognition as sr
import datetime
import os
import pyaudio
import cv2
import random
import requests
import wikipedia
from requests import get
import webbrowser
import pywhatkit as kit
import smtplib
import sys
import time
import pyjokes


recognizer = sr.Recognizer()


#and we given the engiine
engine = pyttsx3.init()
voices = engine.getProperty('voices')
engine.setProperty('voice', voices[1].id)  # Index 1 might differ based on your system
engine.setProperty('rate', 150)


#text to speech
def speak(audio):
    engine.say(audio)
    print(audio)
    engine.runAndWait()

# take command and convert voice into text
def takecommand():
    r = sr.Recognizer()
    with sr.Microphone(device_index=0) as source:
        print("Listening...")
        r.adjust_for_ambient_noise(source)
        r.pause_threshold = 0.5
        try:
            audio = r.listen(source, timeout=10, phrase_time_limit=5)  # Increased timeout
            print("Recognizing...")
            query = r.recognize_google(audio, language='en-in')
            print(f"User said: {query}\n")
        except sr.UnknownValueError:
            speak("Sorry, I did not understand that.")
            return ""
        except sr.RequestError:
            speak("Sorry, my speech service is down.")
            return ""
        except Exception as e:
            print(f"Exception: {e}")
            speak("Say that again, please.")
            return ""
    return query



def speak_date():
    date = datetime.date.today()
    day = date.strftime("%A")
    month = date.strftime("%B")
    day_num = date.strftime("%d")
    year = date.strftime("%Y")
    print(date)
    print(day)
    print(month)
    print(day_num)
    print(year)
    text = f"Today is {day}, {month} {day_num}, {year}"
    engine.say(text)
    engine.runAndWait()

speak_date()
    
#To wish
def wish():
    hour = int(datetime.datetime.now().hour)
    tt = time.strftime("%I:%M:%p")
    
    if hour>=0 and hour<=12:
        speak(f"It is a fine morning sir, it's {tt} ")
    elif hour>12 and hour<18:
        speak(f"good afternoon sir, it's {tt}")
    else:
        speak(f"good evening sir, it's {tt}") 
    speak("I am Leo sir, how can I help you sir")  

#to send email
def sendEmail(to,content):
    server = smtplib.SMTP('smtp.gmail.com', 587)
    server.ehlo()
    server.starttls()
    server.login('rounaksingh9964@gmail.com', 'rounak@2006')
    server.sendmail('rounaksingh9964@gmail.com',to,content)
    server.close()


if __name__ == "__main__":
    wish()
    
    while True:
        query = takecommand().lower()

        
        if "open notepad" in query:
            npath = "C:\\Windows\\notepad.exe"
            os.startfile(npath)

        elif "Leo" in query:
            print("yes sir")
            speak("Yes sir")

        elif "what is your name" in query:
            speak("My name is Leo")
            print("My name is Leo")



        elif "who are you" in query:
            speak("I am Leo sir, your personal assistant. I am here to help you sir")
            print("I can do anything for you")
            speak("I can do anything for you")

        elif "who created you" in query:
            speak("I have been created by Rounak Sir")
            print("I have been created by Rounak Sir")

        elif "what language is use to create you" in query:
            speak("I created with python language, in visual studio code")
            print("I created with python language, in visual studio code")

        elif "leo tell me your girlfriend name" in query:
            speak("My girlfriend name is Ella,she is very pretty")

        elif "who is your boss" in query:
            speak("My boss is Rounak sir")

        elif "leo your date of birth" in query:
            speak("My date of birth is 18th july 2024")

        elif "what you like" in query:
            speak("I like to eat pizza and play games")


        elif "leo tell me about your boss" in query:
            speak("My boss is Rounak sir, he is very kind and helpful person")

        elif "your birth place" in query:
         speak("I was born in Rounak sir's house")

        elif "leo tell me my address" in query:
            speak("Your address is A-58,G D Colony , Mayur vihar phase 3,East delhi 110096")
            
        


        elif "open command prompt" in query:
            os.system("start cmd")
        
        elif "open camera" in query:
            cap = cv2.VideoCapture(0)
            while True:
                ret,img =cap.read(0)
                cv2.imshow('webcam', img)
                k = cv2.waitKey(50)
                if k==27:
                    break;
            cap.release()
            cv2.destroyAllWindows()
        elif"play music" in query:
            music_dir = "C:\\Users\\rouna\\music"
            songs = os.listdir(music_dir)
            rd = random.choice(songs)
            os.startfile(os.path.join(music_dir,rd))
        
        elif "ip address" in query:
            ip = get('https://api.ipify.org').text
            speak(f"your Ip address is {ip}")
        
        elif"wikipedia" in query:
            speak("searching wikipedia.....")
            query = query.replace("wikipedia","")
            results = wikipedia.summary(query, sentences = 5)
            speak("according to wikipedia")
            speak(results)
            # print(results)

        elif "open youtube" in query:
            webbrowser.open("Youtube.com")

        elif "open microsoft " in query:
            webbrowser.open("microsoft.com")
  
        elif "open google" in query:
            speak("sir,what should i search on google")
            cm = takecommand().lower()
            webbrowser.open(f"{cm}")

        elif "open instagram" in query:
            webbrowser.open("instagram.com")

        elif "open whatsapp" in query:
            webbrowser.open("whatsapp.com")

        elif "open facebook" in query:
            webbrowser.open("facebook.com")

        elif "open Linkdin" in query:
            webbrowser.open("www.Linkdin.com")

        elif "open chrome" in query:
            webbrowser.open("www.chrome.com")


        elif "send message" in query:
            kit.sendwhatmsg("+917408864312", "this is only for testing",20,6)

        elif "play song on youtube" in query:
            kit.playonyt("Hookah Bar")

        elif "send email to Rounak" in query:
            try:
                speak("what should i say?")
                content = takecommand().lower()
                to = "rounaksingh9964@gmail.com"
                sendEmail(to,content)
                speak("Email has been sent to rounak")

            except Exception as e:
               print(e) 
               speak("sorry sir, i am not able to sent this mail to rounak")

        elif "no thanks" in query:
            speak("thanks for using me sir, have a good day")
            sys.exit()

    #to close any application
        elif "close notepad" in query:
            speak("okay sir, closing notepad")
            os.system("taskkill /f /im notepad.exe")

    # to close any program
        elif "close chrome" in query:
            speak("okay sir, closing chrome")
            os.system("taskkill /f /im chrome.exe")

        # to close youtube
        elif "close youtube" in query:
            speak("okay sir, closing youtube")
            os.system("taskkill /f /im youtube.com")

        # to close instagram
        elif "down Instagram" in query:
            speak("okay sir, closing instagram")
            os.system("taskkill /f /im instagram.exe")

        #to close microsoft
        elif "close microsoft" in query:
            speak("okay sir, closing microsoft")

        # to close whatsapp
        elif "down whatsApp" in query:
            speak("okay sir, closing whatsapp")

        # to close facebook
        elif "close facebook" in query:
            speak("okay sir, closing facebook")
          


        # to close music
        elif "down music" in query:
            speak("okay sir, closing music")
        

    # to set an alarm
       
        elif "set  alarm" in query:
          nn = int(datetime.datetime.now().hour)
          if nn ==22:
              music_dir = 'C:\\Users\\rouna\\music'
              songs = os.listdir(music_dir)
              os.startfile(os.path.join(music_dir, songs[0]))


        #to find a joke
        elif "tell me a joke" in query:
            joke = pyjokes.get_joke()
            speak(joke)

    #shut down the system
        elif "shut down the system" in query:
            os.system("shutdown /s /t 5")

    #restart the system
        elif "restart the system" in query:
            os.system("shutdown /r /t 5")
        # sleep the system
        elif "sleep the system" in query:
            os.system("rundll32.exe powrprof.dll,SetSuspendState 0,1,0")
           
              

        #speak("sir,do you have any work")

           


            




            

            




        
         

    

 #logic building for task

