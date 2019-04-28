# REPOMETRICS  

REPOMETRICS is a visual representation of activities in a GitHub repository. It helps you decide / settle on an open source project by showing if it's being actively worked upon or not. 

It uses your GitHub username/password to make authenticated requests from the browser directly to GitHub. If you don't provide your credentials you will eventually run into rate-limiting problems.

Hosted version: https://musing-shannon-9c15b4.netlify.com/

**NOTE: This is just a single browser page and has no back-end. This means the entered github credentials are not saved anywhere on the server.**

![](https://gitlab.com/joelewis/codemetrics/raw/master/screenshot.png)

## TODO

- ~~There's a bug (or maybe something that I'm doing wrong) that occasionally shows up as empty GitHub API responses, while fetching commit stats for a repo. Should should look into it.~~ Since commit stats APIs are compute intensive ones, GitHub serves only from caches for such API. If a repo doesn't have cached data the API returns a 202 (empty response), and kicks off a background process to compute and cache commit stats. This means, if we receive a 202 response, the same API should be hit again with a reasonable timeout.

- Theming toggle (dark mode / light mode)

- ~~Properly handle Basic Authentication inputs. Right now it's just a couple of input fields slapped onto the toolbar.~~

- ~~Present some example sets (say, [React, Vue, Ember] or [Django, Flask, Tornado]) in the UI for first time users.~~

- Write tests!

- ~~Reduce bundle Size. We are using 1% of HighChartJS & 0.1% of Vuetify. No point in shipping them entirely to the browser.~~ The production build weighs 214KB and page loads within 1.33 seconds, which is fairly reasonable.

## Project setup
```
yarn install
```

### Compiles and hot-reloads for development
```
yarn run serve
```

### Compiles and minifies for production
```
yarn run build
```

### Lints and fixes files
```
yarn run lint
```
