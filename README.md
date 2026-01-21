<!--
  howto.md
  A step-by-step guide to integrate PsotgreSQL with Syncfusion React Scheduler using Node.Js
-->
# How to Integrate PostgreSQL with Syncfusion React Scheduler using Node.Js

This repository contains a sample full-stack application demonstrating how to synchronize events between PostgreSQL and the Syncfusion React Scheduler component.The Node.js backend handles CRUD operations on Scheduler events using a PostgreSQL database, and the React frontend delivers a modern, responsive scheduler interface for interacting with those events.


## Prerequisites

- Node.js (>= 20.19)
- npm (>= 7.0)
- react (>= 18.0)
- A PostgreSQL Database with Username and Password (create at https://www.postgresql.org/download/)
- Basic familiarity with React and PostgreSQL Query
- Make sure the ports nothing run on 8080 , 8081

## Project Structure
```
├── README.md                           # This guide
├── backend                             # Node.js backend
│   ├── config    
│   │    ├── db.config.js               # Database Configuration
│   ├── controllers
│   │    ├── scheduler.controller.js     
│   ├── models
│   │    ├── index.js     
│   │    ├── scheduler.model.js     
│   ├── routes
│   │    ├── scheduler.routes.js     
│   ├── package.json
│   └── server.js                       # Express server
├── public
│    ├── index.html
├── src
│    ├── App.css       
│    ├── App.js                         # Scheduler Configuration
│    ├── App.test.js
│    ├── index.css
│    ├── index.js
│    ├── logo.svg
│    ├── reportWebVitals.js
│    ├── setupTests.js    
├── .env                                #Environment Variables
│── package.json

```
## Setup

### Cloning the repository
    
- Clone the repository to your local machine:

### Backend Setup

### Installation
1. Open a terminal and navigate to the backend folder:
    ```bash
    cd backend
    ```
2. Install dependencies:
    ```bash
    npm install
    ```

### PostgreSql Configuration
- Create a PostgreSQL user with a chosen username and password and create a new database name as `eventdetails`.
- In `backend/config/db.config.js` file update the USER, PASSWORD, and DB as per the database configuration.
```ini
USER=<your-user-name>
PASSWORD=<password-for-specific-user>
```

### Available Endpoints
The Express server (`server.js`) exposes the following REST routes:
| Method | URL                          | Description                         |
| ------ | ---------------------------- | ----------------------------------- |
| POST    | `/getData`    | List events in the given time range |
| POST   | `/crudActions`                | Create a new ,edit and delete event.                  |                    |

### Frontend Setup

### Start Syncfusion React Scheduler 

- Open the project directory in terminal and run `npm install` to install the required packages. 

### Running the Application
1. Navigate to backend folder
      ```bash
    cd backend
    ```
2. Start the backend server:
    ```bash
    node server.js
    ```
3. Server started running on `http://localhost:8080`
4. Start the frontend:
    ```bash
    npm start
    ```
5. Navigate to [`http://localhost:8081`](http://localhost:8081) in your browser.


6. You can perform CRUD operation on the scheduler that will be reflected in the postgreSQL database table.


## Output Preview
![Frontend Preview](./Outputs/frontend.png)
*Image illustrating the Syncfusion React Scheduler*

![Database Preview](./Outputs/database.png)
*Image illustrating the events of Syncfusion React Scheduler in PostgreSQL*

## Troubleshooting
- **401 Unauthorized**: Check `User` and `Password`.
- **CORS errors**: Ensure frontend calls runs on `localhost:8081` 
<br/>
<br/>
<br/>

## A step by step guide integrate Syncfusion React Scheduler with PostgreSQL using NodeJS.

### Frontend Setup

1. Create a Syncfusion React Scheduler by following this [getting started](https://ej2.syncfusion.com/react/documentation/schedule/getting-started?cs-save-lang=1&cs-lang=js).

    Create a react app by run the following Command
    ```bash
    npm create vite@latest my-app
    ```
    or    

    To set-up a React application in JavaScript environment, run the following command.
    ```bash
    npm create vite@latest my-app -- --template react
    ```
2. Install the Required Packages in frontend by following commands
    ```bash
        npm install @testing-library/jest-dom
        npm install @testing-library/react
        npm install @testing-library/user-event
        npm install @syncfusion/ej2-react-schedule
        npm install react
        npm install react-dom
        npm install react-scripts
        npm install web-vitals

    ```
    After that you replace this lines in scripts and add browserslist in package.json to defines commands with npm

    ```bash
        
        "scripts": {
          "start": "react-scripts start",
          "build": "react-scripts build",
          "test": "react-scripts test",
          "eject": "react-scripts eject"
        },
        "browserslist": {
            "production": [
              ">0.2%",
              "not dead",
              "not op_mini all"
            ],
            "development": [
              "last 1 chrome version",
              "last 1 firefox version",
              "last 1 safari version"
            ]
        }

    ```
3. Create a `App.js` file in `src` folder to define url and crudurl also define schedule component
    ```bash
        
    import * as React from 'react';
    import { ScheduleComponent, Day, Week, WorkWeek, Month, Agenda, Inject, Resize, DragAndDrop } from '@syncfusion/ej2-react-schedule';
    import { DataManager,  UrlAdaptor } from '@syncfusion/ej2-data';

    import "../node_modules/@syncfusion/ej2-base/styles/material.css";
    import "../node_modules/@syncfusion/ej2-buttons/styles/material.css";
    import "../node_modules/@syncfusion/ej2-calendars/styles/material.css";
    import "../node_modules/@syncfusion/ej2-dropdowns/styles/material.css";
    import "../node_modules/@syncfusion/ej2-inputs/styles/material.css";
    import "../node_modules/@syncfusion/ej2-navigations/styles/material.css";
    import "../node_modules/@syncfusion/ej2-popups/styles/material.css";
    import "../node_modules/@syncfusion/ej2-schedule/styles/material.css";

    function App() {
      const dataManager = new DataManager({
        url: 'http://localhost:8080/api/scheduleevents/getData',
        crudUrl: 'http://localhost:8080/api/scheduleevents/crudActions',
        adaptor: new UrlAdaptor(),
        crossDomain: true
    });
    return (
    <div className="App">
      <ScheduleComponent width='100%' height='650px' currentView='Month' eventSettings={{ dataSource: dataManager,
       fields: {
        id: 'id',
        subject: { name: 'subject' },
        isAllDay: { name: 'isallday' },
        location: { name: 'location' },
        description: { name: 'description' },
        startTime: { name: 'starttime' },
        endTime: { name: 'endtime' },
        startTimezone: { name: 'starttimezone' },
        endTimezone: { name: 'endtimezone' },
        recurrenceID: {name:'recurrenceid'},
        recurrenceRule:{name:'recurrencerule'},
        recurrenceException: {name:'recurrenceexception'},
        followingID:{name:'followingid'}
      } }}>
              <Inject services={[Day, Week, WorkWeek, Month, Agenda, Resize, DragAndDrop]}/>
            </ScheduleComponent>
        </div>
        );
        }

        export default App;

    ```
4. Create a `index.js` in src folder to call the App Component and update the Id root according to index.html.
    ```bash
        import React from 'react';
        import ReactDOM from 'react-dom/client';
        import App from './App.js';
        import reportWebVitals from './reportWebVitals.js';

        const root = ReactDOM.createRoot(document.getElementById('root'));
        root.render(
        
            <App />

        );

        // If you want to start measuring performance in your app, pass a function
        // to log results (for example: reportWebVitals(console.log))
        // or send to an analytics endpoint. Learn more: https://bit.ly/CRA-vitals
        reportWebVitals();

    ```

5. Create a `reportWebVitals.js` in the `src` folder with this code
   ```bash 
    const reportWebVitals = onPerfEntry => {
    if (onPerfEntry && onPerfEntry instanceof Function) {
        import('web-vitals').then(({ getCLS, getFID, getFCP, getLCP, getTTFB }) => {
        getCLS(onPerfEntry);
        getFID(onPerfEntry);
        getFCP(onPerfEntry);
        getLCP(onPerfEntry);
        getTTFB(onPerfEntry);
        });
    }};

    export default reportWebVitals;
    ```
6. Create a `.env` file inside the react app
    ```bash
        PORT=8081
    ```
7. Place the `index.html` inside the `public` folder
    ```bash
        <!DOCTYPE html>
        <html lang="en">

        <head>
          <meta charset="utf-8" />
          <link rel="icon" href="%PUBLIC_URL%/favicon.ico" />

          <meta name="viewport" content="width=device-width, initial-scale=1" />
          <meta name="theme-color" content="#000000" />
          <meta name="description" content="Web site created using create-react-app" />
          <link rel="apple-touch-icon" href="%PUBLIC_URL%/logo192.png" />
         
          <link rel="manifest" href="%PUBLIC_URL%/manifest.json" />
          
          <title>React App</title>
        </head>

        <body>
          <noscript>You need to enable JavaScript to run this app.</noscript>
          <div id="root"></div>
          
        </body>

        </html>
    ```
<br/>
<br/>


### Backend and PostgreSQL Setup
1. Download PostgreSQL from [PostgreSQL](https://www.postgresql.org/download/)

2. Create a Username , Password and Database in PostgreSQL

3. Create a `backend` folder

4. Generate the `package.json` with default settings to install the package inside the `backend` folder by run 
    ```bash
        npm init -y
    ```
    
5. Install neccesary packages for backend by following commands
    ```bash
        npm install body-parser
        npm install cors
        npm install express
        npm install pg
        npm install sequelize
        npm install pg-hstore
    ```

    Add this lines after in package.json to defines commands with npm
    ```bash
        "scripts": {
            "test": "echo \"Error: no test specified\" && exit 1",
            "start": "node server.js"
        },
    ```
6. Create a `config` folder inside `backend`

7. Connect database with scheduler by create `db.config.js` inside `config` folder in `backend` and replace your `USER` , `PASSWORD` and `DB` with your credentials
    ```bash
    module.exports = {
        HOST: "localhost",
        USER: "username",
        PASSWORD: "password",
        DB: "database",
        dialect: "postgres",
        pool: {
        max: 5,
        min: 0,
        acquire: 30000,
        idle: 10000
        }
    };
    ```
8. Create a `controllers` folder in `backend`
9. Implement logic for CRUD Operations in `scheduler.controller.js` inside `controllers` folder in `backend`
    ```bash
            const db = require("../models");
            const SchedulerEvents = db.scheduler;

            exports.crudActions = (req, res) => {

        if (req.body.added !== null && req.body.added.  length > 0) {
            for (var i = 0; i < req.body.added.length; i++) {
                var insertData = req.body.added[i];
                SchedulerEvents.create(insertData)
                    .then(data => {
                    res.send(data);
                    })
                    .catch(err => {
                        res.status(500).send({
                        message:
                            err.message || "Some error occurred while inserting the events."
                        });
                    });
                    }
                }

    if (req.body.changed !== null && req.body.changed.length > 0) {
        for (var i = 0; i < req.body.changed.length; i++) {
            var updateData = req.body.changed[i];
            SchedulerEvents.update(updateData, { where: { id: updateData.id } })
                .then(num => {
                    if (num == 1) {
                        res.send(updateData);
                    } else {
                        res.send({
                            message: `Cannot update Event with id=${id}. Maybe Event was not found or req.body is empty!`
                        });
                    }
                })
                .catch(err => {
                    res.status(500).send({
                        message: "Error updating Event with id=" + id
                    });
                });
        }
    }

    if (req.body.deleted !== null && req.body.deleted.length > 0) {
        for (var i = 0; i < req.body.deleted.length; i++) {
            var deleteData = req.body.deleted[i];
            SchedulerEvents.destroy({ where: { id: deleteData.id } })
                .then(num => {
                    if (num == 1) {
                        res.send(deleteData);
                    } else {
                        res.send({
                            message: `Cannot delete Event with id=${id}. Maybe Event was not found!`
                        });
                    }
                })
                .catch(err => {
                    res.status(500).send({
                        message: "Could not delete Event with id=" + id
                    });
                });
        }
    }
    };

        exports.getData = (req, res) => {
        SchedulerEvents.findAll()
        .then(data => {
            res.send(data);
        })
        .catch(err => {
            res.status(500).send({
                message:
                    err.message || "Some error occurred while retrieving Events."
            });
        });
    };

    ```
10. Create a `models` folder in backend
11. Create a `index.js` inside `models` folder in `backend`
    ```bash
        const dbConfig = require("../config/db.config.js");
        const Sequelize = require("sequelize");
        const sequelize = new Sequelize(dbConfig.DB, dbConfig.USER, dbConfig.PASSWORD, {
          host: dbConfig.HOST,
          dialect: dbConfig.dialect,
          operatorsAliases: false,
          pool: {
            max: dbConfig.pool.max,
            min: dbConfig.pool.min,
            acquire: dbConfig.pool.acquire,
            idle: dbConfig.pool.idle
          }
        });
        const db = {};
        db.Sequelize = Sequelize;
        db.sequelize = sequelize;
        db.scheduler = require("./scheduler.model.js")(sequelize, Sequelize);
        module.exports = db;
     ```


12. Create a `scheduler.model.js` inside `models` in `backend`. This code automatically create table inside the database.

    ```bash
        module.exports = (sequelize, Sequelize) => {
        const SchedulerEvents = sequelize.define   ("scheduleevents", {
        id: {
            type: Sequelize.INTEGER,
            primaryKey: true,
            autoIncrement: true,
        },
        starttime: {
            type: Sequelize.DATE,
            allowNull: false
        },
        endtime: {
            type: Sequelize.DATE,
            allowNull: false
        },
        subject: {
            type: Sequelize.STRING
        },
        location: {
            type: Sequelize.STRING
        },
        description: {
            type: Sequelize.STRING
        },
        isallday: {
            type: Sequelize.BOOLEAN
        },
        starttimezone: {
            type: Sequelize.STRING
        },
        endtimezone: {
            type: Sequelize.STRING
        },
        recurrencerule: {
            type: Sequelize.STRING
        },
        recurrenceid: {
            type: Sequelize.INTEGER
        },
        recurrenceexception: {
            type: Sequelize.STRING
        },
        followingid: { 
            type: Sequelize.INTEGER 
        },
        createdAt: {
            type: Sequelize.DATE,
            field: 'created_at'
          },
    
          updatedAt: {
            type: Sequelize.DATE,
            field: 'updated_at'
          }
    });
    return SchedulerEvents;
    };

    ```
13. Create a `routes` folder in `backend`

14. Define routes in `scheduler.routes.js` inside `routes` folder  in `backend`
    ```bash
        module.exports = app => {
            const scheduleService = require("../controllers/scheduler.controller.js");
            var router = require("express").Router();
            router.post("/getData", scheduleService.getData);
            router.post("/crudActions", scheduleService.crudActions); 
            app.use('/api/scheduleevents', router);
    };
    ```
15. Create a `server.js` inside `backend` to start backend server
    ```bash
        const express = require("express");
        const bodyParser = require("body-parser");
        const cors = require("cors");
        const app = express();
        var corsOptions = {
          origin: "http://localhost:8081"
        };
        app.use(cors(corsOptions));
        // parse requests of content-type - application/json
        app.use(bodyParser.json());
        // parse requests of content-type - application/x-www-form-urlencoded
        app.use(bodyParser.urlencoded({ extended: true }));
        app.get("/", (req, res) => {
          res.json({ message: "Welcome to Scheduler backend service." });
        });
        require("./routes/scheduler.routes")(app);
        // set port, listen for requests
        const PORT = process.env.PORT || 8080;
        app.listen(PORT, () => {
          console.log(`Server is running on port ${PORT}.`);
        });

        const db = require("./models");
        db.sequelize.sync({ force: false }).then(() => {
          console.log("Drop and re-sync db.");
        });
    ```


## Running the Application
1. navigate to the backend folder:
    ```bash
    cd backend
    ```
2. Start the backend server:
    ```bash
    node server.js
    ```
3. Server started running on `http://localhost:8080` or you Change the port no in `server.js`
4. Start the frontend:
    ```bash
    npm start
    ```
5. Navigate to [`http://localhost:8081`](http://localhost:8081) in your browser or you change the port no

6. You can perform CRUD operation on the scheduler that will be reflected in the MySQL database table.



