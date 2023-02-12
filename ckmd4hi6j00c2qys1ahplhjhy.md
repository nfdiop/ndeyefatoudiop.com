# A Comprehensive Guide to Setting Up Next.js with ChakraUI and TypeScript

In this post, you'll learn how to set up a project with Next.js, ChakraUI, and TypeScript. 

You can find the final app on [Github](https://github.com/Joyancefa/first-nextjs-pwa). If you have any questions, comment or reach out on  [Twitter](https://twitter.com/joyancefa)  🙂.

**Prerequisites:**

- Basic Next.js, Typescript, React knowledge.
- How to use a terminal
- Familiarity with npm or yarn
- An [IDE](https://www.codecademy.com/articles/what-is-an-ide) (ex: VSCode, Atom, SublimeText, ...)

Let's dive in.

## 1. Create the Next.js app 
### Setup: 
You need to have Node.js installed: you can install it from [here](https://nodejs.org/en/). You also need a [terminal](https://itconnect.uw.edu/learn/workshops/online-tutorials/web-publishing/what-is-a-terminal/) to run the commands.

### Steps:

- Open your terminal
- Go into the directory where you want to create your app
- Run the following command : `npx create-next-app first-nextjs-pwa`. This command will create a Next.JS project named **first-nextjs-pwa** inside the **tutorials** folder. 

![installing_next_app.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1615962554674/AFpW1ROAN.png)

- Once the installation finished, navigate to the project using `cd first-nextjs-pwa`
- Start the development mode by running `yarn dev`

![next_app_started.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1615962712789/yo-72ppaF.png)

If this is successful, open [localhost:3000](http://localhost:3000/) from your browser, and you should see the app live 🎉.

## 2. Add Typescript support
### Steps:
- Open the project in your favorite IDE (I am using VSCode).
- Create an **empty** file named `tsconfig.json` in the root of your project (i.e., the file should be next to the `README.md`, `package.json`,... files)

![new_ts_config_file.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1615962964197/0aAr2BBur.png)

- Stop the development server (with CTRL-C)
- Rerun `yarn dev` => you should see the following error :

![error_when_no_typescript.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1615963066062/o8Fojcpuv.png)

- Run the following commands to fix it `yarn add --dev typescript @types/react @types/node`
- Rerun `yarn dev`. The development environment should work again, and Next.js will do the following :
   - Populate the  [tsconfig.json](https://www.typescriptlang.org/docs/handbook/tsconfig-json.html) 
   - Create the `next-env.d.ts` file, which ensures you can use Next.js types
- Migrate the following `js` files into `ts/tsx` files (the tsx extension is for files with JSX) :
  - **pages/index.js**
     - Rename into `pages/index.tsx`
  - **pages/_app.js**
     - Rename into `pages/_app.tsx`
     - Use the NextJs type `AppProps` made available through step 4.
  - **pages/api/hello.js**
     - Rename into `hello.ts`
     - Use the NextJs types `NextApiRequest`, `NextApiResponse` also made available through step 4. 		

![before_after_app.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1615963322010/vD6LVMK5H.png)

![before_after_hello.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1615963417581/tDUjyNvgW.png)

That's it: you now have Typescript support in your project 🥳.

> PS: After migrating the files, you may get an error with your development server (see on the image below)=> just restart it.

![error_file_rename.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1615963496690/km04LfCba.png)

## 3. Install ChakraUI
Chakra UI is a simple, modular, and accessible UI library for **React** applications. You can read more about it in the  [documentation](https://chakra-ui.com/docs/getting-started). 

These are the steps to install it in your project :

### Steps:
- Run the following command :
```yarn add @chakra-ui/react @emotion/react @emotion/styled framer-motion```

2. Open `pages/_app.tsx` and paste this :

```
import "../styles/globals.css";

import { AppProps } from "next/app";
import { ChakraProvider } from "@chakra-ui/react";

function MyApp({ Component, pageProps }: AppProps) {
  return (
    <ChakraProvider>
      <Component {...pageProps} />
    </ChakraProvider>
  );
}

export default MyApp;
``` 
That's it: you should now be able to use Chakra in your app 🥳. 

Chakra UI provides a  [default theme](https://chakra-ui.com/docs/theming/theme) containing color palettes, type scales, font stacks, etc... Let's customize it to make sure our app works well with Chakra.

### Customizing the Chakra theme

- Create a file called `theme.ts` inside the `styles/` folder
- Add the following lines :

```
// This will enable you to extend the theme
import { extendTheme } from '@chakra-ui/react';

// This will apply a `backgroundColor`, font `color` to the html, body elements
// Colors equivalent can be found here => https://chakra-ui.com/docs/theming/theme
const styles = {
    global: {
      "html, body": {
        color: "white",
        backgroundColor: "blue.900",
      },
    },
  };

// Use the style in the theme and return it
const theme = extendTheme({styles});
export default theme;
```
- Import the theme into the `app.tsx` file and use it in the provider. The file should look like this afterward:

```
import "../styles/globals.css";

import { AppProps } from "next/app";
import { ChakraProvider } from "@chakra-ui/react";
import theme from "../styles/theme"

function MyApp({ Component, pageProps }: AppProps) {
  return (
    <ChakraProvider theme={theme}>
      <Component {...pageProps} />
    </ChakraProvider>
  );
}

export default MyApp;
```
You should see the background color changed to blue and the font color changed to white 💪🏾. 

> PS: If you see an error like Error: ENOENT: no such file or directory... just restart your dev server.
 
![before_after_chakra_theme.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1615963866666/z9iFSYsPG.png)

## 4. (optional) Put the code on Github

Let's put your code on Github so it is shareable and you don't lose your progress.

### Steps :
- Go to your  [Github](https://github.com/)  account (create an account if you don't have one already)
- Create a new repository


![github_create_repo.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1615963917567/3rdcnv09I.png)

3. Run the following commands on your terminal inside the nextjs project you created

```
git add .
git commit -m "first commit"
git remote add origin https://github.com/Joyancefa/first-nextjs-pwa.git
git push -u origin main
```

Your app should be available on Github 🎉.

## Conclusion

Well done 💪🏾 : you now have Typescript and Chakra working with your Next.js project. 
In my next article, we will learn how to set up VSCode to make development easier.

**Find the source code here** 👉🏾 https://github.com/Joyancefa/first-nextjs-pwa






