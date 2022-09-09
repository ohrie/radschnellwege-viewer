<div align="center">
    <img src="public/assets/radschnellwege-logo.svg" width="150">
    <h1>Radschnellwege Viewer</h1>
</div>

[![Deploy](https://github.com/ohrie/radschnellwege-viewer/actions/workflows/deploy.yml/badge.svg)](https://github.com/ohrie/radschnellwege-viewer/actions/workflows/deploy.yml)

Look at all the Radschnellwege in Germany planned, being built and already build. This project is depending on [radschnellwege](https://github.com/ohrie/radschnellwege), which contains the data of the cycle highways for this map.

## Initial Setup
Execute
```
npm install
```
Then create a file `.env.local` with your Mapbox Data.
```
VUE_APP_MAPBOX_ACCESS_TOKEN=<PUT_YOU_MAPBOX_ACCESS_TOKEN_HERE>
VUE_APP_MAPBOX_STYLE=<PUT_MAPBOX_STYLE_URL_HERE>
VUE_APP_UMAMI_KEY=
VUE_APP_UMAMI_URL=
```

## Serve locally

Start local server.
```
npm run serve
```

## Build
Build into `dist/` folder.
```
npm run build
```

### Lints and fixes files
```
npm run lint
```

### About
This project is based on [Vue CLI](https://cli.vuejs.org/).
Licensed under MIT License.