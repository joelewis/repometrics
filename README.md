# REPOMETRICS  

REPOMETRICS helps you to check the activity on Github repos, before deciding to adopt the open source project in your product.
It uses your GitHub username/password to make authenticated requests from the browser directly to GitHub. If you don't provide your credentials you will eventually run into rate-limiting problems. Made with ❤ using Vuetify and GitHub API V3 , and some amounts of an Easter weekend.

Hosted version: https://musing-shannon-9c15b4.netlify.com/

**NOTE: This is just a single browser page and has no back-end. This means the entered github credentials are not saved anywhere on the server.**

## TODO

- There's a bug (or maybe something that I'm doing wrong) that occasionally shows up as empty GitHub API responses, while fetching commit stats for a repo. Should should look into it.

- Theming toggle (dark mode / light mode)

- Properly handle Basic Authentication inputs. Right now it's just a couple of input fields slapped onto the toolbar. 

- Present some example sets (say, [React, Vue, Ember] or [Django, Flask, Tornado]) in the UI for first time users.

- Write Tests!

- Reduce bundle Size. We are using 1% of HighChartJS & 0.1% of Vuetify. No point in shipping them entirely to the browser.

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
