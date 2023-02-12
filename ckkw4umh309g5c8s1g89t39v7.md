# (Work in progress 😅) Goalswin - Set your goals and track your habits

This is embarrassing ... 

I thought I would finish my app on time,  I was wrong 😅. 

In fact, 3 weeks ago, I stumbled across the hackathon announcement and decided to give it a go. 

I collected some design ideas, shared my mocks on Twitter, and optimistically started developing... 

Despite not finishing the app, I would like to share with you some learnings while you can find the code  [here](https://github.com/Joyancefa/goalswin).

%[https://twitter.com/joyancefa/status/1353349268707405824]

# But what to build?

After prolonged reflection, I settled on a **goals tracker app**. I know... there is a lot of them, so why another one? 

I would say for **4 reasons** :

- I always feel behind when I use the current solutions: there is always something to do and I **don't see my progress**

- I can't see the **high picture** with the current apps: I can track goals, I can track habits, but I don't see the link

- I want an app that combines **Pomodoro + goals tracker + actions/habits tracker **

- I want to **own my data completely**

## The first step: Design

I love designing UIs. I am not that skilled, but I love it.

As a result, I spent ~20h on Adobe XD making the app mocks 🤦🏾‍♀️. I collected inspirations on Dribble, picked a color (red for energy), a font - "Karla", etc...

You can find some of the results below. Let me know what you think ;) 

![original mocks light](https://pbs.twimg.com/media/EsgPERNW8AAaTXR?format=jpg&name=large)
![original mocks dark](https://pbs.twimg.com/media/EsgPERNXMAIg27B?format=jpg&name=large)

## Second Step: Pick a stack

Along with UI design, I love learning new things. So for the challenge, I decided to go for the following tech stack:

- Typescript  + Eslint
-  [Next.js](https://nextjs.org/) 
- [Formik](https://formik.org/)  => for forms validations + creation
- [Chakra](https://chakra-ui.com/)  => for React components
- [Prisma](https://www.prisma.io/) => for handling databases
- [NextAuth](https://next-auth.js.org/) => for authentication
- [SWR](https://swr.vercel.app/) 
- and  [React-Select](https://react-select.com/home) for input selects

## Third Step: The development + challenges

I wouldn't want to use a **slow web app**. So, perf was a priority for me. Because of that, and early optimizations, I stumbled upon a series of issues :
 

### 1. Preact != React

**Symptoms**

My code would work locally but not in production. I tried many things but couldn't find the culprit. 

**Problem**

I was using Preact in production (because it can trim >20kb of JS) and React in dev mode. What I didn't know was that Preact doesn't work well with some  [Chakra UI components](https://github.com/chakra-ui/chakra-ui/issues/3099) or [NextAuth](https://github.com/nextauthjs/next-auth/issues/838).


### 2. Responsiveness and server-side rendering

**Problem: **

Media queries don't work on the server. As a result, I was shipping some useless code for mobiles. 

**Solution**

- Estimate if the user had a mobile by looking at the user agent in  [getServerSideProps](https://github.com/Joyancefa/goalswin/blob/ea8524cb895d2c7b250755ccba110351787b2cb4/src/components/Chakra.tsx#L45)  
- Extensively use Next.js [dynamic imports](https://nextjs.org/docs/advanced-features/dynamic-import) to only ship code when needed


### 3. Picking a database

**Dilemna**

I hesitated a lot between Firebase, Prisma, and Faunadb and listed the following pros and cons :

-  [**Firebase**](http://firebase.google.com/) 

**Pros:** I already used it, it handles authorization well + It is free + I don't need a BE

**Cons:** The billing is not clear + I will learn nothing new 🙈

- [**FaunaDB**](https://fauna.com/)

**Pros:** It looks really efficient + It handles authorization well + It has a free tier + I learn something new

**Cons:** The billing is not clear too + I couldn't use multi authentication in the free tier 

- [**Prisma**](https://www.prisma.io//)

**Pros:** It integrates well with Next.js + very friendly documentation + I can use it with NextAuth + I learn something new

**Cons:** I need to set up my own DB + I need to handle the authorization

**Resolution**

I ended up choosing Prisma and I set up a Postgres instance with AWS :). 

### 4. Performance

**Problem**

I would either :
- render too many components on the server, 
- or use dynamic imports aggressively (which creates too many chunks)
- or not use dynamic imports enough and have big chunks sizes

**Resolution**
This is still in progress. But, by using a combination of  [console coverage](https://developers.google.com/web/tools/chrome-devtools/coverage) + Lighthouse, I was able to identify some culprits.

### 4. Encrypt user data (still searching)

I want to have end-to-end encryption for the user but all the solutions I could find ( [Virgil Security](https://virgilsecurity.com/blog/announcing-firebase-sdk#:~:text=You%20can%20now%20add%20End,mirrors%2C%20it's%20super%2Dsimple.), [Seald](https://www.seald.io/blog/an-encrypted-chat-app-with-react-hooks-firebase-and-seald)) were expensive.

At this point, I hesitate between using IndexDB (and store the data on the user desktop/phone) or use encryption on Node.js.

### 5. NextAuth not working in production Vercel (still looking for a fix😥)

## What is next?

I am super excited about this app and I will be actively building it. If you are interested or have any idea, ping me on Twitter 💪🏾❤️

Thanks #Hashnode + #Vercel for organizing this great competition.

App => http://goalswin.vercel.app/

Github => http://goalswin.vercel.app/

Twitter => https://twitter.com/joyancefa