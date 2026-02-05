# Simple Web Application – End-to-End DevOps Demo

This is a simple web application that displays the message "This is an end-to-end DevOps project." It is designed to demonstrate the use of Docker for containerization and CI/CD with Jenkins.

---

## Project Structure

```
jenkins-ci-cd/
├── src/
│   ├── index.html      # Main HTML file
│   ├── styles.css      # CSS for styling
│   └── app.js          # JavaScript for interactivity
├── Dockerfile          # Builds the Nginx-based image
├── docker-compose.yaml # Defines how to run the app container
├── Jenkinsfile         # Jenkins pipeline for CI/CD
└── README.md           # Project documentation
```

---

## Running the Application Locally (Without Docker)

1. Open the `src/index.html` file directly in your browser  
   **OR**
2. Use a simple HTTP server (recommended for JS/CSS):
    - With Python:
        ```
        cd src
        python -m http.server 8000
        ```
        Then visit [http://localhost:8000](http://localhost:8000)

---

## Running with Docker Compose

1. Navigate to the project directory:

    ```
    cd jenkins-ci-cd
    ```

2. Build the Docker image and start the container using Docker Compose:

    ```
    docker-compose up --build
    ```

3. Open a web browser and go to [http://localhost:8080](http://localhost:8080) to view the application.

---

## CI/CD Deployment Workflow

1. **Jenkins Pipeline:**
    - Jenkins builds the Docker image from the `Dockerfile`
    - Jenkins tags and pushes the image to a Docker registry (e.g., Docker Hub)
2. **Production/Deployment Server:**
    - Pulls the latest image from the registry
    - Runs `docker-compose up -d` to start the app

**Example `docker-compose.yaml` for deployment:**

```yaml
version: "3.8"
services:
    web:
        image: your-dockerhub-username/your-image-name:tag
        ports:
            - "8080:80"
        volumes:
            - web-data:/usr/share/nginx/html

volumes:
    web-data:
```

Replace `your-dockerhub-username/your-image-name:tag` with your actual image name and tag.

---

## Summary

-   **Frontend:** HTML, CSS, JavaScript
-   **Containerization:** Docker, Nginx
-   **Orchestration:** Docker Compose
-   **CI/CD:** Jenkins (build, push, deploy)
-   **Demo:** "This is an end-to-end DevOps project" (with interactive JS)

---
