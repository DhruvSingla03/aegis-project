# THE AEGIS PROJECT

Checkout out all the github repos

- [Aeigis-Core](https://www.npmjs.com/package/@algoholics/aegis-core) : Simple framework agnostic Npm package allowing to integrate passive tracking to your website. [GitHub](https://github.com/killerz3/aegis-core)
- [Aegis-engine](https://github.com/killerz3/aegis-engine) : Self-hostable Python Microservice for ml infrence . [GitHub](https://github.com/killerz3/aegis-engine)
- [Aegis-India](https://aegis-india.vercel.app/) : Fully hosted plugin-play solution easily integrated with **aegis-core** [Github](https://github.com/killerz3/aegis-engine)
- [docs-aegis-india](https://docs-aegis-india.vercel.app/) : DOCS [GitHub](https://github.com/killerz3/docs-aegis-india)

- PPT : [LINK](https://drive.google.com/file/d/1FME6w8NwahZOooB36-OikMDuf_E_dxH3/view?usp=drive_link)
- Videov: [LINK]()

## Local Setup Instructions for aegis-india (Windows/Macos/Linux)

Follow these steps to run the project locally

1. **Clone the Repository**

```bash
   git clone https://github.com/killerz3/social_calc/tree/main
   cd social_calc
   npm install
   npx auth secret
```

2. **Setup google OAUTH Credential**
   follow these steps to generate client id and Oauth secret : [LINK](https://support.google.com/cloud/answer/6158849?hl=en)
3. **Create .env**

```
AUTH_SECRET="" # Added by `npx auth secret`

AUTH_GOOGLE_ID=<GOOGLE_OAUTH_CLIENT_ID>

AUTH_GOOGLE_SECRET=<GOOGLE_OAUTH_SECRET>


DATABASE_URL=<POSTGRES_DATABASE_URL>
DIRECT_URL=<DIRECT_DB_URL>
```

4. **Setup Prisma**

```bash
   npx prisma generate
   npx prisma migrate dev
   npx prisma db push
```

5. **Run App Locally**

```bash
npm run dev
```

6. **View Database**
   you can view your database on https://localhost:5555/

```
npx prisma studio
```

Your app would be ready at https://localhost:3000/
