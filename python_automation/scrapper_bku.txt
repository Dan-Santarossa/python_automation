from gettext import find
from urllib import response
import requests # http requests

from bs4 import BeautifulSoup # web scraping 

import smtplib #send the mail

from email.mime.multipart import MIMEMultipart #email body
from email.mime.text import MIMEText

import datetime #system and date manipulation
now = datetime.datetime.now()


content = '' #email content placeholder

#extracting hacker news stories

def extract_news(url):
    print('extracting hacker news stories...')
    cnt = ''
    cnt +=('<b>HN Top Stories:</b>\n'+'<br>'+'-'*50+'<br>')
    response = requests.get(url)
    content = response.content
    soup = BeautifulSoup(content, 'html.parser')

    for i,tag in enumerate(soup.find_all('td',attrs={'class':'title','valign':''})):
        cnt += ((str(i+1)+' :: '+ '<a href="' + tag.a.get('href') + '">' + tag.text + '</a>' + "\n" + '<br>') if tag.text!='More' else '')
        #print(tag.prettify) #find_all('span',attrs={'class':'sitestr'}))
    return(cnt)

cnt = extract_news('https://news.ycombinator.com/')
content += cnt
content += ('<br>------<br>')
content +=('<br><br>End of Message')



