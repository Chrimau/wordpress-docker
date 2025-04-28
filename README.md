## Setting up a Dockerized WordPress + Ngrok on your local machine
Hello, running a dockerized wordpress using Ngrok is straightforward on your CLI. view mine at https://4463-2a06-93c2-7f-6-398e-9c59-7d22-15e8.ngrok-free.app  (note: you may see an error message whenever its offline because its running on my local machine)
Here's a quick guide to get you up and running:

## Prerequisites:
Docker desktop installed
Ngrok Account
your local terminal

## 📁 Step 1: Create a Project Folder
mkdir wordpress-docker
cd wordpress-docker

## 📝 Step 2: Create a docker-compose.yml File
Create a file called docker-compose.yml inside your folder with the following content:
<pre>
cat << 'EOF' > docker-compose.yml
version: '3.8'

services:
  wordpress:
    image: wordpress:latest
    restart: always
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

 ngrok:
    image: wernight/ngrok
    restart: always
    depends_on:
      - wordpress
    command: ["ngrok", "http", "--log=stdout", "wordpress:80"]
    environment:
      NGROK_AUTHTOKEN: ${NGROK_TOKEN:-your_token_here} # Get from ngrok dashboard
    ports:
      - "4040:4040"
      
volumes:
  wordpress_data:
  db_data:
  EOF </pre>
  
## 🚀 Step 3: Run Docker Compose
In your terminal, from inside the wordpress-docker directory, run:

docker-compose up -d
This will download the WordPress and MySQL images, create containers, and run your WordPress site.

## 🌐 Step 4: Access WordPress
Open your browser and go to:

http://localhost:8000
You should see the WordPress installation page. Complete the setup as you wish.

## 📌 Useful Commands
Stop the containers:
<pre> docker-compose down </pre>

Stop and remove volumes:
<pre> docker-compose down -v </pre>

View logs:
<pre> docker-compose logs -f </pre>


🔒Things you should know: Always Keep Tokens Secure
Instead of hardcoding the token (especially for production), it's better to load it from an .env file or ngrok section in the yaml file like this:

In your docker-compose.yml:

  <pre> environment:
    NGROK_AUTHTOKEN=${NGROK_AUTHTOKEN} </pre>

Replace ${NGROK_AUTHTOKEN} with the actual ngrok token,here is a sample :

<pre> NGROK_AUTHTOKEN=2**10Lr********ojAxiBmEq***uwcMQ***X1c*AaB**** </pre>

(This command protects you from accidentally sharing the token which is a secret when you push to GitHub).

## WARNING: Production environments require proper secret management (e.g., Docker Secrets, HashiCorp Vault).

Have fun! Let me know if this process worked for you. You can also play around it further and try to host it on AWS or GCP. View my documentation on how to do just that here https://github.com/Chrimau/gcp-wordpress 
