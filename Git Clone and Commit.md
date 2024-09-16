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
git add .
git commit -m "category: do something"
git push origin main
```

The syntax for making `git commit -m <insert_message_here>` messages should follow this syntax for consistency:

```bash
"category: do something"
```

1. `do something` must be written in [imperative tone](https://www.theserverside.com/video/Follow-these-git-commit-message-guidelines#:~:text=If%20you%20want%20to%20write,Instead%2C%20describe%20what%20was%20done.).
2. `category` must fall under these categories;
   - `feat:` introduces a new feature or component to the codebase
   - `style:` changes a layout, stylesheet, UI look of a certain component
   - `fix:` patches a bug
   - `docs:` any addition pertaining to documentation (comments, README.md, etc)
   - `nit:` small change based on some sort of convention, see [this SO question](https://stackoverflow.com/questions/27810522/what-does-nit-mean-in-hacker-speak).
   - `BREAKING CHANGE:` a change that dramatically changes a pre-existing system - possibly leading to bugs to be patched
