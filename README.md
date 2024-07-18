# Implement Next.js View Transitions
To apply transitions between pages and layouts using the View Transitions API in Next.js, you need to
### Here's a basic example of how to implement it:

##### 1. Setup Your Next.js Project
Install Nextjs
```
npx create-next-app my-app
cd my-app
```
##### 2. Install Dependencies
You need to ensure you have the necessary dependencies. Install `styled-components` for styling (if you prefer to use it), though you can use any styling solution you like.

```
npm install styled-components
npm install --save-dev babel-plugin-styled-components
```
##### 3. Configure Babel
Create or update your `.babelrc` file to use the `styled-components` plugin:
```
{
"presets": ["next/babel"],
"plugins": [["styled-components", { "ssr": true }]]
}
```
##### 4. Implement View Transitions Next, you can implement view transitions. Here's a basic example:
Create a layout component ( `components/Layout.js` ) to handle transitions between pages:

```
// components/Layout.js
import { useRouter } from 'next/router';
import { useEffect } from 'react';
import styled, { createGlobalStyle } from 'styled-components';

const GlobalStyle = createGlobalStyle`
body {
margin: 0;
padding: 0;
box-sizing: border-box;
}`;

const TransitionContainer = styled.div`
transition: opacity 0.5s;
opacity: ${({ isLeaving }) => (isLeaving? 0 : 1 )};
`;

const Layout = ({ children }) => {
const router = useRouter();
const [isLeaving, setIsLeaving] = useState(false);

useEffect(() => {
    const handleRouteChangeStart = () => {
    setIsLeaving(true);
    };
    const handleRouteChangeComplete = () => {
    setIsLeaving(false);
    };
    router.events.on('routeChangeStart', handleRouteChangeStart);
    router.events.on('routeChangeComplete', handleRouteChangeComplete);
    return () => {
        router.events.off('routeChangeStart', handleRouteChangeStart);
        router.events.off('routeChangeComplete', handleRouteChangeComplete);
    };
}, [router.events]);

return (
    <>
        <GlobalStyle />
        <TransitionContainer isLeaving={isLeaving}>
        {children}
        </TransitionContainer>
    </>
)};
export default Layout;
```
##### Update Your Pages

Wrap your pages with the `Layout` component to apply the transitions. Update `_app.js` :

```
// pages/_app.js
import Layout from '../components/Layout';

const MyApp = ({ Component, pageProps }) => {
return (
    <Layout>
    <Component {...pageProps} />
    </Layout>
)};
export default MyApp;
```
##### Create Example Pages
Create a couple of example pages ( `index.js` and `about.js` ) to demonstrate the transitions:

```
// pages/index.js
import Link from 'next/link';
const Home = () => (
    <div>
        <h1>Home Page</h1>
        <Link href="/about">
            <a>Go to About Page</a>
        </Link>
    </div>
);
export default Home;

```
```
// pages/about.js
import Link from 'next/link';

const About = () => (
<div>
    <h1>About Page</h1>
    <Link href="/">
        <a>Go to Home Page</a>
    </Link>
</div>
);
export default About;
```
##### 5. Customize Transitions
You can further customize transitions by adding more styles and adjusting the `transition` property in the `TransitionContainer` styled component.

##### 6. Run Your Application
Start your Next.js application:

```
npm run dev
```
Now, You should now see smooth transitions when navigating between the Home and About pages. Adjust the styles and transitions as needed for your specific use case.



