# wcmf-workspace

A development environment for [wCMF](https://github.com/iherwig/wcmf) projects including:

* Customized [Eclipse IDE](https://eclipse.org/ide/)
* [Docker](https://www.docker.com/) containers to set up [Apache](https://httpd.apache.org/) HTTP Server, [MySQL](https://www.mysql.com/) Database Server and PHP

# Installation

* Clone this respository into `<base_dir>`

  ```
  cd <base_dir>
  git clone https://github.com/iherwig/wcmf-workspace.git
  ```
  
  This will create the following directories and files:
  
  ```
  <base_dir>
  - wcmf-workspace
    - docker
      - docker
        - webserver
          - config
            - php.ini
          - Dockerfile
      - .env
      - docker-compose.yml
    - eclipse
      - wcmf-eclipse.setup
    - www
      - index.php
  ```
  
* **Eclipse installation**
  * Download and install the [Eclipse Installer](https://projects.eclipse.org/projects/tools.oomph/downloads)
  * Launch the installer and switch to *Advanced Mode* using the button in the top right corner
  * Press the *+* sign to open the *Add user products* dialog
  * *Browse File System...* to `<base_dir>/wcmf-workspace/eclipse/wcmf-eclipse.setup` and press *OK*
  * Switch to *Simple Mode* using the tools button in the lower left corner
  * Choose the product *Eclipse IDE for wCMF Developers* and press *Install*
  * Press *Launch* and select a workspace directory
  * **Close Eclipse once before using it** to make sure that all preferences are updated
  * For more information about using Eclipse with PHP visit the [PDT](https://eclipse.org/pdt/) website
  
* **Docker setup**
  * In order to enable debugging support you need to get your local IP address (e.g. `ifconfig`)
  * Open `<base_dir>/wcmf-workspace/docker/.env` in an editor and enter the IP address
    ```
    HOST_IP=<IP address>
    ```
  * Start the docker containers:
    ```
    cd <base_dir>/wcmf-workspace/docker
    docker-compose up -d
    ```
  * Test if everything is working by opening `http://localhost/index.php` in a browser
  * **NOTES**
    * The web server root directory is mapped to `<base_dir>/wcmf-workspace/www`
    * Connections to the MySQL server must use `mysql` as server name
  
