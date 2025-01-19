# Name: Swaraj Bhanja
# Student Id: st125052

# Welcome to Search Engine!

This is a web-based end-to-end application named Search Engine. It leverages the power of deep learning and web development to provide a website that returns 10 relevant words based on the input word.


# About the Deep Learning Models

The brains of this solution are the deep learning models trained for this purpose. The DL models were trained based on the NLTK **Brown** corpus that consists of 500 text samples, each containing approximately 2,000 words. It is a collection of texts compiled to represent a wide range of genres in American English as used in the mid-20th century. Techniques like **Word2Vec**, **Word2Vec Negative Sampling**,
**GloVe** and **GloVe Gensim** were used to train the models using this corpus.

# Converting Words To Vectors

The individual words in the cleaned corpus were used to generate word sequences and unique words. After this, the Word-To-Index logic was applied to associate every word with a unique index. Also, an Index-To-Word reverse logic was applied to get the word from a certain index. The function random_batch is designed to generate random batches of word pairs. Each pair consists of a "center word" and its corresponding "outside word" derived from a predefined window size. By iterating over the corpus, the function identifies context words surrounding each center word and stores these relationships as pairs. This was however NOT done explictly for the Gensim implementation.

# Training
Apart from Gensim, training loops were run where the models learned word relationships over 1000 epochs using the Brown corpus. The model predicts outcomes, calculates the loss, and uses backpropagation to adjust its parameters through an optimizer. This iterative process ensures the model improves its understanding of the data. The training loss and training time of each model was noted as well.

## Testing
For testing purposes, the models were pickled and then testing methods were applied. Metrics like MSE, Window Size, Semantic Accuracy & Syntactical Accuracies were recorded for all the four models and can be found in this
hyperlink: [Analysis Metrics](https://github.com/st125052/a1-nlp-search-engine-st125052/blob/a0ac3c5a15e22b7fb0a7af117ad0f38168716df5/notebooks/pdfs/Training%20and%20Accuracy%20Statistics.pdf)
> - The Spearman's correlation coefficients were also computed. However, for all the values, the correlations were not above the p-values, hence the current models do NOT correlate well with human judgment. This could possibly be attributed to the limitation of the corpus selected.

## Pickling The Model
The Gensim ML model was chosen for deployment.
> The pickled model was saved using a .pkl extension to be used later in a web-based application

# Website Creation
The model was then hosted over the Internet with Flask as the backend, HTML, CSS, JS as the front end, and Docker as the container. The following describes the key points of the hosting discussion.
> **1. DigitalOcean (Hosting Provider)**
> 
>> - **Role:** Hosting and Server Management
>> - **Droplet:** Hosts the website on a virtual server, where all files, databases, and applications reside.
>> - **Dockerized Container:** The website is hosted in a Dockerized container running on the droplet. The container is built over a Ubuntu Linux 24.10 image.
>> - **Ports and Flask App:** The Dockerized container is configured to host the website on port 8000. It forwards requests to port 5000, where the Flask app serves the backend and static files. This flask app contains the pickled model, which is used for prediction.
>> - **IP Address:** The droplet’s public IP address directs traffic to the server.
>
>  **In Summary:** DigitalOcean is responsible for hosting the website within a Dockerized container, ensuring it is online and accessible via its IP address.
> 
>  **2. GoDaddy (Domain Registrar)**
>
>> - **Role:** Domain Registration and Management
>> - **Domain Purchase:** Registers and manages the domain name.
>> - **DNS Management:** Initially provided DNS setup, allowing the domain to be pointed to the DigitalOcean droplet’s IP address.
> 
> **In Summary:** GoDaddy ensures the domain name is registered and correctly points to the website’s hosting server.
>
>  **3. Cloudflare (DNS and Security/Performance Optimization)**
>
>> - **Role:** DNS Management, Security, and Performance Optimization
>> - **DNS Management:** Resolves the domain to the correct IP address, directing traffic to the DigitalOcean droplet.
>> - **CDN and Security:** Caches website content globally, enhances performance, and provides security features like DDoS protection and SSL encryption.
> 
> **In Summary:** Cloudflare improves the website’s speed, security, and reliability.
>
> **How It Works Together:**
> 
>> - **Domain Resolution:** The domain is registered with GoDaddy, which points it to Cloudflare's DNS servers. Cloudflare resolves the domain to the DigitalOcean droplet's IP address.
>> - **Content Delivery:** Cloudflare may serve cached content or forward requests to DigitalOcean, which processes and serves the website content to users.
> 
> **Advantages of This Setup:**
>
>> - **Security:** Cloudflare provides DDoS protection, SSL/TLS encryption, and a web application firewall.
>> - **Performance:** Cloudflare’s CDN reduces load times by caching content globally, while DigitalOcean offers scalable hosting resources.
>> - **Reliability:** The combination of GoDaddy, Cloudflare, and DigitalOcean ensures the website is always accessible, with optimized DNS resolution and robust hosting.


# Access The Final Website
You can access the website [here](https://aitmltask.online)


# How to Run the Search Engine Docker Container Locally
### Step 1: Clone the Repository
> - First, clone the repository to your local machine.
### Step 2: Install Docker
> - If you don't have Docker installed, you can download and install it from the [Docker](https://www.docker.com) website.
### Step 3: Build and Run the Docker Container
Once Docker is installed, navigate to the app folder in the project directory. Delete the docker-compose-deployment.yml file and run the following commands to build and run the Docker container:
> - `docker compose up -d`

### Important Notes
> - The above commands will serve the Docker container on port 5000 and forward the requests to the Flask application running on port 5000 in the containerized environment.
> - Ensure Ports Are Free: Make sure that port 5000 is not already in use on your machine before running the container.
> - Changing Flask's Port: If you wish to change the port Flask runs on (currently set to 5000), you must update the port in the app.py file. After making the change, remember to rebuild the Docker image in the next step. Execute the following command to stop the process: `docker compose down`. Then goto Docker Desktop and delete the container and image from docker. 

