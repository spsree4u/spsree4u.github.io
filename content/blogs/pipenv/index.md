+++
title = "Start with pipenv"
date = "2022-09-22T13:52:22+05:30"
+++

I felt this is a good article for any Python engineers to know. And if you are going through my projects, you may end up seeing the setup using pipenv. 

If you don't want to read it all, feel free to navigate through sections

**[What is pipenv and it's uses](#an-intro-to-pipenv-and-pipfiles)**  
**[Useful pipenv commands](#useful-pipenv-commands)**  
**[Use case scenarios](#use-case-scenarios)**

# An intro to pipenv and pipfiles
Pipenv is a strong tool to help with the package management and virtual environment creation in a python based project. This actually combines the old coventional way of doing the same with requirements file, pip and virtualenv.  Here are some of the advantages of using pipenv.

1. Combines virtual environment creation and package installations together
2. Can create multiple environments with different python versions very easily
3. Package dependencies are automatically handled, so addition or deletion of packages is very easy
4. Provision to distinguish dev and production requirements
5. Can easily convert existing old system (requirements.txt file and virtualsenv) to pipenv system
6. It is possible to view the security vulnerabilities in the packages used using a single command

Now let's start with installing pipenv first. To install pipenv: 
```
pip install pipenv
```

Below section contains the most useful commands associated with pipenv. Before going through these commands, let's understand what Pipfiles are.

* Pipfile:
    - This file contain the packages and dependencies. This will be used for creating the virtual environement.
    - The file is in TOML format (key-value pairs within different sections)
* Pipfile.lock:
    - This file is same as Pipfile, but will be the final file used in Production systems
    - The file is in json format with package version mentioned inside
    - We can finalize lock file once verified that the versions are working fine in dev

NOTE: These files will be created or needs to be present where pipenv commands are being used

# Useful pipenv commands

| Command | Purpose |
| --- | --- |
| pipenv install | This will create a Pipfile (if it does't exist), will create a venv from this Pipfile and then create a Pipfile.lock. See [Use Case](#using-pipenv-in-a-fresh-project-for-the-first-time) for more details |
| pipenv --python 3.6 | This will create Pipfile and virtual environment with the specified version of Python. See [Use Case](#creating-a-new-virtual-environment-with-a-different-python-version-and-new-pipfile) for more details |
| pipenv lock | To create or update Pipfile.lock file with latest available versions of packages after testing in dev using Pipfile |
| pipenv install --ignore-pipfile | To create a venv from Pipfile.lock. See [Use Case](#creating-and-activating-virtual-environment-on-a-production-environment) for more details |
| pipenv install -r requirements.txt | To create Pipfile from an existing requirements.txt file and start a new virtual environment using pipenv. See [Use Case](#converting-existing-requirements-text-file-to-pipfile) for more details |
| pipenv install pytest --dev | To install only in dev and will not be used in production |
| pipenv install requests | To install packages in venv. This will act same as pipenv install command if executing for the first time on a folder. We can specify the version of the package if required. Example: **_pipenv install pyzipcode==3.0.1_** |
| pipenv shell | To activate the existing venv. pipenv will create a subshell for new environment |
| exit | to deactivate venv |
| pipenv run | To run any executable shell command. Example: **_pipenv run python script1.py_**. No sub-shell will be created here. Hence this is a more useful command to use the pipenv in deployment pipelines |
| pipenv --venv | To view the path of the venv |
| pipenv lock -r | To display packages and versions just like **_pip freeze_** |
| pipenv graph | To show the dependencies of packages |
| pipenv check | To view security vulnerabilities |
| pipenv uninstall requests | To uninstall a package |
| pipenv --rm | To remove existing venv |


# Use case scenarios
## Here are some of the common scenarios you may come across when you use pipenv:

#### Using pipenv in a fresh project for the first time:
```
# Create Pipfiles and virtual environments. Latest available Python version will be used here.
pipenv install
pipenv install <package name>
```

#### Converting existing requirements text file to Pipfile:
```
pipenv install -r requirements.txt 
pipenv install <package name>
```

#### Creating a new virtual environment with a different Python version and new Pipfile
```
pipenv --python 3.6
# Install the required packages
pipenv install <package name>
# Create Pipfile.lock file
pipenv lock
```

#### Changing the Python version of an existing project:
```
# Change the Python version under [requires] section in an existing Pipfile and save it
# Now remove the existing virtual environment
pipenv --rm
# Now execute below to create the virtual environment with new Python version specified
pipenv install
```

#### Creating and activating virtual environment on a production environment:
```
# Navigate to the folder where Pipfile.lock resides and then exesute the below
pipfile install --ignore-pipfile

# Activate the virtual environment in a subshell
pipfile shell
# Shell commands can be run now. To view the virtual environment's path, execute the following.
pipenv --venv
# Exit from virtual environment after it's use
exit

# OR execute the shell commands directly without creating a subshell and exiting as follows
pipenv run <command>
# NOTE: .env file can be used to set environment variables and this will get loaded when we run 'pipenv run python' command and these variables can be used using os.environ() function as normal

# And finally, remove virtual environment after it's use
pipenv --rm 
```

**Last modified: 2022-10-06**