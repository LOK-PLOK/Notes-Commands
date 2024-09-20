## Cloning a repo from github
Navigate to your desired directory and run this command:

```bash
# clones the remote repository into your machine
git clone git@github.com:LOK-PLOK/<repo_name>.git

# change current working directory to the project
cd <repo_name>

# gets the newest version of the remote repo
git pull

# open the current directory in your preferred text editor
code .
```

Once that's done, install the project dependencies via this command: 

```bash
# a node_modules folder will appear after running this command
npm install
```

Now, run this command to start a development server that watches for file changes and automatically reloads the application.

```bash
npm run dev
```

## Commiting to github with git
Here's an example commit flow with git:

```bash
# sync latest code from the remote repository
git pull
# create a new branch based on the feature you want to work on
git checkout -b <new_branch>
# after making some changes, add and commit your work
git add .
git commit -m "category: do something"
# push your changes and make a pull request on GitHub afterwards so that I can review them
git push -u origin HEAD
```

The syntax for making `git commit -m <insert_message_here>` messages should follow this syntax for consistency:

```bash
"category: do something"
```

1. `do something` must be written in imperative tone
2. `category` must fall under these categories;
   - `feat:` introduces a new feature or component to the codebase
   - `style:` changes a layout, stylesheet, UI look of a certain component
   - `fix:` patches a bug
   - `docs:` any addition pertaining to documentation (comments, README.md, etc)
   - `nit:` small change
   - `BREAKING CHANGE:` a change that dramatically changes a pre-existing system - possibly leading to bugs to be patched
