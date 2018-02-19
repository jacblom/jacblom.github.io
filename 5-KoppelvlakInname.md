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

## Verwerking
De verwerking van een registratieverzoek of een correctieverzoek verloopt volgens een vast patroon, maar er is wel een verschil tussen beide type verzoeken.

### Registratie
De verwerking van een registratieverzoek (de register operatie) verloopt volgens het hierna weergegeven patroon. Een registratie operatie van de GMW innamewebservice begint bij het doen van een registratieverzoek door middel van een request en eindigt met een response.

![Verwerken registratieverzoek] (PrcWebserviceRegistratie.png "Verwerking van een registratieverzoek")

Stap 1: Doen van een registratieverzoek
Het initiatief om een operatie te beginnen ligt bij het systeem van de dataleverancier. Dat roept de register operatie van de *GMW innamewebservice* aan met het *registrationRequest* als parameter.

Stap 2: Uitvoeren toegangscontrole 
Dit bestaat uit identificatie, authenticatie, versleuteling en autorisatie.

Voor de beveiliging van de gegevensuitwisseling worden, conform de Digikoppeling specificaties, PKIoverheid services server certificaten gebruikt. Zowel de dataleverancier als de BRO beschikt over een dergelijk certificaat. In het certificaat is een **identificatie** op basis van 20 cijfers opgenomen die uniek is voor de houder van het certificaat. 

Op het moment dat het systeem van een dataleverancier een operatie aanroept van de webservice van het BRO-systeem wisselen beide systemen eerst hun PKIoverheid services server certificaten uit. Aan de hand van de identificatie in de certificaten weten beide partijen met wie gegevens¬uitwisseling plaatsvindt. De techniek van het PKIoverheid services server certificaat garandeert dat de identificatie in het certificaat ook daadwerkelijk van die partij is (**authenticatie**).

Als authenticatie succesvol is verlopen, worden beide certificaten vervolgens gebruikt om al het dataverkeer tussen de systemen te **versleutelen**. Deze versleuteling maakt het voor derden onmogelijk om de data te lezen of te wijzigen.

Voor het aanbieden van gegevens aan het BRO-systeem zijn rechten nodig. Aan de hand van de identificatie in het certificaat wordt bepaald of het systeem van de dataleverancier **geautoriseerd** is de operatie uit te voeren. Als hierbij een fout optreedt, ontvangt de dataleverancier een melding met een http-statuscode.

Als niet wordt voldaan aan de toegangscontrole, dan leidt dit tot:
•	Een http ‘401 Unauthorized’ foutmelding.
•	Of een ‘ssl error invalid certificate’ foutmelding.
•	Of een andere http-foutmelding met een http-statuscode anders dan ‘200 OK’.

Stap 3: Controleren verzoek
Als de toegangscontrole succesvol is verlopen, dan wordt het *request* technisch en inhoudelijk gecontroleerd. 

De technische controle vindt plaats door het *request* te valideren op basis van de XSD. Als hierbij fouten gevonden worden, dan worden deze beschouwd als een technische fout van het systeem van de dataleverancier en teruggegeven als een *parseFault*.

De inhoudelijke controle vindt plaats door het *request* te controleren volgens de regels die zijn gedefinieerd in de catalogus of het innamehandboek (*business rules*). Deze regels zijn niet in de XSD vastgelegd, maar worden gecontroleerd door de programmatuur van het BRO-systeem. Voorbeelden van controles zijn: 
•	Is een waarde niet groter dan de toegestane maximale waarde?
•	Voldoet een waarde aan de toegestane waardes voor een gegeven?
Als hierbij fouten worden gevonden, dan worden deze beschouwd als een gebruiksfout en teruggegeven in een *response* bericht.

Stap 4: Vastleggen van gegevens
Als alle controles succesvol zijn verlopen dan legt het BRO-systeem de aangeboden gegevens vast en wordt het resultaat teruggegeven in een response bericht.

### Correctie
De verwerking van een correctieverzoek (de operaties *replace*, *insert*, *move* en *delete*) verloopt volgens het hieronder weergegeven patroon. In de verwerking zijn vijf stappen te onderkennen; deze zijn in onderstaande afbeelding weergegeven en worden vervolgens toegelicht.

![Verwerking correctieverzoek] (PrcWebserviceCorrectie.png "Verwerking van een correctieverzoek")

De eerste drie stappen in de verwerking zijn hetzelfde als bij een registratieverzoek, maar nadat het BRO-systeem heeft gecontroleerd of alles goed is, neemt de registratiebeheerder de controle over.

Stap 1: Doen van een correctieverzoek
Het initiatief om een operatie te beginnen ligt bij het systeem van de dataleverancier. Dat roept de desbetreffende operatie van de GMW innamewebservice aan met het *correctionRequest* als parameter. In het *correctionRequest* geeft hij aan waarom hij het correctieverzoek indient.

Stap 2: Uitvoeren toegangscontrole 
Zie stap 2 bij de beschrijving van de verwerking van een registratieverzoek.

Stap 3: Controleren verzoek
Deze stap lijkt veel op stap 3 bij de beschrijving van de verwerking van een registratieverzoek. Aanvullende controles, die de programmatuur van het BRO-systeem uitvoert bij een correctieverzoek, zijn: 
* Bestaat het te corrigeren registratieobject in de BRO?
* Voldoet het correctieverzoek aan procesmatige eisen? Bijvoorbeeld: mag de dataleverancier dit specifieke object corrigeren?

Als bij deze stap fouten worden gevonden, dan worden deze beschouwd als een gebruiksfout en teruggegeven in een *response* bericht met daarin reden en tijdstip van afwijzing.

Als hierbij geen fouten worden gevonden, dan wordt het correctieverzoek geaccepteerd en vastgelegd, maar nog niet verwerkt in de basisregistratie. Voordat het correctieverzoek wordt verwerkt in de basisregistratie wordt het verzoek eerst nog beoordeeld door de registratie-beheerder zoals beschreven in stap 4. Het BRO-systeem reageert met een *response* bericht met daarin het tijdstip van acceptatie.

Stap 4: Beoordelen correctieverzoek
Nadat het correctieverzoek is vastgelegd neemt de registratiebeheerder de verwerking over en voert een inhoudelijke controle uit of het correctieverzoek verwerkt mag worden in de basisregistratie.

Als de registratiebeheerder een correctieverzoek afwijst, dan wordt een *email* naar de dataleverancier verzonden met daarin reden en tijdstip van bezwaar. Een beschrijving van de inhoud van deze email valt buiten de scope van deze koppelvlakbeschrijving; zie het innamehandboek voor nadere informatie. Een dataleverancier kan het emailadres kenbaar maken tijdens het aanmelden bij de BRO.

Stap 5: Vastleggen van gegevens
Als alle controles succesvol zijn verlopen dan verwerkt het BRO-systeem de correctie in de basisregistratie en wordt een email naar de dataleverancier verzonden met daarin het tijdstip van correctie. Een dataleverancier kan het emailadres kenbaar maken tijdens het aanmelden bij de BRO.


## Berichten bij registratie starten
Bij een *register* operatie zijn drie berichten van toepassing: een registratieverzoek, een bericht van afwijzing en een bericht van registratie. Deze paragraaf beschrijft de inhoud van deze berichten bij een ‘registratie starten’ gebeurtenis.

### Request: registratieverzoek
Onderstaande figuur geeft de mapping weer van het registratieverzoek in het innamehandboek op het datatype *RegistrationRequest* in dit document (zie paragraaf 6.1), zoals gebruikt door de operatie *register* (zie hoofdstuk 5).

![RequestRegistratieStarten] (BrtReqRegistratieStarten.png)

Het element brondocumenttype in het registratieverzoek komt niet voor in het *RegistrationRequest*, omdat dit gegeven impliciet bekend is, gegeven de inhoud van het element *sourceDocument*.

Het element *sourceDocument* bevat alle gegevens die in de catalogus voor het registratieobject Grondwatermonitoringput zijn gespecificeerd, met uitzondering van de gegevens die door het BRO-systeem worden gegenereerd of afgeleid uit het RegistrationRequest.


### Response: bericht van afwijzing
Het innamehandboek benoemt als mogelijke reactie op een registratieverzoek een bericht van afwijzing. De webservice gebruikt hiervoor een *response* van het datatype *IntakeResponse*.

![ResponseregistratieAfwijzen] (BrtRespRegistratieStartAfwijzing.png)

Het handboek definieert een aantal berichten als antwoord op een innameverzoek. In de SOAP webservice definities mag elk *request* slechts één *response* hebben. Daarom is het element *responseType* toegevoegd, om de betekenis van de response te duiden. In dit geval heeft het element responseType de vaste waarde rejection.

De waarde van de elementen requestReference en objectIdAccountableParty wordt overgenomen uit het request c.q. het sourceDocument in het request. De waarde van de overige elementen wordt toegekend door de webservice. Het element rejectionReason bevat een waarde uit de tabel met gebruiksfouten; zie het innamehandboek.

Als deze response wordt gegeven omdat er een of meer gebruiksfouten in het sourceDocument zijn geconstateerd, dan is de waarde van rejectionReason “er zijn 1 of meer fouten geconstateerd in het brondocument” en volgen er na dit element een of meer sourceDocumentErrors. 


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
