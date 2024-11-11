## Explanation of Playbook Structure and Execution Order
Overview:
This Ansible playbook, is designed to provision and run a fully containerized e-commerce platform. The playbook performs several key setup steps, each handled by dedicated roles. Below is a breakdown of each role and the rationale behind the order of execution. Since Ansible playbooks run sequentially, this order ensures that the dependencies required for each step are in place.
## Roles and Execution Order
## Docker Role
    - Purpose: Sets up the Docker environment, including Docker Engine, Docker Compose, and Git cloning of the project repository. Docker is a fundamental dependency since the frontend, backend, and MongoDB services all run as Docker containers.
    - Positioning: This role runs first because Docker is a core dependency for the entire application. The rest of the roles rely on Docker being fully functional.
        Tasks:
            - Install Python3, Pip, and Docker SDK: Installs dependencies for managing Docker through Python.
            - Install Docker and Docker Compose: Configures the official Docker repository, adds the GPG key, installs Docker Engine, and sets up Docker Compose from the binary.
            - Clone the GitHub Repository: Fetches the latest application code for deployment.
            - Check and Copy docker-compose.yml: Ensures the Docker Compose configuration file is available.
            - Run docker-compose up: Initiates containerized services defined in docker-compose.yml.


    - Modules Used:
        apt, apt_key, apt_repository for package management.
        pip for Python package installation.
        user to add the current user to the Docker group.
        get_url for downloading the Docker Compose binary.
        command to verify Docker Compose installation.
## Frontend Role
        - Purpose: Deploys the frontend application, which users interact with for the e-commerce experience.
        - Positioning: After Docker, this role sets up the frontend container, pulling the image from the repository and building it locally if necessary.

        - Tasks:
            - Pull Image from Repository: Downloads the latest frontend image for deployment.
            - Build Frontend Image: Builds the image locally from the specified directory if changes are needed.

        - Modules Used:
            - docker_image to pull the image.
            - command to build the frontend Docker image using docker build.

## Mongo Role
    - Purpose: Sets up MongoDB for persistent data storage, ensuring that product data is retained even after the containers are restarted.
    - Positioning: MongoDB is configured before the backend, as the backend relies on a database connection to function properly.
    - Tasks
        - Create Network and Volume: Creates a Docker network and volume for MongoDB.
        - Run MongoDB Container: Deploys the MongoDB container with the created volume for data persistence and the network configuration for inter-container communication.
    - Modules Used:
        - docker_network to create a network for inter-container communication.
        - docker_volume to set up a volume for data persistence.
        - docker_container to run MongoDB in a container.


## Backend Role
    - Purpose: Deploys the backend service, which manages API requests and communicates with MongoDB.
    - Positioning: The backend depends on MongoDB to function, so it’s deployed after the MongoDB container is live and accessible.
    - Tasks
        - Pull Backend Image: Pulls the backend image from the repository.
        - Run Backend Container: Deploys the backend container, configuring it to connect to MongoDB on the predefined network.
        - Build Backend Image: Builds the backend image locally from the specified directory if changes are required.
    - Modules Used:
        - docker_image to pull the backend image.
        - docker_container to run the backend container, linking it to MongoDB.
        - command for building the backend image with docker build.


In the repository, you will find:

    vars/main.yml: This file contains variables shared across roles, such as compose_file and github_repo and branch to be used when cloning. Used main as that was my active branch.
    README.md: A comprehensive guide on how to clone, build, and run the application using vagrant up command, and ansible for configuration.