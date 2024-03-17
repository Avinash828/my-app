
# my-app

## Tasks

### Version Control

**Create a new public GitHub repository to include directories for Go,
Next.js, and WordPress components.**
 
- Create a New Repository:

Click on the "+" icon in the top-right corner of the GitHub interface.
Select "New repository" from the dropdown menu.
Choose a name for your repository. Make it descriptive, like "project-name-components".
Optionally, add a description.
Choose whether the repository will be public or private. Since you mentioned "public," select the "Public" option.
Initialize this repository with a README (optional but recommended).
Click "Create repository".

- Clone the repo Locally.
```console
git clone https://github.com/your-username/project-name-components.git
Replace "your-username" with your GitHub username and "project-name-components" with the name of your repository.
```
- Configure the git origin.

```bash
git remote add origin https://github.com/your-username/project-name-components.git
```
- Start pushing files to repo.


**Initialise the repositories for each component.**

For each component (Go, Next.js, and WordPress), navigate to its respective directory in your local repository.

``Initialize Git if you haven't already: git init.``


### Continuous Integration & Coding Standards Enforcement:
**Implement CI workflows for Go, Next.js (TypeScript), and PHP
(WordPress).
Configure pipelines to trigger on pushes to the respective branches
(e.g., main, feature branches).**

```yml
name: Wordpress CI

on:
  push:
    branches:
      - main
      - 'feature/*'

```
**Integrate linting and unit testing for each technology with coding standards .
Ensure that the CI pipelines fail if coding standards or tests are not
met.**

`For go`

```yml
      - name: Run golangci-lint
        run: golangci-lint run ./...
        continue-on-error: false # Fail the workflow if linting fails

```

`For nextJS`

```yml

- name: Run ESLint
        run: npx eslint --ext .js,.jsx,.ts,.tsx .
        continue-on-error: false

```

`For Wordpess`

```yml
- name: Run phpcs
        run: phpcs -q --report=checkstyle src | cs2pr
        continue-on-error: false

```

### Containerization:

**Dockerize the Go application, the Next.js (TypeScript) application, and
the WordPress site.
Create Dockerfiles for each application.**

Created sample dockerfile in each folder.

**Push the Docker images to a container registry (e.g., Docker Hub, AWS
ECR).**

Created in the pipeline through github actions tools

```yml
- name: Login to DockerHub
        uses: docker/login-action@v1
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}

      - name: Build and push Dockerimage to DockerHub
        uses: docker/build-push-action@v2
        with:
          context: ./go/
          file: ./go/Dockerfile
          push: true
          tags: Dockerhub_username/my-app_Wordpress:latest,Dockerhub_username/my-app_Wordpress:any_tag
```

```console
**NOTE:**- We can use "BUILDX" for multiarch images"
```

### Orchestration:
**Update the Docker Compose file to include services for Go, Next.js, and
WordPress.
Ensure that the Compose file can be used to spin up the entire
extended application stack locally.**

Create a `Docker-compose` file in the root path.

### Continuous Deployment:
**Extend the CI/CD pipelines to include deployment stages for the Go,
Next.js, and WordPress components.**

```yml
 Deployment:
      needs: build
      name: Build and Test
      runs-on: ubuntu-latest
      steps:
      - name: Checkout Repository
        uses: actions/checkout@v2
       steps:
      - Install Docker-compose
        uses: KengoTODA/actions-setup-docker-compose@v1
        with:
          version: '2.14.2'
      - name: Spin up comtainer
        run: docker-compose up -d

```

**NOTE:-** Add a new job that required a build step to finish first due to the ``needs`` condition.
