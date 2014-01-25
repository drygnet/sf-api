# sf-api
Api som används av SF-Bio -appen för iPhone

SF Bio har en smidig app för iPhone där man främst kan se vilka filmer som visas i en viss stad och även boka biljetter.

Jag missade ofta när bra filmer gick på bio så jag tänkte bygga någon slags notifiering när tex filmer som fått bra betyg på IMDB har premiär i min stad.

... men då måste man få tag på datat på nåt sätt.

## Låt hackandet börja

Jag använder fiddler som proxy för att jag har det installerat och använder det till mycket annat,
andra verkar föredra Charles Proxy men den har jag inte provat.

I Fiddler under menyn Tools väljer du "Fiddler options" och sedan fliken "Connections" för att ställa in Fiddler som proxy åt externa enheter (i det här fallet din iPhone)

Ställ in proxyinställningarna i din iphone att peka på din dator Fiddler och välj den port du konfigurerat i fiddler (standard är 8888)

Starta SB-Bio appen och anropen bör dyka upp i fiddler.

Första anropet går till:
http://sfbm-prod-v3.cybercomhosting.com/services/3.0/config
och det skickas med några viktiga saker i headern

```
  GET /services/3.0/config HTTP/1.1
  Host: sfbm-prod-v3.cybercomhosting.com
  Authorization: Basic c2ZiaW86U0ZiZWUw
  Accept: application/json
  Accept-Encoding: gzip
  Connection: keep-alive
  Connection: keep-alive
  X-SF-Iphone-Version: 2.0.7
  User-Agent: SF Bio 2.0.7 rv:241 (iPhone; iPhone OS 7.0.2; sv_SE)
```
Både X-Headern och 


Byt till fliken "Auth" under "Inspectors" så ser du
```
Decoded Username:Password= sfbio:SFbee0
```

Hoppsan, där var användarnamn och lösenord

Som svar får man en kaka som kan användas för framtida anrop
(Det här är inte en guide i hur man nyttjar ett api, det finns massor av bra såna)


## Hur får man ut vilka filmer som visas då?

På urlen http://sfbm-prod-v3.cybercomhosting.com/services/3.0/cities
så får man JSON med alla städer, de har ett namn och ett ID.
```
{"name":"Visby","id":"VY"}
```
Så då kan man få utförlig information på url:en
http://sfbm-prod-v3.cybercomhosting.com/services/3.0/movies/currentmovies/VY
```
{
movies: [12]
0:  {
tags: null
numberOfTheatres: 1
firstShowDate: 1390656147150
shownInMyCity: true
premiereDate: 1389913200000
movieName: "Bamse och tjuvstaden"
highQualityTrailerLink: "http://sv.clip-1.filmtrailer.com/14027_51400_a_5.mp4?log_var=54%7C4601100194-1%7C-"
lowQualityTrailerLink: "http://sv.clip-1.filmtrailer.com/14027_51400_a_4.mp4?log_var=54%7C4601100194-1%7C-"
ratingAge: "Barntillåten"
genre: "Barn"
ratingCode: "4"
placeHolderPosterURL: "http://sfbm-prod-v3.cybercomhosting.com/image/POSTER/_WIDTH_/-/77035780.jpg"
placeHolderBannerURL: "http://sfbm-prod-v3.cybercomhosting.com/image/BANNER/_WIDTH_/-/77035780.jpg"
index: 0
length: 66
id: "77035780"
}
}
```
Trailer-länkarna fungerar utan inloggning och man behöver inte skicka med var_log-parametern
Posterlänkarna kräver inte heller inloggning men man byter ut _ WIDTH_ till den bredd man vill ha

exempel: http://sfbm-prod-v3.cybercomhosting.com/image/POSTER/400/-/77035780.jpg

ID:t man får i listan kan sedan användas för att få detaljer om filmen:
http://sfbm-prod-v3.cybercomhosting.com/services/3.0/movies/moviedetail/77035780?cityid=VY&includeAllTags=true
```
{
shortDescription: "Bamse, världens starkaste och snällaste björn, 
har älskats av generationer av barn ända sedan starten 1966. 
Nu tar Bamse klivet till bioduken med sin första långfilm. 
Filmen bygger på ett originalmanus av Johan Kindblom och Tomas Tivemark 
samt regisseras av Christian Ryltenius. 
I Bamse och Tjuvstaden får vi följa Bamse, Skalman och Lille Skutt 
på ett nytt spännande äventyr genom Trollskogen och in i 
Tjuvarnas stad för att rädda Farmor från den elake Reinard Räv."
longDescription: null
tags: [0]
movieVersions: [1]
0:  {
tags: [0]
movieId: "77035780"
firstShowDate: 1390656824126
}-
-
numberOfTheatres: null
firstShowDate: 1390656824185
shownInMyCity: true
price: 70
premiereDate: 1389913200000
movieName: "Bamse och tjuvstaden"
highQualityTrailerLink: "http://sv.clip-1.filmtrailer.com/14027_51400_a_5.mp4?log_var=54%7C4601100194-1%7C-"
lowQualityTrailerLink: "http://sv.clip-1.filmtrailer.com/14027_51400_a_4.mp4?log_var=54%7C4601100194-1%7C-"
ratingAge: "Barntillåten"
genre: "Barn"
ratingCode: "4"
actors: [0]
directors: [1]
0:  {
name: "Christian Ryltenius"
}-
-
placeHolderPosterURL: "http://sfbm-prod-v3.cybercomhosting.com/image/POSTER/_WIDTH_/-/77035780.jpg"
placeHolderBannerURL: "http://sfbm-prod-v3.cybercomhosting.com/image/BANNER/_WIDTH_/-/77035780.jpg"
index: 0
length: 66
id: "77035780"
}
``` 

