## Running a Dockerized WordPress on your local machine
Hello, running this is straightforward using Docker Compose. Here's a quick guide to get you up and running:

✅ Prerequisites:
Docker installed
Docker Compose installed
📁 Step 1: Create a Project Folder
mkdir wordpress-docker
cd wordpress-docker
📝 Step 2: Create a docker-compose.yml File
Create a file called docker-compose.yml inside your folder with the following content:

version: '3.8'

services:
  wordpress:
    image: wordpress:latest
    ports:
      - "8000:80"
    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: wordpress
      WORDPRESS_DB_NAME: wordpress
    volumes:
      - wordpress_data:/var/www/html
    depends_on:
      - db

  db:
    image: mysql:5.7
    volumes:
      - db_data:/var/lib/mysql
    restart: always
    environment:
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpress
      MYSQL_ROOT_PASSWORD: rootpassword

volumes:
  wordpress_data:
  db_data:
🚀 Step 3: Run Docker Compose
In your terminal, from inside the wordpress-docker directory, run:

docker-compose up -d
This will download the WordPress and MySQL images, create containers, and run your WordPress site.

🌐 Step 4: Access WordPress
Open your browser and go to:

http://localhost:8000
You should see the WordPress installation page. Complete the setup as you wish.

📌 Useful Commands
Stop the containers:

docker-compose down

Stop and remove volumes:

docker-compose down -v

View logs:

docker-compose logs -f


🔒Something you should know: Always Keep Tokens Secure
Instead of hardcoding the token (especially for production), it's better to load it from an .env file like this:

In your docker-compose.yml:

environment:
  - NGROK_AUTHTOKEN=${NGROK_AUTHTOKEN}
    
In a .env file (same folder):

NGROK_AUTHTOKEN=2**10Lr********ojAxiBmEq***uwcMQ***X1c*AaB*****.

That way you avoid accidentally sharing it when you push to GitHub.
