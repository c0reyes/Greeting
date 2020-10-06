# Greeting example

This example it's base on [rest-service](https://spring.io/guides/gs/rest-service/) and [testing-web](https://spring.io/guides/gs/testing-web/)

## Clone Repository

Clone to your machine, if you want or use the repository from GitHub. Create new repository on Gitea.

```
git clone https://github.com/c0reyes/Greeting.git
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

## Kubernetes Credentials

- Generate kubeconfig

```
kubectl config view --flatten > kubeconfig.txt
```

- Configure credentials
    - Go to Jenkins pipeline project -> Credentials
    - Add Credentials
        - **Kind:** Secret file
        - **File:** (upload kubeconfig.txt file)
        - **ID:** name
        - **Description:** optional

## Test

- **/etc/hosts:** Add the line "127.0.0.1 greeting.info"
- **Open browser:** Go to http://greeting.info:8000/
