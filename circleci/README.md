### What is CI/CD?
CI/CD or CICD generally refers to the combined practices of continuous integration and either continuous delivery or continuous deployment
- `Continuous Integration (CI)` is the practice of automating the integration of code changes from multiple contributors and check the new code’s correctness by automated code quality tests like smoke, unit, integration.
- `Continuous Delivery (CD)` is the software development process of getting code changes into production quickly and safely.


### What is CircleCI?
CircleCI is a fully managed continuous integration and delivery platform which allows us to build, test or deploy our code on every check in.
<img src="./assets/cicd-process.png">

CircleCI uses a YAML configuration file to declare the pipeline and it lives under the project root `.circleci` folder with name `config.yml` (Default convention)

#### YAML in short:
<img src="./assets/yaml.jpg">
Let get familier with some basic syntax that will frequently use for the configuration. 
Here we will compare with JSON so that we can easily understand which syntax is using for what.
<img src="./assets/json-vs-yaml.png">

### Getting Started
So, let's configure a pipeline for a mini project. We will start from minimal configuration.
- First of all, login to circleci with github
- Setup project
- Add a directory `.circleci`  to your project root and a `config.yml` with content of
```yml
version: 2.1
jobs:
  build:
    docker:
      - image: circleci/node:8.10
    steps:
      - checkout
      - run: echo "A first hello"
```