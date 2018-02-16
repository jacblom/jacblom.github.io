# Koppelvlak Inname

## Inleiding
In dit hoofdstuk wordt het koppelvlak in technische termen beschreven: de API (Application Programming Interface). Hierbij staan de twee functionaliteiten van de GMW innamewebservice centraal: 
* functionaliteit voor het registreren van gegevens;
* functionaliteit voor het corrigeren van gegevens.

Elke functionaliteit heeft verschillende operaties. De volgende paragrafen beschrijven:
* het patroon van verwerking van de operatie;
* mapping van verzoeken op requests en responses in de API;
* de inhoud van de requests/responses.

## Registratie en zijn operaties
De innamewebservice voor grondwatermonitoringputten biedt de dataleverancier basale functionaliteit. De API van de BRO GMW innamewebservice definieert hiervoor vijf operaties. Elke operatie heeft een eigen request en response die de verschillende verzoeken en antwoorden realiseren. deze worden in de volgende paragrafen beschreven.
### Register
De aard van een grondwatermonitoringput brengt met zich mee dat er drie soorten gebeurtenissen kunnen worden onderscheiden in de bestaansgeschiedenis van een put:
* Registratie starten 
Nadat de constructie van een put is voltooid, kan de registratie van de put worden gestart. De initiële inrichting van de put wordt geregistreerd in de BRO.
* Registratie aanvullen 
Nadat een put is geconstrueerd, breekt de gebruiksperiode aan. Als in de werkelijkheid  een verandering aan de put optreedt, nadat de put in de BRO is geregistreerd, dan moeten de putgegevens in de BRO worden aangevuld met de nieuwe informatie. Daarbij wordt over de putgegevens *materiële geschiedenis* (zie de catalogus) opgebouwd. Afhankelijk van de gebeurtenis wordt in het request van de register-operatie een bepaald brondocument opgenomen, met daarin de nieuwe gegevens.
* Registratie beëindigen 
Na de gebruiksperiode van de put kan deze worden opgeruimd. Dit wordt in de BRO geregistreerd en de put krijgt in de BRO de status voltooid. De geregistreerde putgegevens kunnen daarna niet meer worden aangevuld. 

De putgegevens kunnen naar aanleiding van ieder van deze drie gebeurtenissen worden aangeboden met een register operatie. Het brondocument in het request geeft aan welke gebeurtenis er is opgetreden.

### Replace
Met deze operatie wordt een eerdere registratie of aanvulling gecorrigeerd door de desbetreffende gegevens te vervangen door de gegevens in het request.

De correctie van het type 'Replace' is een correctie die **niet** ingrijpt in de materiële geschiedenis van een put. Alle gebeurtenissen zijn netjes in volgorde en met de juiste datum geregistreerd. Maar bij een van de transacties zijn een of meer onjuiste waarden aangeleverd, of zijn niet alle nieuwe gegevens aangeleverd.

In het eerste geval dient de dataleverancier een verzoek in om die onjuiste waarde(n) te vervangen, en dat is ook letterlijk wat er in de registratie ondergrond gebeurt: iedere waarde wordt vervangen. In het tweede geval dient de dataleverancier een verzoek in waarin uitsluitend de ontbrekende gegevens zijn opgenomen. In beide gevallen wordt er geen materiële geschiedenis maar uitsluitend formele geschiedenis opgebouwd.

### Insert
Met deze operatie wordt een ontbrekende aanvulling ingevoegd in de tijdlijn van de putgeschiedenis.

De correctie van het type invoegen is een correctie die **wel** ingrijpt in de materiële geschiedenis van een put. Het gaat om de eenvoudigste en meest voorkomende variant. Namelijk het geval dat de dataleverancier de registratie had moeten aanvullen, maar dat vergeten was terwijl hij intussen wel een andere verandering heeft laten registreren.

Om de fout te herstellen, dient de dataleverancier een verzoek in met het doel de ontbrekende gegevens in te voegen in de tijdlijn met geregistreerde gegevens. De vergeten gebeurtenis wordt opgenomen in de putgeschiedenis, de betreffende gegevens krijgen een nieuwe waarde met een begin geldigheid (zie innamehandboek) en iedere reeds geregistreerde waarde die vanwege de gebeurtenis verandert krijgt een datum einde geldigheid.

### Move
Met deze operatie wordt een eerdere registratie of aanvulling verplaatst in de tijdlijn van de putgeschiedenis.

De correctie van het type verplaatsen is een correctie die **wel** ingrijpt in de materiële geschiedenis van een put. Het gaat om een complexe variant, waarbij een gebeurtenis is geregistreerd met een foute waarde voor de datum waarop de gebeurtenis heeft plaatsgevonden.

Om de fout te herstellen, levert de dataleverancier de gegevens uit de eerdere registratie of aanvulling opnieuw aan in een move operatie, nu met het correcte moment waarop de gebeurtenis heeft plaats gevonden. De gebeurtenis wordt opgezocht in de putgeschiedenis en verplaatst naar de juiste plek in de tijdlijn van de putgeschiedenis. Van de betreffende gegevens (rondom de oude plek in de tijdlijn en de gecorrigeerde plek in de tijdlijn) worden de waarden voor begin geldigheid (zie innamehandboek) en einde geldigheid aangepast, afhankelijk van het feit of een gegeven voor of na de verwerking van de move operatie vanwege de verplaatste gebeurtenis van waarde is veranderd of juist niet meer.

### Delete
Met deze operatie wordt een eerdere aanvulling verwijderd uit de tijdlijn van de putgeschiedenis.

De correctie van het type verwijderen is een correctie die **wel** ingrijpt in de materiële geschiedenis van een put. Het gaat om een eenvoudige variant, waarbij een aanvulling ten onrechte is aangeboden.

Om de fout te herstellen, dient de dataleverancier een verzoek in met het doel de betreffende gegevens te verwijderen uit de tijdlijn. De gebeurtenis wordt opgezocht in de putgeschiedenis en verwijderd uit de tijdlijn van de putgeschiedenis. Van de betreffende gegevens (rondom de verwijderde plek in de tijdlijn) worden de waarden voor begin geldigheid (zie innamehandboek) en einde geldigheid aangepast, afhankelijk van het feit of een gegeven voor of na de verwerking van de delete operatie vanwege de verwijderde gebeurtenis van waarde is veranderd of juist niet meer. 

## Correctie
## Berichten bij registratie starten
### Bericht registratieverzoek
### Bericht van afwijzing
### Bericht van registratie
## Berichten bij registratie aanvullen en beëindigen
### Bericht registratieverzoek
### Bericht van afwijzing
### Bericht van registratie
## Berichten bij correctie
### Bericht correctieverzoek
### Bericht van afwijzing
### Bericht van acceptatie
### Berichten bij technische fouten
## Softwarefout
### Systeemfout
### Toegangscontrole
## Interfacemodel
## Packagestructuur
## Modelleerregels
