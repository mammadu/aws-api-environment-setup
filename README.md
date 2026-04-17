<!-- Improved compatibility of back to top link: See: https://github.com/othneildrew/Best-README-Template/pull/73 -->
<a id="readme-top"></a>
<!--
*** Thanks for checking out the Best-README-Template. If you have a suggestion
*** that would make this better, please fork the repo and create a pull request
*** or simply open an issue with the tag "enhancement".
*** Don't forget to give the project a star!
*** Thanks again! Now go create something AMAZING! :D
-->
<div align="center">

<h3 align="center">aws-api-environment-setup</h3>

  <p align="center">
    This repo is for defining my aws environments in code and making setting up the environment repeatable. I am primarily using this for demo api interviews
  </p>
</div>

<!-- ABOUT THE PROJECT -->
## About The Project

This repo configures an aws ec2 instance to be setup as an api server. It primarily uses ansible to configure the environment. The file `ansible/variables.yaml` is used to update project specific variables during configuration

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- GETTING STARTED -->
## Getting Started

### Prerequisites

- An AWS EC2 ubuntu instance
  - Make sure your security group has inbound rules allowing http (port 80), https (port 443) and ssh (port 22) for source 0.0.0.0/0
- AWS elastic ip address for your AWS EC2 instance(optional, it makes testing out different instances simpler)
- A domain (optional)

### Installation

1. Clone this repo to the EC2 instance: `git clone https://github.com/mammadu/aws-api-environment-setup.git`
2. Navigate to the repo: `cd aws-api-environment-setup/`
3. Install Ansible on ubuntu EC2 instance using either install script in repo `install-ansible.sh` or by following [this ansible guide](https://docs.ansible.com/projects/ansible/latest/installation_guide/installation_distros.html#installing-ansible-on-ubuntu)
4. Navigate to the ansible folder in the repo: `cd ansible`
5. Copy `variables.yaml.template` to file `variables.yaml`: `cp variables.yaml.template variables.yaml`
6. Modify `variables.yaml` to replace the domain and email variables along with any other pertinent variables
7. Use ansible to configure the server: `ansible-playbook -v -i inventory.ini playbook.yaml`

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- USAGE EXAMPLES -->
## Usage

### Running a test server

You should now have a django-ninja project in `~/myproject` (replace `myproject` with your actual project name)
From your project directory, activate your virtual environment like so: `source env/bin/activate`
To start the test server: `python manage.py runserver`
To access your api, you can use curl the specific endpoints: `curl your_domain.com/api/hello` (`your_domain.com` could be your actual domain or localhost depending on your settings)

From here you can start building your api

At a basic level you can instantiate a `NinjaAPI` object in urls.py, but best practice is:

- To make an app for the api using `python manage.py startapp APPNAME` from inside the virtual environment.
- Make a file called `APPNAME_api.py`. Create a `NinjaAPI` object in this file
- Add the apps file to your `INSTALLED_APPS` in the settings.py. e.g. `APPNAME.apps.AppnameConfig`
- In your urlconf add `from APPNAME.api import api` to your imports and add `path('api/', api.urls)` in the url_patterns in your urlconf
- If necessary, add models to `APPNAME/models.py`
- You can then start defining schemas and endpoints in your `APPNAME/api.py` file

### Running a production server

For production, make sure to update the file `~/myproject/settings.py`

- set `DEBUG = False`
- update the value of `SECRET_KEY`

To start a production server you can run `python -m gunicorn -w $(lscpu -e=CPU | wc -l) myproject.asgi -k uvicorn_worker.UvicornWorker`.

- `$(lscpu -e=CPU | wc -l` should get you an optimal amount of workers

## Testing

For testing, you will have to grant your database user, (i.e.{{ db_user }} in the variables.yaml file) createdb privileges. To do this

1. enter psql: `sudo -u postgres psql`
2. grant your user privileges: `ALTER USER db_user CREATEDB;`

Otherwise, define your tests in `APPNAME/tests.py`. Run your tests with `python manage.py tests`. See https://docs.djangoproject.com/en/6.0/intro/tutorial05/ for more details

<p align="right">(<a href="#readme-top">back to top</a>)</p>

## Troubleshooting

Certbot modifies the nginx configuration so that http requests are redirected to https (using HTTP response 301). This can cause POSTS to become GETS so it may be beneficial to modify the nginx configuration to change the 301 repsone to a 308 response - see https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Status/301

Certbot will also rate limit you if you get too many certificates in a short period of time, see https://letsencrypt.org/docs/staging-environment/

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- ROADMAP -->
## Roadmap

- [x] Remove things from readme template that I do need
- [x] Document server setup steps
  - [x] Document installing ansible on server
- [x] Document installation of required applications
  - [x] django
  - [x] gunicorn
  - [x] nginx/apache
  - [x] postgresql
  - [ ] docker if I end up containerizing this
- [x] document how to run the api endpoint
  - [x] include steps to setup env file if necessary
  - [x] include steps on how to access the api endpoints both locally (on AWS) and remotely
- [ ] automate granting your user privileges: `ALTER USER db_user CREATEDB;`
- [ ] split ansible tasks into roles
- [ ] automate setup for api file structure as described in [Running a test server](#running-a-test-server)

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- LICENSE -->
## License

Distributed under the project_license. See `LICENSE.txt` for more information.

<p align="right">(<a href="#readme-top">back to top</a>)</p>
