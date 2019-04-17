# Ansible for DFIRTrack

## About

The following [Ansible](https://docs.ansible.com/ansible/latest/index.html) playbook and role was made for installing [DFIRTrack](https://github.com/stuhli/dfirtrack) on a Linux server. The following distributions are currently tested:

* Debian Stretch
* Ubuntu 16.04 LTS
* Ubuntu 18.04 LTS

Beside the installation of DFIRTrack several tasks are executed alongside. It is planned to build another role that only takes the minimum needed steps for successful plain installation.

## Usage

### Pre-execution steps

#### Local host preparation

You need Ansible installed on your local host. If you want to install the ['develop' branch of DFIRTrack](https://github.com/stuhli/dfirtrack/tree/develop) (see "Execution of Ansible playbook" below), you need Ansible 2.7.

#### Debian Stretch preparation

Install `sudo` (`# apt install -y sudo`) and add your favorite deployment user to `sudo` group (`# usermod -aG sudo <USER>`).

**Attention:**

The used default deployment user is called "forensics". If you wish to change, edit the variable `ansible_ssh_user` in `group_vars/all`.

#### Ubuntu 16.04 LTS / 18.04 LTS preparation

Install `python` (e. g. `$ sudo apt install -y python`).

#### Fast testing

For fast testing the playbook may be executed with the default values. It was created with every option predefined. So for first testing you may skip this section.

**Attention:**

The used default deployment user is called "forensics". If you wish to change, edit the variable `ansible_ssh_user` in `group_vars/all`.

#### Production usage

For production usage (not publicly available!!!) it is recommended to think about the following values before executing:

* django secret key (there are many instructions in the wild how to generate it properly),
* path for the project (`<PROJECT_DIR>`),
* path for virtual environment (`<VENV_DIR>`),
* path for logging,
* path and project name (needed separately) for markdown documentation,
* password for PostgreSQL database (default and dedicated user),
* path for database backup,
* URL for reaching the web interface (`<FQDN>`),
* service name for nginx logging,
* path for ngingx webserver files,
* organization name and unit for self signed SSL certificates,
* stuff for GIRAF API (project is not public yet).

### Execution of Ansible playbook

* clone this repository to desired location: `git clone https://github.com/stuhli/dfirtrack_ansible <LOCATION> && cd <LOCATION>`,
* add destination host to `hosts` file like it is addressed by your ssh config,
* if you want to install the ['develop' branch of DFIRTrack](https://github.com/stuhli/dfirtrack/tree/develop), checkout this repo to 'develop' too: `git checkout develop`,
* execute ansible: `ansible-playbook -i hosts [-k] -K dfirtrack.yml`,
* confirm or change the default values while executing.

### Post-execution steps

* login to destination host,
* source the virtual environment: `source <VENV_DIR>/bin/activate`,
* change to project folder: `cd <PROJECT_DIR>`,
* create superuser: `python3 manage.py createsuperuser`,
* login to web interface (`https://<FQDN>`),
* additional administration is possible due to admin UI (`https://<FQDN>/admin`).

## Background information

### Tasks

The following tasks are executed:

* clone [DFIRTrack repository](https://github.com/stuhli/dfirtrack) to a desired destination,
* install and prepare `django` project,
* copy and customize main project configuration file `settings.py`,
* prepare folders for logging and markdown documentation (in `mkdocs` style),
* configure `PostgreSQL` database (including users and passwords),
* prepare cronjob for database backup,
* install and configure `nginx` reverse proxy server including self signed SSL certificates,
* install WSGI server `gunicorn` as service,
* install `django-q` task queue as service,
* configure firewall `ufw`,
* prepare API call for `GIRAF` (project is not public yet),
* the dependencies are installed as described below and a virtual environment is established.

### Dependencies

The following dependencies are installed (partly in a virtual environment (\*)). These are also needed for minimal installation:

* `django`\* (2.0),
* `django_q`\*,
* `djangorestframework`\*,
* `gunicorn`\*,
* `postgresql` (9.5),
* `psycopg2-binary`\*,
* `python3-pip`,
* `PyYAML`\*,
* `requests`\*,
* `virtualenv`,
* `xlwt`\*.

### Additional software

The following additional software is installed:

* `mkdocs`,
* `nginx`,
* `python-psycopg2`,
* `ufw`.
