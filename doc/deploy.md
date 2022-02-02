# How to Deploy qtepinfo

qtepinfo is splitted into 3 repos:
* [https://github.com/xcoincoder/qtepinfo](https://github.com/xcoincoder/qtepinfo)
* [https://github.com/xcoincoder/qtepinfo-api](https://github.com/xcoincoder/qtepinfo-api)
* [https://github.com/xcoincoder/qtepinfo-ui](https://github.com/xcoincoder/qtepinfo-ui)

## Prerequisites

* node.js v12.0+
* mysql v8.0+
* redis v5.0+

## Deploy qtep core
1. `git clone --recursive https://github.com/xcoincoder/qtep.git --branch=qtepinfo`
2. Follow the instructions of [https://github.com/xcoincoder/qtep/blob/master/README.md#building-qtep-core](https://github.com/xcoincoder/qtep/blob/master/README.md#building-qtep-core) to build qtep
3. Run `qtepd` with `-logevents=1` enabled

## Deploy qtepinfo
1. `git clone https://github.com/xcoincoder/qtepinfo.git`
2. `cd qtepinfo && npm install`
3. Create a mysql database and import [docs/structure.sql](structure.sql)
4. Edit file `qtepinfo-node.json` and change the configurations if needed.
5. `npm run dev`

It is strongly recommended to run `qtepinfo` under a process manager (like `pm2`), to restart the process when `qtepinfo` crashes.

## Deploy qtepinfo-api
1. `git clone https://github.com/xcoincoder/qtepinfo-api.git`
2. `cd qtepinfo-api && npm install`
3. Create file `config/config.prod.js`, write your configurations into `config/config.prod.js` such as:
    ```javascript
    exports.security = {
        domainWhiteList: ['http://example.com']  // CORS whitelist sites
    }
    // or
    exports.cors = {
        origin: '*'  // Access-Control-Allow-Origin: *
    }

    exports.sequelize = {
        logging: false  // disable sql logging
    }
    ```
    This will override corresponding field in `config/config.default.js` while running.
4. `npm start`

## Deploy qtepinfo-ui
This repo is optional, you may not deploy it if you don't need UI.
1. `git clone https://github.com/xcoincoder/qtepinfo-ui.git`
2. `cd qtepinfo-ui && npm install`
3. Edit `package.json` for example:
   * Edit `script.build` to `"build": "QTEPINFO_API_BASE_CLIENT=/api/ QTEPINFO_API_BASE_SERVER=http://localhost:3001/ QTEPINFO_API_BASE_WS=//example.com/ nuxt build"` in `package.json` to set the api URL base
   * Edit `script.start` to `"start": "PORT=3000 nuxt start"` to run `qtepinfo-ui` on port 3000
4. `npm run build`
5. `npm start`
