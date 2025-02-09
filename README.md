<div align="center">

  <div>
      <img src="https://img.shields.io/badge/-Docker-black?style=for-the-badge&logoColor=white&logo=docker&color=2496ED" alt="docker" />
  </div>

  <h3 align="center">Notes On Docker</h3>

</div>

<br />

## <a name="code-snippets">🕸️ Code Snippets</a>

<details>
<summary><code>Docker Anologies</code></summary>

```dockeranologies
Docker concepts using a restaurant analogy:

🏢 Docker as a Restaurant
Imagine Docker is like running a restaurant. Different components of Docker represent different parts of how a restaurant operates.

📜 Dockerfile → Kitchen Setup Instructions + Recipe
A Dockerfile is like a complete set of instructions for setting up the kitchen AND preparing the dish. It includes both environment setup (installing equipment, setting up workstations) and the actual recipe steps.

🍽️ Image → Prepared Dish
A Docker Image is like a fully prepared dish based on the recipe (Dockerfile). Once a dish is ready, it can be served repeatedly without needing to cook it from scratch again.

🛒 Container → Private Dining Room with a Served Dish
A Container is like a private dining room with its own served dish. It has its own isolated space, resources, and environment (like a private room with its own temperature control, music, etc.) while still being part of the main restaurant building (host system).

🍱 Docker Compose → Restaurant Menu & Order Management
Docker Compose is like the restaurant menu and ordering system that allows the chef to cook multiple dishes (run multiple services) together. Instead of manually preparing one dish at a time, the system ensures multiple meals (containers) are prepared and served in the right order.

🔄 Docker Compose Watch → Chef Watching for Orders & Adjusting the Menu
Docker Compose Watch is like a chef who keeps an eye on ongoing orders and automatically adjusts the menu when needed. If a dish is modified (code changes), the chef updates the recipe and re-prepares meals accordingly.

🔒 Container Networking → Restaurant Service Areas
Container networking is like how different areas of the restaurant (bar, kitchen, dining room) communicate and pass items between each other while maintaining proper separation and security - just like how containers can communicate while staying isolated.

📦 Volumes → Restaurant's Pantry
Volumes are like a restaurant's pantry that can be accessed by multiple chefs (containers) and persists even when chefs change shifts or got removed. The ingredients and supplies in the pantry remain available regardless of what happens in individual cooking stations.

🏪 Docker Daemon → Restaurant Kitchen Operations System
The Docker Daemon is like the entire kitchen management system - controlling food storage, cooking equipment, order processing, and resource allocation. It's not just the staff, but the whole operational infrastructure that makes everything work together.

👨‍🍳 Docker Client → Customer Placing an Order
A Docker Client is like a customer placing an order at the restaurant. The customer (developer) tells the kitchen staff (Docker Daemon) what dishes to prepare (commands like docker run, docker build), and they take care of the rest.

🛎️ Docker Hub → Restaurant Supplier
Docker Hub is like a food supplier that delivers pre-prepared ingredients (pre-built images) so the restaurant can quickly cook meals without having to prepare everything from scratch.

🔄 Container Orchestration (Swarm/Kubernetes) → Multiple Restaurant Branches
If you run multiple restaurants (servers), you need a system to coordinate everything efficiently. Docker Swarm or Kubernetes is like a restaurant chain management system, ensuring all branches serve dishes consistently across different locations.
```

</details>

<details>
<summary><code>Docker Notes</code></summary>

```dockernotes
Docker Notes🐳

🍀Traditional Approach (Old Way)🍀

1. Create a Dockerfile

	Define the necessary setup and dependencies inside a Dockerfile.
	Build a Docker Image
	Run the following command to create a Docker image:
	docker build -t image_name path_to_dockerfile

	Example:
	docker build -t codemate .
	(The dot represents the current directory, which contains the Dockerfile.)

2. Run a Container

	Run the following command to create and run a container:
	docker run -p host_port:container_port image_name

	Example:
	docker run -p 5173:5173 -v "$(pwd):/app" -v /app/node_modules codemate

	Explanation:

	-p 5173:5173
	The first 5173 is the port exposed inside the container (defined in the Dockerfile). The second 5173 is the port on the host machine that maps to the 	container’s port.

	-v "$(pwd):/app"
	Creates a volume that syncs the current working directory ($(pwd)) with /app in the container. Ensures live updates in the container when files are modified on the host.

	-v /app/node_modules
	Creates a separate volume for node_modules inside the container.
	Prevents dependencies from being reinstalled each time the container starts, improving efficiency during development.

	⚠️Important:

        (NOT APPLICABLE TO NEXT.JS !!)
	Modify package.json to include --host in the dev script to ensure the framework listens on the correct port.
	Example:
	"scripts": {
	"dev": "vite --host"
	}

3. Publish the Image to Docker Hub

	Step 1: Login to Docker Hub:
	docker login

	Step 2: Tag the Image:
	docker tag local_image_name dockerhub_username/repository_name

	Example:
	docker tag react-docker shahirulprojects/react-docker

	Step 3: Push the Image to Docker Hub
	docker push dockerhub_username/repository_name

	Example:
	docker push shahirulprojects/react-docker

🍀Modern Approach (New Way)🍀

1. Initialize Docker Configuration

	Run the following command to generate necessary Docker configuration files:
	docker init

2. Modify compose.yaml (If Needed)

	Adjust settings inside compose.yaml to fit your project requirements.

3. Update package.json (NOT APPLICABLE TO NEXT.JS !!)

	Ensure the dev script in package.json includes --host:
	Example:
	"scripts": {
	"dev": "vite --host"
	}

4. Run the following command to run the Container Using Docker Compose:

	docker compose up

5. Run the following command to sync every package or file changes:
     
       docker compose watch

6.Publish the Image to Docker Hub

	Step 1: Login to Docker Hub:
	docker login

	Step 2: Tag the Image:
	docker tag local_image_name dockerhub_username/repository_name

	Example:
	docker tag react-docker shahirulprojects/react-docker

	Step 3: Push the Image to Docker Hub
	docker push dockerhub_username/repository_name

	Example:
	docker push shahirulprojects/react-docker


🦕EXTRA NOTES (VERY IMPORTANT TO PRACTICE!!):🦕

🦩Running Containers in Different Modes🦩

Regular Mode (Foreground)

docker run my-container
# or
docker compose up

- Container runs in the foreground with live logs in terminal
- Terminal is blocked while container runs
- Ctrl+C stops the container
- Best for: Development, debugging, and situations where you need to monitor real-time output


Detached Mode (Background)

docker run -d my-container
# or
docker compose up -d

- Container runs in the background
- Terminal remains free for other commands
- Container continues running even if terminal closes
- Best for: Production environments, running multiple services, CI/CD pipelines

Useful Commands for Detached Containers:

# View logs of a detached container
docker logs -f container_name

# Check running containers
docker ps

# Stop a detached container
docker stop container_name

# Execute commands in running container
docker exec -it container_name bash
When to Use Each Mode:

Use Regular Mode when:

- Developing and debugging your application
- Need to see immediate log output
- Running short-lived processes
- Testing container configurations


Use Detached Mode when:

- Running services in production
- Managing multiple containers simultaneously
- Setting up development environments with multiple services
- Running containers that need to persist after terminal closes
- Executing CI/CD pipelines

🦠There are significant operational risks and drawbacks if you don't use detached mode in production.🦠

Without Detached Mode in Production:

1. Reliability Risks:

- If your terminal session disconnects (network issues, SSH timeout), the container stops
- If the terminal window closes accidentally, your service goes down
- Server reboots would require manual container restart

2. Resource Management Issues:

- Each container needs a dedicated terminal session
- Managing multiple services becomes impractical
- SSH sessions need to stay open permanently

🦩Docker Scout🦩

- Docker Scout is used to scan the images in the container and all the layouts and software pieces to look for vulnerability and weakpoints that might be exposed for cyber attacks
- It creates a detailed list called Software Bill of Materials (SBOM). This list includes all the thing that our container is made of, and then docker scout checks the list against always updated database of known vulnerabilities
- It is VERY important to scan our images before deploying them for production
- We can use docker scout at docker desktop, docker hub, docker CLI
```

</details>
