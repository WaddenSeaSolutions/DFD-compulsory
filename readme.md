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