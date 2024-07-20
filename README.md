# Vue Learning with Crash Course (from Brad Traversy)

This is simple project made during Vue Crash Course 2024 ([link](https://www.youtube.com/watch?v=VeNfHj6MhgA)) and is a little showcase of my skills.

This is simple single page application with dev job offers magement (what a coincidence 🤔 i'm currently looking for a job) with backend api (json-server).

## Using
1. Clone repo
2. (Optional) In `vite.config.js` you can change port on which `json-server` will be running
```javascript
proxy: {
      '/api': {
        target: 'http://localhost:5177', // <- change port here
        changeOrigin: true,
        rewrite: (path) => path.replace(/^\/api/, ''),
      },
```
3. Start dev server with `npm run dev` (this will run cocurrently `vite` server alongside with `json-server`)

## Info
Lot of original code was writen by [Brad Traversy](https://github.com/bradtraversy/vue-crash-2024), but I made a lot of changes for myslef (this is only for learning purposes, code from this repo is not shared on any open license)