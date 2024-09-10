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

# React Integration with Aegis Core

## Introduction

This guide walks you through the process of integrating the `@algoholics/aegis-core` NPM package into a React application to passively track user interactions like mouse movements, keystroke dynamics, browser properties, device information, network details, and more.

## Installation

To get started, install the `@algoholics/aegis-core` package:

```bash
npm i @algoholics/aegis-core
```

## Setting Up in a React Application

### 1. Create a Tracker Instance

First, you need to create a `TrackerInstance` by either providing an `apiKey` or an `engineURL`. This can be done inside a React component or in a service file depending on the structure of your project.

```javascript
import { useEffect } from 'react';
import { createTrackerInstance } from '@algoholics/aegis-core';

const tracker = createTrackerInstance({ apiKey: 'your-api-key' });
// Or you can use an engineURL
// const tracker = createTrackerInstance({ engineURL: 'https://your-engine-url.com' });
```

### 2. Initialize Tracking in a Component

You can initialize tracking in any component using React's `useEffect` hook, which ensures that the tracking starts when the component mounts.

Here's an example of how to track mouse movements and keystroke dynamics:

```javascript
import React, { useEffect } from 'react';
import { createTrackerInstance } from '@algoholics/aegis-core';

const App = () => {
  useEffect(() => {
    const tracker = createTrackerInstance({ apiKey: 'your-api-key' });
    
    // Start tracking mouse movements
    tracker.trackMouseMovement();
    
    // Start tracking keystroke dynamics
    tracker.trackKeystrokeDynamics();

    // Optionally, track other events such as browser properties, device info, etc.
    tracker.getBrowserProperties();
    tracker.getDeviceInfo();
    tracker.getNetworkDetails();
    tracker.trackTouchGestures();

    // Send data to the backend when needed
    const sendData = async () => {
      const response = await tracker.sendToBackend();
      console.log("Response from backend:", response);
    };

    // Optionally trigger sending data to the backend on component unmount
    return () => {
      sendData();
    };
  }, []);

  return (
    <div>
      <h1>Welcome to the Aegis Core Integrated App</h1>
    </div>
  );
};

export default App;
```

### 3. Sending Data to Backend on User Action

You can send buffered tracking data to the backend based on specific user interactions, such as a button click, instead of sending it on component unmount.

```javascript
import React, { useEffect } from 'react';
import { createTrackerInstance } from '@algoholics/aegis-core';

const App = () => {
  const tracker = createTrackerInstance({ apiKey: 'your-api-key' });

  useEffect(() => {
    tracker.trackMouseMovement();
    tracker.trackKeystrokeDynamics();
    tracker.getBrowserProperties();
    tracker.getDeviceInfo();
    tracker.getNetworkDetails();
    tracker.trackTouchGestures();
  }, []);

  const handleSendData = async () => {
    const response = await tracker.sendToBackend();
    console.log("Data sent to backend:", response);
  };

  return (
    <div>
      <h1>React App with Aegis Core Integration</h1>
      <button onClick={handleSendData}>Send Tracking Data</button>
    </div>
  );
};

export default App;
```

### 4. Real-Time Data Collection in Global Context (Optional)

For global data collection across different components, you can set up a `TrackerInstance` in a React context, ensuring that all user interactions are captured regardless of which component is active.

Create a `TrackerContext.js`:

```javascript
import React, { createContext, useEffect, useState } from 'react';
import { createTrackerInstance } from '@algoholics/aegis-core';

const TrackerContext = createContext();

const TrackerProvider = ({ children }) => {
  const [tracker, setTracker] = useState(null);

  useEffect(() => {
    const instance = createTrackerInstance({ apiKey: 'your-api-key' });
    instance.trackMouseMovement();
    instance.trackKeystrokeDynamics();
    instance.getBrowserProperties();
    instance.getDeviceInfo();
    instance.getNetworkDetails();
    instance.trackTouchGestures();

    setTracker(instance);

    return () => {
      // Optionally send data to backend when app is unmounted
      instance.sendToBackend();
    };
  }, []);

  return (
    <TrackerContext.Provider value={tracker}>
      {children}
    </TrackerContext.Provider>
  );
};

export { TrackerContext, TrackerProvider };
```

Now wrap your app in `TrackerProvider` in `index.js` or `App.js`:

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
import { TrackerProvider } from './TrackerContext';

ReactDOM.render(
  <TrackerProvider>
    <App />
  </TrackerProvider>,
  document.getElementById('root')
);
```

This ensures that tracking runs globally, and you can access the tracker instance anywhere in the app by consuming the context:

```javascript
import React, { useContext } from 'react';
import { TrackerContext } from './TrackerContext';

const SomeComponent = () => {
  const tracker = useContext(TrackerContext);

  const sendTrackingData = async () => {
    if (tracker) {
      const response = await tracker.sendToBackend();
      console.log("Tracking data sent:", response);
    }
  };

  return <button onClick={sendTrackingData}>Send Tracking Data</button>;
};

export default SomeComponent;
```

## Conclusion

By following this guide, you can easily integrate `@algoholics/aegis-core` into your React application and start passively tracking user interactions with minimal setup. You can customize when and how the data is sent to the backend based on your specific use case.

