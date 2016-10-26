# thymeflow-deploy

`thymeflow-deploy` makes it much easier to deploy a [Thymeflow](https://thymeflow.com) app on a server.

You can also use to setup a development environment locally or on a VM. Ubuntu 16.04 is recommended, albeit the setup scripts should work on any modern Linux distribution. 

The scripts were built using [ansible](https://www.ansible.com/).

## Requirements

 - Python 2.7, pip/virtualenv.
 - A user with sudo privileges for the target machine.

## Setup

 - Install python requirements via `pip install -r requirements`
 - Install external ansible roles via `ansible-galaxy install -r ansible-requirements.txt`
 - Create an `application.yml` file and define the following variables:

```
google_client_id: "XXXXXXXXXXXXXXXXXXX"
google_client_secret: "XXXXXXXXXXXXXXXXXXX"
microsoft_client_id: "XXXXXXXXXXXXXXXXXXX"
microsoft_client_secret: "XXXXXXXXXXXXXXXXXXX"
facebook_client_id: "XXXXXXXXXXXXXXXXXXX"
facebook_client_secret: "XXXXXXXXXXXXXXXXXXX"
google_geocoder_api_key: "XXXXXXXXXXXXXXXXXXX"
```
 - Configure a [list of host machines](http://docs.ansible.com/ansible/intro_inventory.html) to deploy to in a `hosts` file.
 - Launch the installation using `ansible-playbook -i hosts playbook.yml`.

## Notes

If deploying on `localhost`, make sure you run `ansible-playbook` with `--ask-sudo-pass` instead of running it with `sudo`. Otherwise the permissions will not be set correctly and your the application will not work properly.
The script assumes that the target machine has access rights to the Thymeflow repositories, especially using passwordless ssh authentication.
If deploying from your own repository, also add the following variables to your `application.yml`.
```
project_back_repository: CUSTOM_THYMEFLOW_BACK_REPOSITORY.git
project_front_repository: CUSTOM_THYMEFLOW_FRONT_REPOSITORY.git
```

The script will deploy the same release version of the frontend and backend repositories as this script's. This way, we can ensure that there exists a version of the deploy script for each backend/frontend release. If you know what you are doing, you may want to override this setting in your `application.yml`:
```
project_back_version: BRANCH_NAME/TAG/COMMIT_NUMBER
project_front_version: BRANCH_NAME/TAG/COMMIT_NUMBER
```

## Production setup

For a production deployment, also provide the following variables:

```
is_production: true
letsencrypt_email: contact@thymeflow.example.com
nginx_server_name: thymeflow.example.com
```

## License

MIT
