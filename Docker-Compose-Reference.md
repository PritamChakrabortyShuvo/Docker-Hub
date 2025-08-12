<!-- @format -->

```yaml
version: '3.8'

services:
  db:
    image: mysql:8.0
    container_name: mysql_container
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: root_password
      MYSQL_DATABASE: example_db
      MYSQL_USER: example_user
      MYSQL_PASSWORD: user_password
    ports:
      - "3306:3306"
    volumes:
      - db_data:/var/lib/mysql

  wordpress:
    depends_on:
      - db
    image: wordpress:latest
    ports:
      - "80:80"
    restart: always
    environment:
      WORDPRESS_DB_HOST: mysql_container
      WORDPRESS_DB_USER: root
      WORDPRESS_DB_PASSWORD: root_password
      WORDPRESS_DB_NAME: example_db
    volumes:
      - wordpress:/var/www/html
volumes:
  db_data:
  wordpress:
```

# Explanation of Docker Compose YAML for WordPress and MySQL

This **Docker Compose YAML file** sets up a simple way to run a **WordPress website** with a **MySQL database** using two containers. Below is a beginner-friendly explanation of what the file does and how it works.

## What It Does

The file tells **Docker** (a tool for running apps in containers) to create:

1. A **MySQL database** container to store the website’s data (like blog posts, users, etc.).

2. A **WordPress** container to run the website itself.

The containers work together, and the file ensures they’re set up correctly and can communicate.

## Breaking Down the YAML File

The file has three main parts: **version**, **services**, and **volumes**.

### 1. Version

- **`version: '3.8'`**: Specifies the Docker Compose file format (like a template version). Version 3.8 works with modern Docker features.

### 2. Services (The Containers)

The `services` section defines the two containers: `db` (MySQL) and `wordpress` (WordPress).

#### Service 1: `db` (MySQL Database)

This sets up the database for WordPress to store data like posts and user info.

- **Image**: Uses the official **`mysql:8.0`** image (a pre-built MySQL package).

- **Container Name**: Names the container **`mysql_container`** for easy reference.

- **Restart**: Set to **`always`**, so the container restarts automatically if it stops (e.g., after a computer reboot).

- **Environment Variables** (settings for MySQL):

  - **`MYSQL_ROOT_PASSWORD: root_password`**: Sets the admin password for the database.

  - **`MYSQL_DATABASE: example_db`**: Creates a database called **`example_db`**.

  - **`MYSQL_USER: example_user`**: Creates a user named **`example_user`**.

  - **`MYSQL_PASSWORD: user_password`**: Sets the password for **`example_user`**.

  - **Where to Find These Variables:** These environment variables are listed in the **MySQL image documentation on Docker Hub**. Search for the "**Environment Variables**" section to see all support.

- **Ports**: **`3306:3306`** lets you access the database from your computer on port 3306 (useful for tools like MySQL Workbench). Format is **`host_port:container_port`**.
- **Volumes**: Saves database files in a “folder” called **`db_data`** so data isn’t lost if the container stops.

#### Service 2: **`wordpress`** (WordPress Website)

This sets up the WordPress website, which connects to the MySQL database.

- **Depends On**: **`db`** ensures the database starts before WordPress, since WordPress needs the database.
- **Image**: Uses the latest **`wordpress`** image (the website software).

- **Ports**: **`80:80`** makes the website available at **`http://localhost`** in your browser (port 80 is standard for websites).

- **Restart**: Set to **`always`** to restart if the container stops.

- **Environment Variables** (settings to connect WordPress to the database):

  - **`WORDPRESS_DB_HOST: mysql_container`**: Tells WordPress to connect to the database using the name **`mysql_container`**.

  - **`WORDPRESS_DB_USER: root`**: Uses the MySQL **`root`** user to log in (not the safest, but works for testing).

  - **`WORDPRESS_DB_PASSWORD: root_password`**: The password for the **`root`** user.

  - **`WORDPRESS_DB_NAME: example_db`**: Tells WordPress to use the **`example_db`** database.

  - **Where to Find These Variables**: These environment variables are documented in the WordPress image page on Docker Hub. Look under the "Environment Variables" section for details on **`WORDPRESS_DB_HOST, WORDPRESS_DB_USER, WORDPRESS_DB_PASSWORD, WORDPRESS_DB_NAME`** & other options.

- **Volumes**: Saves WordPress files (like themes, plugins, and uploads) in a “folder” called **`wordpress`** so they don’t disappear if the container stops.

### 3. Volumes

- **`db_data:`**: A special “folder” to store MySQL database files, ensuring data persists even if the container is deleted.

- **`wordpress:`**: A “folder” to store WordPress files (e.g., themes, plugins, uploads) for persistence.

## How It All Works

- **Networking**: Docker creates a network so WordPress can talk to MySQL using the name **`mysql_container**`.

- **Starting Up**: The database starts first (due to **`depends_on`**), then WordPress. MySQL might take a moment to be fully ready.

- **Accessing the Site**: Visit **`http://localhost`** in your browser to set up WordPress. The database is ready with the provided settings.

- **Data Safety**: The **`db_data`** and **`wordpress**` volumes keep your data safe, even if you stop or delete the containers.

## How to Run It

1. Save the YAML file as **`docker-compose.yml`**.

2. Ensure **Docker** and **Docker Compose** are installed.

3. In a terminal, go to the folder with the file and run:

```bash
   docker-compose up -d
```

(**`-d`** runs it in the background.) 4. Open **`http://localhost`** in your browser to set up WordPress. 5. To stop the containers:

```bash
docker-compose down
```

## Things to Know

- **Security**: Using the **`root`** user for WordPress isn’t very secure. For a real site, use **`example_user`** instead. Also, don’t hardcode passwords in the file—use a **`.env`** file.

- **Port 3306**: The database is accessible from your computer, but you may not need this for a live site (remove **`3306:3306`** for safety).

- **Data Persistence**: The volumes ensure your website and database data are saved, even if containers stop.

- **Finding Environment Variables**: The environment variables for both MySQL and WordPress are documented on their respective Docker Hub pages:

  - **`MySQL Docker Hub for MYSQL_*`** variables.

  - **`WordPress Docker Hub for WORDPRESS_*`** variables. These pages list all supported variables and their purposes, so you can customize the setup if needed.

## Summary

This YAML file is like a recipe for Docker to:

1. Set up a MySQL database with a user, password, and database name.

2. Run a WordPress website that connects to that database.

3. Save all data (database and website files) so nothing is lost.

4. Make the website available at **`http://localhost`**.

It’s a quick way to get a WordPress site running without manually installing MySQL or WordPress.
