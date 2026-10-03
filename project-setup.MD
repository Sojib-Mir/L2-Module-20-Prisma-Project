1. ### mkdir folderName
2. ### go to prisma.io
3. ### v7 prisma postgres step by step commnad run
4. ### replace tsconfig.json file

```bash
   {
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
}

```

5. ### create `src` folder & create `app.ts, and server.ts`
6. ### create db & go to `https://console.prisma.io/cmqjkxdp30u4gzndvu1mi1ycv/dashboard` & get connection string & make .env file and pase => `DATABASE_URL="connection string here" `
7. ### install all requerments => `express npm, cors npm, bcrypt npm, jwt npm, cookie-parser npm, http-status npm` etc
8. ### commnad run `npm run build`
9. ### setting up `server.ts, app.ts` with express
10. ### replace `package.json` only `sctipts` tag =>

```bash
"scripts": {
  "dev": "tsx watch src/server.ts",
  "build": "tsc",
  "start": "node dist/server.js",
  "test": "echo \"Error: no test specified\" && exit 1"
},
```

11. ### create folder `src/lib/prisma.ts`
12. ### commnad run `npx prisma migrate dev`
13. ### edit `prisma.ts` folder in `PrismaClient` delete and new import from `../../generated/prisma/client`
14. ### crate a file `src/config/index.ts` ===>

```bash
import dotenv from "dotenv";
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

```

15. ### add server.ts ===>

```bash
const PORT = config.port;
```

16. ### src/utils/catchAsync.ts

### catchAsync.ts

```bash
import { NextFunction, Request, RequestHandler, Response } from "express";
import httpStatus from "http-status";

export const catchAsync = (fn: RequestHandler) => {
  return async (req: Request, res: Response, next: NextFunction) => {
    try {
      await fn(req, res, next);
    } catch (error) {
      console.log(error);

      res.status(httpStatus.INTERNAL_SERVER_ERROR).json({
        success: false,
        statusCode: httpStatus.INTERNAL_SERVER_ERROR,
        message: "Failed to register user",
        error: (error as Error).message,
      });
    }
  };
};

```

17. ### src/utils/sendResponse.ts

### sendResponse.ts

```bash
import { Response } from "express";

type TMeta = {
  page: number;
  limit: number;
  total: number;
};

type TResponseData<T> = {
  success: boolean;
  statusCode: number;
  message: string;
  data: T;
  meta?: TMeta;
};

export const sendResponse = <T>(res: Response, data: TResponseData<T>) => {
  res.status(data.statusCode).json({
    success: data.success,
    statusCode: data.statusCode,
    message: data.message,
    data: data.data,
    meta: data.meta,
  });
};

```

18. ### file stuctures && file name:

### commnet.controller.ts

```bash
import { NextFunction, Request, Response } from "express";
import { catchAsync } from "../../utils/catchAsync";

const createComment = catchAsync(
  async (req: Request, res: Response, next: NextFunction) => {},
);

const getCommentByAuthorId = catchAsync(
  async (req: Request, res: Response, next: NextFunction) => {},
);

const getCommentByCommentId = catchAsync(
  async (req: Request, res: Response, next: NextFunction) => {},
);

const updateComment = catchAsync(
  async (req: Request, res: Response, next: NextFunction) => {},
);

const deleteComment = catchAsync(
  async (req: Request, res: Response, next: NextFunction) => {},
);
const moderateComment = catchAsync(
  async (req: Request, res: Response, next: NextFunction) => {},
);

export const commentController = {
  createComment,
  getCommentByAuthorId,
  getCommentByCommentId,
  updateComment,
  deleteComment,
  moderateComment,
};

```

19. ### commnet.service.ts

```bash
const createComment = async () => {};

const getAllComments = async () => {};

const getCommentStats = async () => {};

const getCommentById = async () => {};

const updateComment = async () => {};

const deleteComment = async () => {};

export const commentService = {
  createComment,
  getAllComments,
  getCommentStats,
  getCommentById,
  updateComment,
  deleteComment,
};

```

20. ### commnet.route.ts

```bash
import { Router } from "express";
import { auth } from "../../middlewares/auth";
import { Role } from "../../../generated/prisma/enums";
import { commentController } from "./comment.controller";

const router = Router();

router.post(
  "/",
  auth(Role.ADMIN, Role.USER, Role.AUTHOR),
  commentController.createComment,
);

router.get(
  "/author/:authorId",
  commentController.getCommentByAuthorId,
);

router.get(
  "/:commentId",
  commentController.getCommentByCommentId,
);

router.patch(
  "/:commentId",
  auth(Role.ADMIN, Role.USER, Role.AUTHOR),
  commentController.updateComment,
);

router.delete(
  "/:commentId",
  auth(Role.ADMIN, Role.USER, Role.AUTHOR),
  commentController.deleteComment,
);

router.patch(
  "/:commentId/moderate",
  auth(Role.ADMIN, Role.USER, Role.AUTHOR),
  commentController.moderateComment,
);



export const commentRouter = router;

```

21. ### commnet.interface.ts

```bash

```
