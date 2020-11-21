# Useful Snippets

## Mongoose connect middleware for next.js ([source](https://github.com/vercel/next.js/discussions/13280))

```javascript
import mongoose from 'mongoose';

const connect = async () => {
    await mongoose
        .connect(process.env.MONGO_URI, {
            useNewUrlParser: true,
            useFindAndModify: false,
            useUnifiedTopology: true
        })
        .then(() => console.log(`Mongo running at ${process.env.MONGO_URI}`))
        .catch((err) => console.log(err));
};

const connectDB = (handler) => async (req, res) => {
    if (mongoose.connections[0].readyState !== 1) {
        await connect();
    }
    return handler(req, res);
};

const db = mongoose.connection;
db.once('ready', () =>
    console.log(`connected to mongo on ${process.env.MONGO_URI}`)
);
export default connectDB;
```

## Another mongoose middleware but with models ([source](https://codeconqueror.com/blog/using-a-database-with-next-js))

```javascript
// api/user/index.js
import mongoose from 'mongoose';
import UserSchema from '../../../data/models/User';

const connectToMongo = async () => {
    const connection = await mongoose.createConnection(
        'mongodb://localhost:27017/nextjs',
        {
            useNewUrlParser: true,
            bufferCommands: false,
            bufferMaxEntries: 0,
            useUnifiedTopology: true
        }
    );
    const User = connection.model('User', UserSchema);
    return {
        connection,
        models: {
            User
        }
    };
};

const apiHandler = (res, method, handlers) => {
    if (!Object.keys(handlers).includes(method)) {
        res.setHeader('Allow', Object.keys(handlers));
        res.status(405).end(`Method ${method} Not Allowed`);
    } else {
        handlers[method](res);
    }
};

const mongoMiddleware = (handler) => async (req, res) => {
    const { connection, models } = await connectToMongo();
    try {
        await handler(req, res, connection, models);
    } catch (e) {
        connection.close();
        res.status(500).json({ error: e.message || 'something went wrong' });
    }
};

// Create User
export default mongoMiddleware(async (req, res, connection, models) => {
    const {
        query: { name },
        method
    } = req;
    apiHandler(res, method, {
        POST: (response) => {
            models.User.create({ name }, (error, user) => {
                if (error) {
                    connection.close();
                    response.status(500).json({ error });
                } else {
                    response.status(200).json(user);
                    connection.close();
                }
            });
        }
    });
});
```

## Socket io with next.js api routes [source](https://github.com/vercel/next.js/issues/8311)

**NOTE: Must use custom server, [reference](https://nextjs.org/docs/advanced-features/custom-server)**

```javascript
import { parse } from 'url';
import next from 'next';
import socketIO from 'socket.io';
import { createServer } from 'http';

const context = {
    io: null
};

const dev = process.env.NODE_ENV !== 'production';
const app = next({ dev });
const handle = app.getRequestHandler();

const requestListener = (req: any, res: any) => {
    // Be sure to pass `true` as the second argument to `url.parse`.
    // This tells it to parse the query portion of the URL.
    const parsedUrl = parse(req.url, true);
    req.context = context;
    handle(req, res, parsedUrl);
};

app.prepare().then(() => {
    const port = parseInt(process.env.PORT || '3000', 10);

    const server = createServer(requestListener);

    context.io = socketIO(server);
    context.io.on('connection', (client) => {
        console.log(client.id, 'connected');
        client.on('disconnect', () => {
            console.log(client.id, 'disconnected');
        });
    });

    server.listen(port, () => {
        console.log(`> Ready on http://localhost:${port}`);
    });
});
```
