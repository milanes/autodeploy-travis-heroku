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

### 3. Create an account on Heroku and link with the repository
At https://www.heroku.com/ create a new account, you need to confirm your email and all that stuff. When you have finished, click on "Create New App".

Inside your application, go to "Deploy", search for "Deployment method" and then select the GitHub option.

A wild window appears, "Authorize Heroku", accept it, and to complete the connection just search by the name of the repository and connect!
![Create Heroku App](/img/heroku.gif)

At Deploy tab on the “Automatic deploys” section don't forget to check the "Wait for CI to pass before deploy" option and enable the Automatic Deploys:
![Create Heroku autodeploy](/img/autodeploy.gif)

### 4. Start the Heroku console from the terminal
Now that you have Heroku linked with the repo, install the Heroku CLI and login with our new account.

#### 4.1 Install 
To install the Heroku CLI just follow the official documentation, if you are using a Linux Debian like me, just use: <br>
`$ sudo snap install --classic heroku` <br>
To verify your CLI installation, use the `heroku --version` command.

#### 4.2 Getting started
After you install the CLI, run the heroku login command. You’ll be prompted to enter any key to go to your web browser to complete login. The CLI will then log you in automatically.
```
$ heroku login
heroku: Press any key to open up the browser to login or q to exit
 ›   Warning: If browser does not open, visit
 ›   https://cli-auth.heroku.com/auth/browser/***
heroku: Waiting for login...
Logging in... done
Logged in as me@example.com
```
##### Create a remote reference to your repo:
`$ heroku git:remote -a autodeploy-travis-heroku` <br><br>
Done! Now you can directly push to your Heroku and deploy your app by the terminal, however, we will learn in the last steps how to put all these things together!

### 5. Setup a travis yml file
#### 5.1 Let's start creating a file
`$ touch .travis.yml` 
#### 5.2 Now open the created file: 
```
language: node_js

cache:
- node_modules
deploy:
  provider: heroku
  api_key:
    secure: KEY
  app: autodeploy-travis-heroku
  on:
    repo: milanes/autodeploy-travis-heroku
```

In this yml file I’m defining:
- The language to the Travis CI knows what to do to run my code;
- What I want to cache, in this case is the node_modules
- Creating a deploy task to run at Heroku, to do that Is needed the API KEY (we don't have it yet), the name of the app at Heroku(autodeploy-travis-heroku) and the name of the repo at Github(milanes/autodeploy-travis-heroku)

#### 5.3 Get API KEY
To get the API KEY from Heroku just run a command on terminal, although this key needs to be secret so we should encrypt that before putting it in the file.
Install the Travis CI gem to be able to use the encrypt from Travis:<br>
`$ gem install travis`  <br>
Now, run this code that will envoque the encrypt and pass the Heroku API KEY <br>
`$ travis encrypt $(heroku auth:token)`
<br>
You probabaly will see this message: `Detected repository as yourname/reponame, is this correct? |yes|`

Answer with yes and you have your API KEY!
*So replace the KEY at the secure: line on the yml file by your key.*

### 6 Push it
In this last step, push everything you did to GitHub, commit the yml file and push it to a new branch at the repo.
```
$ git branch add-travis-yml-file
$ git checkout add-travis-yml-file
$ git add .
$ git commit -m "Add travis yml file"
$ git push -u origin add-travis-yml-file
```
Inside your repository on Github open a new PR from your new branch, target the master and the CI will be running there.
![Create Heroku autodeploy](/img/add-travis.png)

When you merge the PR, go to your Heroku Dashboard and check the lastest activity.

Now evertytime you open a PR Travis will be running the tests and when this PR were merged Heroku will deploy it automatically!


### Bibliography
https://medium.com/@felipeluizsoares/automatically-deploy-with-travis-ci-and-heroku-ddba1361647f <br>
https://docs.travis-ci.com/ <br>
https://devcenter.heroku.com/ <br>
https://github.com/yodra/node-travis-hello-world <br>
