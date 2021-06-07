# GeoCEM

GeoNode template project. Generates a django project with GeoNode support.
## Description
This repository contains a Django application based on Geonode project that provide metadata and geospatial data from the Metropolitan Study Center (CEM) of São Paulo.

This information is shared with everybody.
## Live Demo
The demo version it is at the CEM website of Interactive Systems, and you can see [here](http://200.144.244.238)
## Development setup
This section covers the necessary steps to get the application running on a
local development machine. It assumes you're using Ubuntu, so you may need
to adapt some of the commands if this is not true.
### Dependencies
* Docker
* Docker-compose

### Dependencies installation and setup

#### Docker
##### Installation
  ```bash
    sudo add-apt-repository universe
    sudo apt-get update -y
    sudo apt-get install -y git-core git-buildpackage debhelper devscripts
    sudo apt-get install -y apt-transport-https ca-certificates curl gnupg-agent software-properties-common

    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

    sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

    sudo apt-get update -y
    sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-compose
    sudo apt autoremove --purge

    sudo adduser geonode
    sudo usermod -aG sudo geonode
    sudo usermod -aG docker geonode
    su geonode
  ```
#### Deploy an instance of a geonode-project Django template 3.1.x with Docker on localhost
Prepare the environment
  ```bash
  sudo mkdir -p /opt/geonode_custom/
  sudo usermod -a -G www-data geonode
  sudo chown -Rf geonode:www-data /opt/geonode_custom/
  sudo chmod -Rf 775 /opt/geonode_custom/
  ```
Clone the source code
  ```bash
  cd /opt/geonode_custom/
  git clone https://github.com/GeoNode/geonode-project.git -b 3.1.x
  ```
Make an instance out of the Django Template
** Note: ** We will call our instance geo_cem. You can change the name at your convenience.

Install virtualenv and virtualenvwrapper, edit .bashrc file:

   ```bash
   #Virtualenvwrapper settings:
   export VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3
   export WORKON_HOME=$HOME/.virtualenvs
   export VIRTUALENVWRAPPER_VIRTUALENV=/home/geonode/.local/bin/virtualenv
   source ~/.local/bin/virtualenvwrapper.sh
   ```

   ```bash
   source ~/.local/bin/virtualenvwrapper.sh
   mkvirtualenv --python=/usr/bin/python3 geo_cem

   Alterantively you can also create the virtual env like below
   python3 -m venv /home/geonode/dev/.venvs/geo_cem
   source /home/geonode/dev/.venvs/geo_cem/bin/activate

   pip install Django==2.2.12

   django-admin startproject --template=./geonode-project -e py,sh,md,rst,json,yml,ini,env,sample,properties -n monitoring-cron -n Dockerfile geo_cem
   cd /opt/geonode_custom/geo_cem
   ```
Modify the code and the templates and rebuild the Docker Containers
   ```bash
   docker-compose -f docker-compose.yml build --no-cache
   ```
Finally, run the containers
   ```bash
   docker-compose -f docker-compose.yml up -d
   ```
[Installation reference](https://docs.geonode.org/en/master/install/advanced/project/index.html#deploy-an-instance-of-a-geonode-project-django-template-3-2-0-with-docker-on-localhost)

#### Recommended: Track your changes

Step 1. Install Git (for Linux, Mac or Windows).

Step 2. Init git locally and do the first commit:

```bash
git init
git add *
git commit -m "Initial Commit"
git branch -M main
git remote add origin https://github.com/repo_demo/demo.git
git push -u origin main
```

Step 3. Set up a free account on github or bitbucket and make a copy of the repo there.



### Project setup

Update the templates or the Django models. Once in the bash you can edit the templates or the Django models/classes. From here you can run any standard Django management command.

Whenever you change a template/CSS/Javascript remember to run later:

```bash
python manage.py collectstatic
```
in order to update the files into the statics Docker volume.

** Warning: ** This is an external volume, and a simple restart won’t update it. You have to be careful and keep it aligned with your changes.

Whenever you need to change some settings or environment variable, the easiest thing to do is to:

```bash
# Stop the container
docker-compose stop

# Restart the container in Daemon mode
docker-compose -f docker-compose.yml -f docker-compose.override.<whatever>.yml up -d
```

Whenever you change the model, remember to run later in the container via bash:

```bash
python manage.py makemigrations
python manage.py migrate
```

#### Backup and Restore from Docker Images

##### Run a Backup

```bash
SOURCE_URL=$SOURCE_URL TARGET_URL=$TARGET_URL ./{{project_name}}/br/backup.sh $BKP_FOLDER_NAME
```

- BKP_FOLDER_NAME:
  Default value = backup_restore
  Shared Backup Folder name.
  The scripts assume it is located on "root" e.g.: /$BKP_FOLDER_NAME/

- SOURCE_URL:
  Source Server URL, the one generating the "backup" file.

- TARGET_URL:
  Target Server URL, the one which must be synched.

e.g.:

```bash
docker exec -it django4{{project_name}} sh -c 'SOURCE_URL=$SOURCE_URL TARGET_URL=$TARGET_URL ./{{project_name}}/br/backup.sh $BKP_FOLDER_NAME'
```

Copy files from container:

```bash
sudo docker cp django4geo_cem:"/backup_restore" "/opt/"
```
Copy files to container:

```bash
sudo docker cp "/backup_folder/*" django4geo_cem:"/backup_restore/"
```
##### Run a Restore

```bash
SOURCE_URL=$SOURCE_URL TARGET_URL=$TARGET_URL ./{{project_name}}/br/restore.sh $BKP_FOLDER_NAME
```

- BKP_FOLDER_NAME:
  Default value = backup_restore
  Shared Backup Folder name.
  The scripts assume it is located on "root" e.g.: /$BKP_FOLDER_NAME/

- SOURCE_URL:
  Source Server URL, the one generating the "backup" file.

- TARGET_URL:
  Target Server URL, the one which must be synched.

e.g.:

```bash
docker exec -it django4{{project_name}} sh -c 'SOURCE_URL=$SOURCE_URL TARGET_URL=$TARGET_URL ./{{project_name}}/br/restore.sh $BKP_FOLDER_NAME'
```

## Testing

## Project Phases
During the implementation of the project will be constantly updated the following features:
* Design the website (style)
* Data Migration
* User and development documentation
* Define the style of the layers maps

### Phase 1 (may 17th)
* ** Goals: ** lay the groundwork for the next phases. The goal of this phase was
mainly to familiarize with the tools that was chosen to adopt in the
project, and test drive each one.
* see all issues that were planned (and delivered) for this phase [here]().
### Phase 2 (june 7th)
* ** Goals: **
  * Design the basic style for the website.
  * Migrate the available data.
  * Configure users accounts
* see all issues that were planned (and delivered) for this phase [here]().
### Phase 3 (july 5th)
* ** Goals: **
  * Link and fill the information from GeoCEM to he Dataverse
  * Add feature module for fast migration data
  * Configure the advance search
* see all issues that were planned (and delivered) for this phase [here]().
### Phase 4 (august 2nd)
* ** Goals: ** Publish the project
* see all issues that were planned (and delivered) for this phase [here]().
## Wiki
   - [Wiki links](https://github.com/hansbecc/geo_cem/wiki) 
