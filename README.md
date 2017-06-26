# wcmf-workspace

A development environment for [wCMF](https://github.com/iherwig/wcmf) projects consisting of:

* A customized [Eclipse IDE](https://eclipse.org/ide/) with PHP and modeling support
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
  * For more information about using Eclipse with PHP visit the [PDT](https://eclipse.org/pdt/) website and 
    to learn more about the modeling tools visit the [Eclipse Papyrus](https://eclipse.org/papyrus/) website
  
* **Docker setup**
  * Download and install [Docker](https://www.docker.com/)
  * To enable debugging support you need to get your local IP address (e.g. `ifconfig`)
  * Open `<base_dir>/wcmf-workspace/docker/.env` in an editor and enter the IP address
  
    ```
    docker_host_ip=<IP address>
    ```

  * Adjust paths, ports and additional configuration in `<base_dir>/wcmf-workspace/docker/.env` according 
    to your environment (see [default values](https://github.com/iherwig/wcmf-workspace/blob/master/docker/.env))
  * **Start the docker containers**
  
    ```
    cd <base_dir>/wcmf-workspace/docker
    docker-compose up -d
    ```
    
  * **Test the installation** by opening the URL 
  
    ```
    http://localhost/index.php
    ```
    
    or 
    
    ```
    http://localhost:<http_port>/index.php
    ```

    if you changed the value of the `http_port` variable
    
## Notes

  * When connecting to the **MySQL server** from inside the webserver's docker container (e.g. from a PHP script)
    you have to use the **host name** `mysql`
  * Use the following command to open a **shell** in the webserver's docker container:
  
    ```
    docker exec -it docker_webserver_1 bash
    ```
