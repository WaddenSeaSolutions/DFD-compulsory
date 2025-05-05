1. Database Selection:
NoSQL Database: MongoDB

Anvendes til:
- Varelister (listings)
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

2. Data Schema and Storage Strategy
Vi bruger en NoSQL-database til alle datatyper, da det giver fleksibilitet, høj skalerbarhed og god understøttelse af semi-strukturerede data som bruger genererede data.

Dataobjekter:
User:
- Indeholder basale brugeroplysninger såsom navn og email. Brugerens ID bruges som reference i ordrer og anmeldelser.

Listing:
- Indeholder information om den vare, brugeren ønsker at sælge. Felter inkluderer titel, pris, billeder og sælgerens ID.

Order:
- Indeholder reference til den købte vare samt BuyerId og SellsId. Indeholder også total pris og ordrestatus.

Review:
- Indeholder information om en anmeldelse givet til en sælger. Felter inkluderer ReviewerId, SellerId, rating og dato.

3. Integration af Cloud Storage

Billeder og mediefiler gemmes eksternt i cloud storage, f.eks. Amazon S3 eller Azure Blob Storage, i stedet for direkte i databasen.
- Når en bruger uploader et billede, håndteres det af en service (IBlobStorageService), som returnerer en URL.

- Denne URL gemmes som en streng i f.eks. ImageUrls-feltet på Listing.

Fordele:
- Database holdes letvægts og hurtig
- Storage skalerer uafhængigt af databasen