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




