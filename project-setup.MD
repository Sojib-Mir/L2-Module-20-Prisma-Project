1. mkdir folderName
2. go to prisma.io
3. v7 prisma postgres step by step commnad run
4. replace tsconfig.json file
   `{
  "compilerOptions": {
    "outDir": "./dist",
    "module": "ESNext",
    "moduleResolution": "bundler",
    "target": "ES2023",
    "types": ["node"],
    "sourceMap": true,
    "declaration": true,
    "declarationMap": true,
    "noUncheckedIndexedAccess": true,
    "strict": true,
    "isolatedModules": true,
    "noUncheckedSideEffectImports": true,
    "moduleDetection": "force",
    "skipLibCheck": true
  },
  // "include": ["src/**/*"],
  "exclude": ["node_modules", "dist"]
}`
5. create `src` folder & create `app.ts, and server.ts`
6. create db & go to `https://console.prisma.io/cmqjkxdp30u4gzndvu1mi1ycv/dashboard` & get connection string & make .env file and pase => `DATABASE_URL="connection string here" `
7. install all requerments => `express npm, cors npm, bcrypt npm, jwt npm, cookie-parser npm, http-status npm` etc
8. commnad run `npm run build`
9. setting up `server.ts, app.ts` with express
10. replace `package.json` only `sctipts` tag => `"scripts": {
  "dev": "tsx watch src/server.ts",
  "build": "tsc",
  "start": "node dist/server.js",
  "test": "echo \"Error: no test specified\" && exit 1"
},`
11. create folder `src/lib/prisma.ts`
12. commnad run `npx prisma migrate dev`
13. edit `prisma.ts` folder in `PrismaClient` delete and new import from `../../generated/prisma/client`
14. crate a file `src/config/index.ts` ===>
    `import dotenv from "dotenv";
    import path from "path";

dotenv.config({ path: path.join(process.cwd(), ".env") });

export default {
databaseUrl: process.env.DATABASE_URL,
port: process.env.PORT || 5000,
bcrypt_saltRounds: process.env.BCRYPT_SALT_ROUNDS,
app_url: process.env.APP_URL,
jwt_access_secret: process.env.JWT_ACCESS_SECRET,
jwt_refresh_secret: process.env.JWT_REFRESH_SECRET,
jwt_access_expires_in: process.env.JWT_ACCESS_EXPIRES_IN,
jwt_refresh_expires_in: process.env.JWT_REFRESH_EXPIRES_IN,
};`15. replace app.ts file ===> `import cookieParser from "cookie-parser";
import express, { Application, Request, Response } from "express";
import cors from "cors";
import config from "./config";

const app: Application = express();

app.use(
cors({
origin: config.app_url,
}),
);

app.use(express.json());
app.use(express.urlencoded({ extended: true }));
app.use(cookieParser());

app.get("/", (req: Request, res: Response) => {
res.send("Hello, World!");
});

export default app;
`
16. add server.ts ===> `const PORT = config.port;`
17. 
