import tweepy
import requests
import json
import time
import random

# تسجيل الدخول إلى Twitter API
consumer_key = 'YOUR_CONSUMER_KEY'
consumer_secret = 'YOUR_CONSUMER_SECRET'
access_token = 'YOUR_ACCESS_TOKEN'
access_token_secret = 'YOUR_ACCESS_TOKEN_SECRET'

auth = tweepy.OAuthHandler(consumer_key, consumer_secret)
auth.set_access_token(access_token, access_token_secret)
api = tweepy.API(auth)

# استدعاء API الخاص بـ Quran
response = requests.get("http://api.alquran.cloud/v1/quran/quran-uthmani")

# تحويل JSON إلى Dictionary
data = json.loads(response.text)

# استخدام العشوائية لاختيار آية عشوائية
surah = random.choice(data['data'])
ayah = random.choice(surah['ayahs'])

# صيغة النص الذي سيتم نشره على Twitter
text = "Surah " + str(surah['number']) + ", Ayah " + str(ayah['number']) + ": " + ayah['text']

# نشر التغريدة على Twitter
api.update_status(text)

# توقيت نشر التغريدة كل 10 دقائق
while True:
    time.sleep(600)
    surah = random.choice(data['data'])
    ayah = random.choice(surah['ayahs'])
    text = "Surah " + str(surah['number']) + ", Ayah " + str(ayah['number']) + ": " + ayah['text']
    api.update_status(text)
