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

> ```npx tsc init```

5. Ändra module till esnext i tsconfig.json så vi kan använda import / export

## Sätta upp Express.js

1. Installera express med typings

> ```npm i express @types/express```

2. Skapa server.ts och lägg in grundkoden för en express-server:

```ts
import express, { Request, Response } from 'express';

const app = express()
const port = 3000

app.get('/', (req: Request, res: Response) => {
  res.send('Hello World!')
})

app.listen(port, () => {
  console.log(`Example app listening on port ${port}`)
})
```

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

2. Skapa en anslutning till 


## Fler endpoints

Nu har vi gått igenom hur man sätter upp alla komponenter och skapar en POST-endpoint. Nu återstår att skapa endpoints för GET, PUT och POST /animals. Ta hjälp av föreläsningsanteckningarna och Mongoose-dokumentationen för queries (https://mongoosejs.com/docs/queries.html).

## Refaktorering - förslag

När du gjort endpoints för alla CRUD-operationer och koden fungerar är det dags för förbättringar och refaktorisering. Här kommer förslag på saker att göra:

- Lägg till validering av input-data. Använd t.ex. express-validator (https://express-validator.github.io/docs/).
- 




