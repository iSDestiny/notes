# Next.js

Next js is a react framework that is a lot more powerful than the standard create-react-app of creating
a react project. It is most useful for server side rendering which is very important if you consider
SEO to be important. With the regular CRA react app, when google accesses the web page to parse the data,
it actually does not have all the data immediately, it needs to wait for the javascript to generate
the web page and this really hurts SEO. For applications that are very dynamic, meaning that data can
change really fast such as a notetaking app or an ecommerce app, SSR is necessary. Next.js can also load
specific pages statically (SSG) and this is useful for pages of your web app that barely changes such
as something like the landing page or an about page.

## pages

Unlike your default CRA, routing is available by default in next js. Each regular tsx/jsx/ts/js file in the pages
folder are loaded in as pages. These files must export default a react component. You can nest routes by putting
the files into folders. To get the root path of a folder you must use index.js for that specific folder.

### dynamic routes

Dynamic routes are also possible by surrounding the filename with brackets. With the file post/[id].tsx, routes like /post/123 and /post/542
will point to the [id].tsx file. To extract the id from the route you can use the next.js hook, useRouter. On top of the parameters you can also extract query parameters (ex "?someparam=hello) with the same method. That is why it is important to not use the same variable for the
parameters and query parameters.

```javascript
import { useRouter } from 'next/router';

const Post = () => {
    const router = useRouter();
    const { pid } = router.query;

    return <p>Post: {pid}</p>;
};

export default Post;
```

## \_document.tsx

In order to configure your html pages to share the same things such as meta data, the header or the footer, you can use the \_document.tsx file. You must extend the
Document class because you don't want to mess with next.js default configurations in the document already. Use this file sparingly. This file's javascript is only
executed on the server, not the client. This file is not actually a real component since you can't use regular react component functionalities such as hooks or
lifecycle methods since it is only executed in the server.

```javascript
import Document, { Html, Head, Main, NextScript } from 'next/document';

export default class CustomDocument extends Document {
    render() {
        return (
            <Html>
                <Head></Head>
                <body>
                    <Main></Main>
                </body>
                <NextScript />
            </Html>
        );
    }
}
```

## \_app.tsx

This file is responsible for determining what pages are rendered on any route. Treat it as an endpoint where we can configure
a page right before it gets rendered. Useful for injecting global styles to our pages. **Global styles can't be imported in files other than \_app.txt**
This file is executed on the server (node.js) and as well as on the client.

```javascript
  static async getInitialProps(ctx) {
    const initialProps = await Document.getInitialProps(ctx)
    return { ...initialProps }
  }
```
