Installation
===================================

1. **Clone the framework**

``git clone https://github.com/th3-j0k3r/Patronus``

2. **Google SSO Configuration**

* You need to generate ``GOOGLE_ID`` and ``GOOGLE_SECRET`` tokens which you can create from your GCP console. 
* Google SSO will also require a callback url which requires a domain name, which you should have with you beforehand.
* In the google console, configure the ``Authorized redirect URIs`` field as: ``http://<domain-name>:3000/api/auth/callback/google``

These ``GOOGLE_ID`` and ``GOOGLE_SECRET`` configs should be added along with your custom domain name in the following two config files: ``.env`` in ``Patronus`` folder and ``.env.local`` in ``Patronus/frontend`` folder.

.. note::

   If you have any difficulties please refer: https://developers.google.com/identity/sign-in/web/sign-in

3. **Whitelisting email domain for SSO:**

Configure your organisation email domain from which you want to allow users to access the frontend dashboard in the file ``front-end/pages/api/auth/nextauth.ts:34``

4. **Create new SSH private and public key pair**

``cd Patronus/SSH_KEYS; ssh-keygen``

Input the file in which to save the key as ``./id_rsa`` and enter the pass-phrase of your choice.

5. **Add Public key to your organisation's bitbucket account**

* Once you are logged in to bitbuket, select the ``Bitbucket settings`` from the avatar in the lower left.
* Choose ``SSH keys`` under ``Security`` settings.
* Select ``Add key`` and add your public key with a label to it.

.. note::

   If you have any difficulties please refer https://confluence.atlassian.com/bitbucket/set-up-an-ssh-key-728138079.html


6. **Generate new app token in bitbucket**

* After logging into bitbucket, choose ``Your profile and settings``.
* Choose ``App passwords`` under ``Access Management``.
* Select ``Create app password`` and give a label to the created app password for future reference.
* Tick the read only access check box for the ``Account``, ``Team membership`` and ``Projects`` options for the app password permissions.

.. note::

   If you have any difficulties please refer https://confluence.atlassian.com/bitbucketserver/personal-access-tokens-939515499.html

7. **Generate a new Slack incoming webhook**

Generate a new incoming webhook for the channel you want to push the alerts from Patronus.

If you have any difficulties please refer https://slack.com/intl/en-in/help/articles/115005265063-Incoming-webhooks-for-Slack

8. **Update the config file of Patronus**

Open the config file ``Patronus/config/config.py`` and set the values for the following configuration keys:

.. list-table:: 
   :widths: 25 50 25
   :header-rows: 1

   * - Field
     - Description
     - Mandatory
   * - SSH_PUB_KEY
     - Path to the SSH Public Key added to your bitbucket org
     - ✅ 
   * - SSH_PRI_KEY
     - Path to the SSH Public Key added to your bitbucket org
     - ✅  
   * - SSH_PASSWORD
     - Password for the SSH Key
     - ✅  
   * - BITBUCKET_USERNAME
     - Username for the bitbucket account
     - ✅
   * - BITBUCKET_APP_PASSWORD
     - App password for bitbucket
     - ✅
   * - DB_HOST
     - Database host name
     - ✅
   * - DB_DATABASE
     - Database Name
     - ✅
   * - DB_USER
     - Database Username
     - ✅
   * - DB_PASSWORD
     - Database Password
     - ✅
   * - PATRONUS_SLACK_WEB_HOOK_URL
     - Slack incoming webhook URL
     - ✅
   * - PATRONUS_DOWNLOAD_LOCATION
     - Download location to clone all the repos from the bitbucket
     - ✅
   * - JIRA_URL
     - Jira Project URL
     - ✅
   * - JIRA_TOKEN
     - Jira Project name
     - ✅
   * - PATRONUS_CVSS_SCORE_FILTER
     - Minimum CVSS score
     - ✅

9. **Build the docker image and deploy Patronus**

Once the neccessory configiration are set, we can deploy patronus with the command from the Patronus root directory:

``docker-compose up --build -d``

Now the Patronus Framework is up and running and the pre-schduled scans will be run in the background.

The pre-scheduled scan timings can be changed by modifying the cron schedule expression in: ``Patronus/cron.sh:7``

10. **Ports**

Once the container is deployed, by default: 

.. list-table::
   :widths: 25 25
   :header-rows: 1

   * - Functionality
     - Port
   * - Web Dashboard
     - 3000
   * - API Server
     - 8080

These ports are default configs and can be changed in the docker-compose file at ``Patronus/docker-compose.yml``
