# Twitter-Shadow

This is an exercise for React&Typescript by build a very simple Twitter. There is no backend involved and all the data is stored locally.

This is a [Next.js](https://nextjs.org/) project bootstrapped with [`create-next-app`](https://github.com/vercel/next.js/tree/canary/packages/create-next-app).

## Getting Started

Run the development server:

```bash
npm run dev
# or
yarn dev
```

Open [http://localhost:3000](http://localhost:3000) with your browser to see the result.

## Tech Stack

- [React](https://reactjs.org/docs/hooks-intro.html) with Functional Components and Hooks
- [Typescript](https://www.typescriptlang.org/docs/handbook/2/types-from-types.html)
- [Nextjs](https://nextjs.org/docs/getting-started) project bootstrapper
- [emotion](https://github.com/emotion-js/emotion) CSS-in-JS support
- [RuetifyUI](https://github.com/KyLoc20/Ruetify-UI) self-hosted UI lib adherent to [Material-UI](https://mui.com/) and [Vuetify](https://vuetifyjs.com/en/)

## Features

- Register&Login&Logout
  - [ ]as options in the timeline page
  - [ ]unique username password without security
  - [ ]transfer to his timeline page with the path (/{username}) after a user successfully registers or logs in
  - [ ]transfer to the home page with the path (/) after a user logs out
- Tweets in the timeline page
  - [ ]all tweets are readable but can be edited or deleted only by the posters
  - [ ]tweets are in a descending order
  - [ ]each tweet owns a unique incremental integer ID
  - [ ]users can post text tweets
  - [ ]transfer to the detail page with the path (/tweet/{tweet ID}) after clicking a tweet
- Tweets in the detail page
