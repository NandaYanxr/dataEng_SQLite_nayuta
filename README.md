# Welcome to Nayuta data test

This repo contains the dbt project for Nayuta data test

## Table of Contents

**[Prerequisites](#Prerequisites)**<br>
**[Installation](#Installation)**<br>
**[Project Organisation and Naming Convention](#OrganiseDBT)**<br>


<a name="Prerequisites"/>

## Prerequisites

The following is required to run this data test:
- Install Python 3.12 ([https://www.python.org/downloads/](https://www.python.org/downloads/))
- Install Sqlite 3 **(Optional - Recommended)** ([https://www.prisma.io/dataguide/sqlite/setting-up-a-local-sqlite-database](https://www.prisma.io/dataguide/sqlite/setting-up-a-local-sqlite-database))
- Install Github Desktop **(Optional - Recommended)** ([https://desktop.github.com/download/](https://desktop.github.com/download/))

<a name="Installation"/>

## Installation

Please notice you might need to update some of the project paths below. E.g. From nayuta_data_test_sqlite to nayuta_data_test_sqlite_yourname

### 1a) Cloning dbt repository with Github desktop

a) Sign in to GitHub. Download and install GitHub Desktop before you start to clone.

b) Navigate to the data test repo in Github ([https://github.com/Nayuta-analytics/nayuta_data_test_sqlite](https://github.com/Nayuta-analytics/nayuta_data_test_sqlite)).

c) Above the list of files, click **"Code"**, then click **"Open with GitHub Desktop"**.

d) Choose a local directory where you want to clone the repository in Github Desktop, then click **"Clone"**.

(For more information, see [Cloning a repository from GitHub to GitHub Desktop](https://docs.github.com/en/desktop/adding-and-cloning-repositories/cloning-a-repository-from-github-to-github-desktop))

### 1b) Cloning dbt repository with SSH connection and terminal (Optional, if you are cloning the repo by Github Desktop you can skip this step. Go to step 2 to continue.)

Generating and saving SSH keys 🔑 

The Secure Shell Protocol (SSH) is a secure network protocol (usually on port 22) that allows communication to another network location or service. Authentication is typically handled by asymmetric (public / private key) cryptography.

In order to clone our repository, you will need to generate an SSH key for your device, and then save that SSH key to your Github account. This will then grant you access to the remote repo.

Please follow the step by step guide from Github to set up SSH into your Terminal (Mac) or Command Prompt (Window).

**Step 1** can be found at [generating a new SSH key and adding it to the ssh-agent](https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent#generating-a-new-ssh-key).

**Step 2** can be found at [adding a new SSH key to your GitHub account](https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account).

**Step 3** Cloning dbt repository from Github directly

Please go to your Terminal. Navigate to the folder in which you would like to store the whole project. Then run:

```bash
git clone git@github.com:Nayuta-analytics/nayuta_data_test_sqlite.git
```

The command will clone the repo and put all its contents into a folder called `nayuta_data_test_sqlite`.

### 2) Setting up virtual environment

a) Navigate into the project folder and set up the virtual environment by running

If Mac:
```bash
cd nayuta_data_test_sqlite
python3 -m venv .venv
```
If Windows:
```bash
cd nayuta_data_test_sqlite
py -m venv .venv
```

b) Activate the environment by running

If Mac:
```bash
source ./.venv/bin/activate
```

If Windows:
```bash
.\.venv\Scripts\activate
```

If you encounter difficulties activating the virtual environment in Windows, try the following:
```bash
Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy Unrestricted
```

c) Install dependencies from the `requirement.txt` by running
```bash
pip install --upgrade pip
pip install -r requirements.txt
```

d) Restart your terminal and repeat step 2b to activate your virtural environment before continue.


### 3) Setting up your dbt profile on your local machine

We will store your `profiles.yml` file in the same folder in the dbt project folder.

If Mac:
```bash
open profiles.yml
```

If Windows:
```bash
start notepad "profiles.yml"
```

Please check if the following credentials is included in the `profiles.yml` file.

```bash
nayuta_data_test_sqlite_profile:
  target: dev
  outputs:
    dev:
      type: sqlite
      threads: 1
      database: "database"
      schema: 'main'
      schemas_and_paths:
        main: 'sqlite/etl.db'
        raw: 'sqlite/raw.db'
      schema_directory: 'sqlite'
```

Alternatively, you can also store your local dbt profile in ~/.dbt/profiles.yml by default. You will need to populate this file with your development profile. You can do so by running:

```bash
open ~/.dbt/profiles.yml
```

For more details about configuring your dbt profile file, please refer to [this documentation](https://docs.getdbt.com/dbt-cli/configure-your-profile).

### 4) Installing all required dbt packages

Some packages are necessary for running this dbt project. You can install them by running

```bash
dbt deps
```
### 5) Running `dbt debug`

Run the following command and test if connection and setup is working

```bash
dbt debug
```

You will see:

```bash
Connection:
  database: database
  schema: main
  schemas_and_paths: {'main': 'sqlite/etl.db', 'raw': 'sqlite/raw.db'}
  schema_directory: sqlite
Registered adapter: sqlite=1.9.0
  Connection test: [OK connection ok]
```

Then it shows the setup is properly configured for the test.

### 6) Performing `dbt build`

Run the command:

```bash
dbt build
```

From the sqlite folder, you will find the `raw.db` database having all the raw data provided for this test.
And from the `etl.db` database after the above dbt run, you will find all the new models created.
You can also connect the two databases to your preferred database development environment to query the data for validation or visualization.


<a name="OrganiseDBT"/>

## Project Organisation and Naming Convention


All the models are located in the `models` folder, generally organised according to the following folder structure:

- `a_staging`: staging models, which draw directly from sources, performing functions such as cleaning, data type casting and renaming, particularly with no joins to other tables. Each source also has its own subfolder (named after the source name) under `stg`, whereas each stagining model therein is named `stg_sourcename_tablename`. 

- `b_intermediate`: intermediate models, which draw from staging models, applying complex business logics. All intermediate models are with the filename prefix `int_`. 

- `c_marts`: materialised models, which include fact and dimension tables that are ready for consumption.


