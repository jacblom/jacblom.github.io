# Uitgangspunten

## Authenticatie en autorisatie

## Berichten en meldingen

## Communicatiestandaarden en uitwisselformaat
De communicatie tussen het systeem van de dataleverancier en het BRO-systeem verloopt over een aantal lagen. In de volgende afbeelding is per laag aangegeven welke communicatiestandaard van toepassing is.

![Communicatiestandaarden] (media/CommunicatieStandaarden.png "Communicatie standaarden - lagenmodel")

De keuzes van de communicatiestandaarden die zijn gebruikt bij de inrichting van het BRO-systeem zijn gebaseerd op de NORA (Nederlandse Overheid Referentie Architectuur en de Digikoppeling specificaties. 
### Gegevens- en berichtenstandaarden
Omdat alle registratieobjecten van de BRO een relatie hebben met een locatie op het aardoppervlak, zijn de gegevens en berichten volgens de NEN3610 standaard ([3]) gemodelleerd.
### Logistieke standaard
Als logistieke standaard is voor de BRO het 2W-be profiel van Digikoppeling ([2]) gehanteerd. Het koppelvlak is daarom gerealiseerd als een WUS-webservice waarvoor een aantal onderliggende standaarden zijn voorgeschreven, waaronder WSDL 1.0 en SOAP 1.1. In onderstaande afbeelding is dat schematisch weergegeven.

![LogistiekeStandaard] (LogistiekStandaard.png "Logistieke standaard")

Het WSDL-document (Web Service Definition Language) beschrijft in technische termen de volledige API (Application Programming Interface) van de GMW innamewebservice (zie hoofdstuk 3). Het beschrijft de operaties, inclusief request en response, maar ook het protocol (in dit geval SOAP) waarmee request en response worden uitgewisseld en de URL waarop de webservice benaderd kan worden.

SOAP (Simple Object Access Protocol) is een protocol voor het versturen van berichten. Een SOAP bericht bestaat uit een Envelope met daarin de Header en de Body. De Body bevat het eigenlijke XML-bericht dat uitgewisseld wordt. Ieder XML-bericht dat als onderdeel van een SOAP-bericht met het BRO-systeem uitgewisseld wordt, is beschreven in een aantal BRO XML-schema’s (XSD). Deze structuur van een SOAP-bericht is in de volgende afbeelding samengevat.

![SOAP] (SOAPEnvelop.png "SOAP Envelop")

De XML-schema’s (XSD) volgen de gegevensdefinities van de catalogus nauwkeurig, maar soms leidt de toepassing van de NEN3610 standaard tot afwijkingen. Daar waar wordt afgeweken van de catalogus wordt dat expliciet toegelicht. Omdat de XSD is uitgewerkt in het Engels en de catalogus in het Nederlands is beschreven, is in bijlage A een vertaling van de berichtgegevens opgenomen.
### Netwerkstandaard
Als netwerkstandaard wordt TCP/IP over het internet gehanteerd.
