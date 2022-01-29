# Weather.App
from configparser import ConfigParser
import requests
from tkinter import *
from tkinter import messagebox
import time 

config_file = "config.ini"
config = ConfigParser()
config.read(config_file)
api_key = config['api_key']['key']
url = 'http://api.openweathermap.org/data/2.5/weather?q={}&appid={}'


def getweather(city):
    result = requests.get(url.format(city, api_key))
      
    if result:
        json = result.json()
        city = json['name']
        country = json['sys']
        temp_kelvin = json['main']['temp']
        temp_celsius = temp_kelvin-273.15
        weather1 = json['weather'][0]['main']
        sunrise = time.strftime('%I:%M:%S', time.gmtime (json['sys']['sunrise']- 21600))
        sunset = time.strftime('%I:%M:%S', time.gmtime (json['sys']['sunset']- 21600))
        final = [city, country, temp_kelvin,temp_celsius, weather1, sunrise, sunset]
        return final
    else:
        print("NO Content Found")
  

def search():
    city = city_text.get()
    weather = getweather(city)
    if weather:
        temperature_label['text'] = "Temperature   " + '{:.2f}°C'.format(weather[3]) 
        sunrise_lbl['text'] = "Sunset   " + '{}'.format(weather[5])
        sunset_lbl['text'] = "Sunrise   " + '{}'.format(weather[6])
    else:
        messagebox.showerror('Error', "Cannot find {}".format(city))

App=Tk()
App.minsize(height=400,width=200)
App.title("Weather App")

city_text = StringVar()
city_entry = Entry(App, textvariable=city_text, justify='center', width = 40, font=('Times_New_Roman',15))
city_entry.pack()
Search_btn = Button(App, text="Search Weather", width=40, font=('Times_New_Roman'),activebackground='blue',command=search)
Search_btn.pack(side=BOTTOM)
location_lbl = Label(App, text="", font={'bold', 20})
location_lbl.pack()
temperature_label = Label(App, text='')
temperature_label.pack()
sunrise_lbl = Label(App, text='')
sunrise_lbl.pack()
sunset_lbl = Label(App, text='')
sunset_lbl.pack()
weather_l = Label(App, text="")
weather_l.pack()


App.mainloop() 
