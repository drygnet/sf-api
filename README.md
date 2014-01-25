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
{"mainCities":[{"name":"Stockholm","id":"SE"},{"name":"Göteborg","id":"GB"},{"name":"Malmö","id":"MA"}],"cities":[{"name":"Alingsås","id":"AL"},{"name":"Borlänge","id":"BO"},{"name":"Borås","id":"BS"},{"name":"Eskilstuna","id":"EA"},{"name":"Falun","id":"FN"},{"name":"Flen","id":"FL"},{"name":"Gävle","id":"GA"},{"name":"Halmstad","id":"HD"},{"name":"Helsingborg","id":"HE"},{"name":"Hudiksvall","id":"HL"},{"name":"Härnösand","id":"HS"},{"name":"Jönköping","id":"JO"},{"name":"Kalmar","id":"KL"},{"name":"Karlskrona","id":"KK"},{"name":"Karlstad","id":"KA"},{"name":"Katrineholm","id":"KM"},{"name":"Kristianstad","id":"KD"},{"name":"Kungsbacka","id":"KB"},{"name":"Köping","id":"KP"},{"name":"Linköping","id":"LI"},{"name":"Luleå","id":"LU"},{"name":"Lund","id":"LD"},{"name":"Mariestad","id":"MD"},{"name":"Mjölby","id":"MJ"},{"name":"Mora","id":"MR"},{"name":"Motala","id":"ML"},{"name":"Norrköping","id":"NO"},{"name":"Norrtälje","id":"NT"},{"name":"Nyköping","id":"NG"},{"name":"Skara","id":"SA"},{"name":"Skellefteå","id":"ST"},{"name":"Skövde","id":"SK"},{"name":"Strängnäs","id":"SS"},{"name":"Sundsvall","id":"SU"},{"name":"Sälen","id":"SN"},{"name":"Söderhamn","id":"SH"},{"name":"Uddevalla","id":"UD"},{"name":"Umeå","id":"UM"},{"name":"Uppsala","id":"UP"},{"name":"Vetlanda","id":"VL"},{"name":"Visby","id":"VY"},{"name":"Vänersborg","id":"VG"},{"name":"Värnamo","id":"VR"},{"name":"Västervik","id":"VS"},{"name":"Västerås","id":"VA"},{"name":"Växjö","id":"VO"},{"name":"Ystad","id":"YD"},{"name":"Örebro","id":"OR"},{"name":"Örnsköldsvik","id":"OK"},{"name":"Östersund","id":"OS"}]}

```




