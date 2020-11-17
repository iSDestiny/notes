# Next.js

Next js is a react framework that is a lot more powerful than the standard create-react-app of creating
a react project. It is most useful for server side rendering which is very important if you consider
SEO to be important. With the regular CRA react app, when google accesses the web page to parse the data,
it actually does not have all the data immediately, it needs to wait for the javascript to generate
the web page and this really hurts SEO. For applications that are very dynamic, meaning that data can
change really fast such as a notetaking app or an ecommerce app, SSR is necessary. Next.js can also load
specific pages statically (SSG) and this is useful for pages of your web app that barely changes such
as something like the landing page or an about page.
