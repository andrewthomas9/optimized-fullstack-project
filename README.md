> # 🆕 Upgraded version is now available!
> An upgraded and more modern version with full-stack monorepo, featuring an event-driven, highly scalable architecture using Node.js, React, Redis, MongoDB, and Docker is now available,
 check out [github.com/atulmy/fullstack-event-driven-architecture](https://github.com/andrewthomas9/optimized-fullstack-project)

<br /><br />

<a href="https://docs.google.com/drawings/d/1zROz5TgqlxTYyUZmYpfboMwyyuLxX3HV0jR6tkByKzw/edit?usp=sharing"><img src="https://raw.githubusercontent.com/atulmy/storage/master/images/fullstack-javascript-architecture/architecture.png?v=1.0" alt="Full-Stack JavaScript Architecture" /></a>

# Full-Stack JavaScript Architecture

Opinionated project architecture for Full-Stack JavaScript Applications.

## About

Using JavaScript for full-stack has always been a challenge especially with architecting various pieces of the application, choosing technologies and managing devOps. This project provides a base for typical project consisting a Landing Website, Web and Mobile Applications, API service and easy deployment of these services. This project uses a microservice architecture where all individual project runs as a service (container).

A typical product (SaaS, etc.) usually consists of following services:

- Landing page
  - Used for introducing your business to customers
  - Provide links to download the mobile application
  - Provide link to access web application
  - Contact page
  - Privacy policy page
  - Terms of use page
  - SEO friendly (should support server side rendering)
- Web Application
  - Your actual application for your customers to use
  - Desktop browser
  - Tablet and mobile browser via responsive design
- Mobile Application
  - Your actual application for your customers to use
  - Android (Mobile/Tablet)
  - iOS (Mobile/Tablet)

## Core Structure

    fsja
      ├── backend
      │   ├── api
      │   │   > NodeJS
      │   │   > PORT 8000
      │   │   > api.example.com
      │   │
      │   ├── database
      │   │   > MongoDB
      │   │   > PORT 27017
      │   │
      │   └── proxy
      │       > NGINX
      │       > PORT 80 / 443
      │
      ├── deployment
      │   > Docker Compose
      │
      ├── frontend
      │   ├── app
      │   │   ├── mobile
      │   │   │   > React Native
      │   │   │   > iOS (Apple App Store)
      │   │   │   > Android (Google Play Store)
      │   │   │
      │   │   └── web
      │   │       > React
      │   │       > Single page application
      │   │       > PORT 5000
      │   │       > app.example.com
      │   │
      │   └── landing
      │       > React
      │       > Server side rendered
      │       > PORT 3000
      │       > example.com
      │
      └── README.md (you are here)

## Stack

### Backend

- API
  - NodeJS
  - Express
- Database
  - MongoDB
- Proxy
  - NGINX

### Frontend

- Landing

  - React
  - Next.js
  - Material UI
  - Server Side Rendering

- Web
  - React
  - Redux
  - React Router
  - Material UI
- Mobile (iOS, Android)
  - React Native
  - Redux
  - React Navigation

### Deployment

- Technologies
  - Docker
  - Docker compose

## Setup and Running

- Prerequisites

  - Node (`v10.x`)
  - MongoDB (`v4.x`)
  - Xcode (for iOS) (latest)
  - Android Studio (for Android) (latest)
  - Follow [React Native Guide](https://facebook.github.io/react-native/docs/getting-started) to setup your local machine

- Clone repository `git clone git@github.com:atulmy/fullstack-javascript-architecture.git fullstack`

- **API**

  - Info
    - Authentication strategy: [JWT](https://jwt.io/introduction/) (JSON Web Token)
    - Current implementation uses [RPC](https://www.jsonrpc.org/) (Remote Procedure Call) for API endpoints (one endpoint URL, multiple operations) and can be easily replaced with REST.
    - Resources
  - Switch to `api` directory `cd backend/api`
  - Configuration
    - Create local environment file `cp .env.dev.example .env.local`
    - Modify `.env.local` for
      - `PORT` (`8000`)
      - `NODE_ENV` (`development` | `production`)
      - `SECURITY_SECRET` (Use [PasswordsGenerators](https://passwordsgenerators.net))
      - `SECURITY_SALT_ROUNDS` (`10`)
      - `MONGO_URL` (`mongodb://localhost:27017/example`)
      - `LANDING_URL` (`http://localhost:3000`)
      - `WEB_URL` (`http://localhost:5000`)
      - `API_URL` (`http://localhost:8000`)
      - `EMAIL_ON` (`0` | `1` (in development, you can toggle to send emails or not))
      - `EMAIL_TEST` (send test emails to this address)
      - `EMAIL_HOST`, `EMAIL_USER`, `EMAIL_PASSWORD` (use any email service, eg. [mailgun.com](https://www.mailgun.com/) and get credentials to start sending emails
  - Setup
    - Install packages and seed database `npm run setup`
  - Run
    - Start API server: `npm start` (http://localhost:8000)

- **Landing**

  - Switch to `landing` directory `cd frontend/landing`
  - Configuration
    - Create local environment file `cp .env.dev.example .env.local`
    - Modify `.env.local` for
      - `PORT` (`3000`)
      - `NODE_ENV` (`development` | `production`)
      - `URL_LANDING` (`http://localhost:3000`)
      - `URL_WEB` (`http://localhost:5000`)
      - `URL_API` (`http://localhost:8000`)
      - `GA_TRACKING_ID` (Google analytics tracking ID)
  - Setup
    - Install dependencies: `npm install`
  - Run
    - Start Landing server: `npm start`, browse at http://localhost:3000
  - Resources
    - Use [getterms.io](https://getterms.io) to generate Privacy Policy / Terms of Use
    - Favicon generator [realfavicongenerator](https://realfavicongenerator.net)

- **Web**

  - Switch to `web` directory `cd frontend/app/web`
  - Configuration
    - Create local environment file `cp .env.dev.example .env.local`
    - Modify `.env.local` for
      - `PORT` (`5000`)
      - `REACT_APP_LANDING_URL` (`http://localhost:3000`)
      - `REACT_APP_WEB_URL` (`http://localhost:5000`)
      - `REACT_APP_API_URL` (`http://localhost:8000`)
  - Setup
    - Install dependencies: `npm install`
  - Run
    - Start Web server: `npm start`, browse at http://localhost:5000

- **Deployment**

  - Deploy code
    1. Login to the server (SSH)
    2. Create a new directory on server: `mkdir /var/www/fullstack` and switch to the directory `cd /var/www/fullstack`
    3. Clone repository `git clone git@github.com:atulmy/fullstack-javascript-architecture.git .`
    4. Switch to `deployment` directory `cd deployment`
    5. Build containers `docker-compose up --build -d`
       - `up` = Builds, (re)creates, starts, and attaches to containers for a service.
       - `--build` = Build images before starting containers
       - `-d` = Detached mode: Run containers in the background, print new container names
    6. Re-deploy with following steps:
       - Change directory `cd /var/www/fullstack`
       - Pull latest code `git pull`
       - Rebuild containers: `docker-compose up --build -d`
  - Resources
    - [Set Up Free SSL Certificates from Let's Encrypt using Docker and Nginx](https://humankode.com/ssl/how-to-set-up-free-ssl-certificates-from-lets-encrypt-using-docker-and-nginx)

- **Mobile**
  - Switch to `mobile` directory `cd frontend/app/mobile`
  - Configuration
    - Modify `src/setup/config/env.js` for
      - `APP_ENV` (`development` | `production`)
      - `LANDING_URL` (`http://<your local network IP>:3000`)
      - `WEB_URL` (`http://<your local network IP>:5000`)
      - `API_URL` (`http://<your local network IP>:8000`)
      - Tip: use `ifconfig` on macOS or Linux to get your local IP address
  - Setup
    - Install dependencies: `npm install`
  - Run
    - iOS `react-native run-ios --simulator='iPhone 8'`
    - Android `react-native run-android` (connect your Android phone via USB or use already created simulator with name `Mobile_-_5` by running `cd ~/Library/Android/sdk/tools && ./emulator -avd Mobile_-_5`)
  - Publish
    - Android
      - Build: `cd android && ./gradlew assembleRelease && cd ..`.
      - Upload `frontend/app/mobile/android/app/build/outputs/apk/release/app-release.apk` to Play Store.
    - iOS
      - Build: Open `frontend/app/mobile/ios/example.xcodeproj` in Xcode -> Choose Generic iOS Device (top left) -> Product (top menu) -> Archive.
      - Upload using Archiver (will open automatically once Archive is complete)
  - Resources
    - [From react-native init to app stores real quick](https://blog.elao.com/en/dev/from-react-native-init-to-app-stores-real-quick/)
    - iOS App icon and splashscreen generator [appicon](https://www.appicon.build/)
    - Icon generator [makeappicon](https://makeappicon.com/)
    - Icons and splashscreen generator [imagegorilla](https://apetools.webprofusion.com/app/#/tools/imagegorilla)

## Authors

- Andrew Thomas- [GitHub](https://github.com/andrewthomas9) ·

## Hire me

Looking for a developer to build your next idea or need a developer to work remotely? Get in touch

## Donate

If you liked this project, you can donate to support it ❤️

Thank you for donating:


## License

Copyright (c) 2018 Atul Yadav http://github.com/andrewthomas9

The MIT License (http://www.opensource.org/licenses/mit-license.php)
