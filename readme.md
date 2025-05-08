1. Database Selection:
NoSQL Database: MongoDB

Anvendes til:
- Listings
- Brugerprofiler
- Sælgeranmeldelser

Begrundelse:
- Understøtter dynamiske skemaer – ideelt til varer med forskellige attributter.
- Optimeret til horisontal skalering og store mængder data.
- Indbygget understøttelse af geospatial søgning.
- Velegnet til data som er oprettet af brugere og fleksible datamodeller.

Relationsdatabase: PostgreSQL
- Anvendes til:
- Ordrer
- Betalinger

Begrundelse:
- ACID-kompatibel, hvilket sikrer datakonsistens og pålidelighed.
- Ideel til strukturerede relationer, dette kunne være kobling mellem brugere, varer og transaktioner.

2. Data Schema and Storage Strategy:
Vi har tænkt os at bruge en NoSQL-database og Relation Database, da det giver fleksibilitet, høj skalerbarhed og god understøttelse af semi-strukturerede data, fordi vi deler data op i to forskellige databaser.

Dataobjekter:
User:
- Indeholder basale brugeroplysninger såsom navn og email. Brugerens ID bruges som reference i ordrer og anmeldelser.

Listing:
- Indeholder information om den vare, brugeren ønsker at sælge. Felter inkluderer titel, pris, billeder og sælgerens ID.

Order:
- Indeholder reference til den købte vare samt BuyerId og SellsId. Indeholder også total pris og ordrestatus.

Review:
- Indeholder information om en anmeldelse givet til en sælger. Felter inkluderer ReviewerId, SellerId, rating og dato.

3. Integration af Cloud Storage:
Billeder og mediefiler gemmes eksternt i cloud storage, f.eks. Amazon S3 eller Azure Blob Storage, i stedet for direkte i databasen.
- Når en bruger uploader et billede, vil det blive håndteret af en service (IBlobStorageService), som så skal returnerer en URL.
- Denne URL gemmes som en streng i f.eks. ImageUrls-feltet på Listing.

Fordele:
- Database holdes letvægts og hurtig
- Storage skalerer uafhængigt af databasen

4. Caching Strategy:
Vi vil benytte os af Redis til caching for at forbedre performance og reducere læsebelastning på databasen.
- Populære og fremhævede produkter.
- Højeste rated profiles.
- Ved ændringer i data, så som opdatering af annoncer eller nye anmeldelser fjernes relaterede keys fra cache.
- TTL (Time to Live) sættes på cache key for automatisk forældelse, dette kunne være smart hvis vi havde en liste med "Nye varer".

5. CQRS Implementation:
- Command classes skal håndterer forespørgsler som opretter annonce og placer ordre.
- Query classes skal håndtere forespørgsler som hente annoncer og brugerprofiler.
Vi har valgt at bruge CQRS/Commands/ og CQRS/Queries/ foldere i projektet.
Dette gør vi fordi:
- Øger skalerbarhed read and write operationer kan optimeres individuelt.
- Forbedrer vedligeholdelse og testbarhed
- Gør det lettere at tilføje f.eks. event sourcing eller separate reading databaser senere.

6. Transaction Management:
Selvom MongoDB ikke er relationsbaseret, tilbyder det transaktioner på tværs af dokumenter, når de er i samme database.
Vi sikrer transaktionel integritet i følgende scenarier:
- Når en Order placeres, oprettes ordren og den relaterede Listing markeres som solgt.
- Ved opdatering/sletning af en Listing, sikres konsistens ved at validere at ingen igangværende Order refererer til den.


Alt dette 