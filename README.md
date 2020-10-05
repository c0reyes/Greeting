# Greeting example

This example it's base on [rest-service](https://spring.io/guides/gs/rest-service/) and [testing-web](https://spring.io/guides/gs/testing-web/)

## Clone Repository

Clone to your machine if you want or use the repository from GitHub. Create new repository on Gitea.

```
git clone ....
```

Change remote url to local Gitea.

```
git remote remove origin
git remote set-url origin http://localhost:3000/git/Greeting.git
git push
```

## Configure Pipeline

- **Open browser:** Go to http://localhost:8080/
- **Jenkins:** Open Blue Ocean
    - New Pipeline
    - Select Git
    - Write repository url and Create pipline

## Test

- **/etc/hosts:** Add the line "127.0.0.1 greeting.info"
- **Open browser:** Go to http://greeting.info:8000/
