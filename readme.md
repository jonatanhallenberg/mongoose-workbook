# Moongoose Workbook

Ni ska nu bygga ett REST-api med hjälp av Typescript, ExpressJS, MongoDB och Mongoose.

## Prerequisites

Innan ni starta behöver ni:

- MongoDB-instans t.ex. via Docker
- NodeJS installerat
- VSCode

## Stackens komponenter

Ni använder er av följande komponenter:

- __TypeScript__ är programmeringsspråket ni använder er av.

- __Node.JS__ är en runtime som kör er kod
- __MongoDB__ är databasen där ni lagrar data som postas in via api:et
- __Mongoose__ är ett npm-paket ni använder för att ansluta och skicka frågor till databasen.

## Sätta upp TS-miljön

1. Skapa en katalog och öppna den med VSCode, starta terminalen och skriv:

> ```npm init```

2. Stega igenom guiden för att sätta upp npm
3. Installera *TypeScript*, *ts-node* och *nodemon*:

> ```npm i typescript ts-node```

> ```npm i nodemon --save-dev```

4. Generera *tsconfig.json*

> ```npx tsc --init```

5. Ändra i module till ESNext och target till NodeNext i tsconfig.json så vi kan använda import / export

6. Avkommentera moduleResolution="node" så att importerna fungerar korrekt

## Sätta upp Express.js

1. Installera express med typings

> ```npm i express @types/express```

2. Skapa server.ts och lägg in grundkoden för en express-server:

![Code](/img/code1.png?raw=true "Code")

3. Lägg till start-script i package.json

```json
"start": "nodemon server.ts"
```

4. Testa starta appen och surfa till http://localhost:3000

>```npm start```

## Skapa en router

Vi ska nu lägga till CRUD-operationer för entiteten *animal*. För att kunna separera logiken i appen skapar vi en egen router för animal.

1. Skapa routes/animal.ts

```typescript
import express, { Request, Response } from 'express';

const router = express.Router();

router.get('/', (req: Request, res: Response) => {
    res.send('GET animals');
})

export default router;
```

2. Importera animal-routern i server.ts och registrera i express-appen

```typescript
import animalRouter from './routes/animal';
```

...

```typescript
app.use('/animal', animalRouter)
```

## Sätta upp anslutning till Mongoose

1. Installera mongoose och typings

>```npm i mongoose @types/mongoose```

2. Skapa en anslutning till Mongoose i server.ts. Se till att URI:n matchar din databas. Databasnamnet står sist, i exemplet är det *zoo*:

```typescript
import { connect } from 'mongoose';
connect('mongodb://localhost:27017/zoo')
```

## Skapa interface och schema för animal

1. Lägg till en mapp 'db' som innehåller all databas-logik
2. I db, lägg till en mapp 'models' som innehåller modeller för alla entiteter (collections)
3. Skapa db/models/animal.ts och lägg in interface, schema och model för animal:

```typescript
import { Schema, model } from 'mongoose';

export interface AnimalType {
    type: string,
    name: string,
    isMammal: boolean,
    numberOfLegs: number
}

const schema = new Schema<AnimalType>({
    type: { type: String, required: true },
    name: { type: String, required: true },
    isMammal: { type: Boolean, required: true },
    numberOfLegs: { type: Number, required: true },
})

const AnimalModel = model<AnimalType>('Animal', schema)

export default AnimalModel;
```

## Skapa en addAnimal-funktion

Nu när vi har modellen är vi redo att skapa funktionen som lägger till ett animal i databasen

1. Skapa db/animalCrud.ts och lägg till en createAnimal-fuktion som använder sig av Animal-modellen vi har skapat

```typescript
import AnimalModel, { AnimalType } from './models/animal';

export const createAnimal = async (animal: AnimalType) => {
    const newAnimal = new AnimalModel(animal);
    await newAnimal.save();
    return newAnimal;
}
```

## Skapa en route for POST /animal

1. I routes/animal.ts lägger vi till en ny route för POST som anropar createAnimal från db/animalCrud.ts och returnerar det skapade djuret:

```typescript
router.post('/', async (req: Request, res: Response) => {
    const createdAnimal = await createAnimal(req.body);
    res.status(201).json(createdAnimal);
})
```

## Testa koden
Använd en Rest-klient t.ex. Insomnia för att testa post-endpointen

> ```POST /animal```

```json
{
	"name": "Bo",
	"type": "Ko",
	"isMammal": true,
	"numberOfLegs": 2
}
```

## Fler endpoints

Nu har vi gått igenom hur man sätter upp alla komponenter och skapar en POST-endpoint. Nu återstår att skapa ytterligare endpoints:

>```GET /animal```
>```PUT /animal```
>```DELETE /animal```
 
Ta hjälp av föreläsningsanteckningarna och Mongoose-dokumentationen för queries (https://mongoosejs.com/docs/queries.html).

## Refaktorering - förslag

När ni gjort endpoints för alla CRUD-operationer och koden fungerar är det dags för förbättringar och refaktorisering. Här kommer förslag på saker att göra:

- Bryt ut konfigurerbara värden (t.ex. Connection URI) i en .env-fil och läs in med hjälp av dotenv (https://www.npmjs.com/package/dotenv)
- Lägg till validering av input-data. Använd t.ex. express-validator (https://express-validator.github.io/docs/).
- Se till att rätt statuskoder returneras beroende på metod och utfall.