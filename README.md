## Automatically deploy with Travis CI and Heroku
### In this guide you will learn how to add to your stack Travis CI to run your tests and automatically deploy to Heroku when your PR is merged on GitHub.

### Step by Step:
1. Create a GitHub repository;
2. Create an account on Travis CI and link it with your repository;
3. Create an account on Heroku and link it with the repository;
4. Start the heroku console from the terminal;
5. Setup a travis yml file;
6. Push it.

### 1. Create a GitHub repository
![Create Git Repo](/img/travis-heroku.png)
Create a GitHub repository with your account and clone it using HTTPS or SSH. <br>
`git clone [your-repo]`

After clone, enter at the directory using: <br>
`cd [your-repo-name]`

### 2. Create an account on Travis CI and link it with your repository
To do that you can go to https://travis-ci.org/, to make it easier create your account using your Github.

You will see the Authorize Screen telling to accept and link Travis CI with your Github account.

Inside your account, on Accounts option click on the switch besides the repository name as the gif below.
![Create Travis Account](/img/travis.gif)
