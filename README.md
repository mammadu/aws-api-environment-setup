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
  - make sure your security group has inbound rules allowing http (port 80), https (port 443) and ssh (port 22) for source 0.0.0.0/0
- AWS elastic ip address for your AWS EC2 instance(optional, it makes testing out different instances simpler)
- A domain (optional)

### Installation

1. clone this repo to the EC2 instance: `git clone https://github.com/mammadu/aws-api-environment-setup.git`
2. navigate to the repo: `cd aws-api-environment-setup/`
3. Install Ansible on ubuntu EC2 instance using either install script in repo `install-ansible.sh` or by following [this ansible guide](https://docs.ansible.com/projects/ansible/latest/installation_guide/installation_distros.html#installing-ansible-on-ubuntu)
4. navigate to the ansible folder in the repo: `cd ansible`
5. copy `variables.yaml.template` to file `variables.yaml`: `cp variables.yaml.template variables.yaml`
6. Modify `variables.yaml` to replace the domain and email variables along with any other pertinent variables
7. use ansible to configure the server: `ansible-playbook -i inventory.ini playbook.yaml`
8. install gunicorn and start the application server
    1. if your using django, you can install gunicorn in the same virtual environment as django. Most django applications will have a `wsgi.py` file you can use to start the gunicorn server. For example, assuming your django project name is `mysite` and you are in root of your django repo, using `gunicorn mysite.wsgi` will start the gunicorn server

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- USAGE EXAMPLES -->
## Usage

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- ROADMAP -->
## Roadmap

- [x] Remove things from readme template that I do need
- [x] Document server setup steps
  - [x] Document installing ansible on server
- [x] Document installation of required applications
  - [ ] django
  - [x] gunicorn
  - [x] nginx/apache
  - [ ] postgresql
  - [ ] docker if I end up containerizing this
- [x] document how to run the api endpoint
  - [ ] include steps to setup env file if necessary
  - [ ] include steps on how to access the api endpoints both locally (on AWS) and remotely

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- LICENSE -->
## License

Distributed under the project_license. See `LICENSE.txt` for more information.

<p align="right">(<a href="#readme-top">back to top</a>)</p>
