# ACIT 3475 - Project Part 2 - Zoya Wajzer

# Part 1: OAuth Integration and Portfolio Extension

This part of the project focuses on integrating **OAuth 2.0** for secure login and adding a **GitHub Contributions Widget** to enhance the portfolio. It consists of two primary steps:

- **Step 1**: OAuth Integration for secure login and restricting access to the "Projects" section.
- **Step 2**: Adding a GitHub Contributions section using a widget.

---

## Step 1: OAuth Integration (Google OAuth 2.0)



### Instructions:

1. **Set Up OAuth in Google Cloud Console:**
   - Go to the [Google Cloud Console](https://console.cloud.google.com/).
   - Create a new project or select an existing project.
   - In the navigation panel, go to **APIs & Services** → **Credentials**.
   - Click on **Create Credentials** → **OAuth 2.0 Client IDs**.
   - Set the following:
     - **Application type**: Web application

     - **Authorized JavaScript origins**: Add the URL of your portfolio (e.g., `http://localhost:5000` for local testing).


     ![Javascript](./java.png)

     - **Authorized redirect URIs**: Add the redirect URI 


     ![Redirect URI](./redirect.png)

   - After creating the credentials, copy the **Client ID** and **Client Secret** provided.

2. **Install the Required Python Libraries** (for Flask):
   ```bash
   pip install Flask-OAuthlib


3. **Configure Flask to Use Google OAuth**

- In your Flask project, configure the code to handle OAuth 2.0 authentication

4. **Testing the OAuth Login**


- Run the Flask app using the command:

   ```
   python app.py
   ```

   Open a browser and navigate to http://localhost:5000/.

You should be redirected to the Google OAuth login page. After logging in, you will be redirected back to your app, and it should display the logged-in user's email.

![Portfolio Screenshot](./login.png)

![Portfolio Screenshot](./account.png)

![Portfolio Screenshot](./correct.png)


## Step 2: Adding a GitHub Contributions Section

### Instructions:

1. **Use the GitHub Contributions Widget CDN**

- edit your html code!

2. **Customize CSS to match your portfolio theme.**

![Portfolio Screenshot](./github.png)






# Part 2: High-Availability Setup with Load Balancing

This part of the project focuses on configuring a high-availability setup using **Nginx** as a reverse proxy and **HAProxy** for load balancing to ensure scalability and reliability of the portfolio.

---

## Step 1: Preparing Nginx Instances

### Instructions:

1. **Create Two Cloud Instances**:
   - Deploy at least two cloud instances (EC2 instances on AWS) that will serve as your Nginx servers.

   - Each instance will host the portfolio, serving as a reverse proxy for the backend (Flask).

2. **Install Nginx**:
   - SSH into each cloud instance and install Nginx. You can install it using the following command:
     ```bash
     sudo apt update
     sudo apt install -y nginx
     ```

3. **Configure Nginx as a Reverse Proxy**:
   - Configure each Nginx instance to reverse proxy requests to the backend 

   - Modify the Nginx configuration file (`sudo nano /etc/nginx/sites-available/reverse_proxy.conf`) to forward traffic to your Flask app running on a specific port 
   
   Example Nginx configuration:
   
   ![Portfolio Screenshot](./nginx.png)

4. **Enable the config:**

```
sudo ln -s /etc/nginx/sites-available/reverse_proxy.conf /etc/nginx/sites-enabled/

sudo rm /etc/nginx/sites-enabled/default

```
4. **Start Nginx:**

Once configured, verify the Nginx Setup and start it

```
sudo nginx -t
sudo systemctl restart nginx
```

5. **test the reverse proxy with:**

- curl http://localhost

My output looks like  this, but it varies per content


   ![Portfolio Screenshot](./localhost.png)



## Step 2: Configuring HAProxy


### Instructions:
1. **Install HAProxy:**

- Deploy a separate ec2 instance for HAProxy

- Install HAProxy using the following command:

```
sudo apt update
sudo apt install -y haproxy
```

2. **Configure HAProxy:**

- Edit the HAProxy configuration file:
`sudo nano /etc/haproxy/haproxy.cfg` to load balance traffic between the two Nginx instances.

3. **HAProxy configuration:**

![Portfolio Screenshot](./HAproxy.png)


4. **Start HAProxy:**

- Restart HAProxy to apply the configuration:

```
sudo systemctl restart haproxy
```

5. **Verify Load Balancing:**

- Test that the load balancer is distributing traffic correctly by accessing the public IP address or domain of the HAProxy instance.

```
curl http://localhost
curl http://<HAProxy_Public_IP>
```

![Portfolio Screenshot](./localhost.png)

# Part 3: Scalability and Performance

This part of the project ensures the portfolio is not only scalable and performant but also secure and well-documented. It involves simulating traffic to test high availability, applying security best practices for OAuth.


## Step 1: Scalability and Performance Testing

### Objective:
To test how well the application performs under simulated load and to demonstrate the benefits of the HAProxy load balancer setup.

1. **Install Apache Benchmark**
- run the command below:
```
sudo apt update
sudo apt install apache2-utils
```

2.  **Test with Apache Benchmark:**
```bash
ab -n 1000 -c 100 http://<haproxy-public-ip>/
