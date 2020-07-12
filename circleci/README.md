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
- Create a config file `.circleci/config.yml` to your project root with a minimal content of
```yml
version: 2.1
jobs:
  build:
    docker:
      - image: circleci/node:10
    steps:
      - checkout
      - run: echo "A first shell command"
      - run:
          name: A Hello-World Step
          command: |
            echo 'Hello World!'
            echo 'This is the delivery pipeline'
      - run:
          name: Code Has Arrived
          command: |
            ls -al
            echo '^^^That should look familiar^^^'

```
Let's explain a bit very shortly what's in the `config.yml` we will explan more later.

- `version`: Specifying which circleci config version we are using.
- `jobs`: All job will be defined under the jobs block
    - `build`: is the name of our job
        - `docker`: This is kind of a executor (Where the job steps will run)
            - `- image`: Specifying our docker image (it can be multiple)
        - `steps`: is the set of commands that will run step by step based on our definition.
            - `checkout`: Is actually pulling your code from github
            - `run`: This is all about shell so we can write our any command. you can imagine like running command in a terminal. As you can see there is a 2 way of defining it. One is simply define the command and another one is (This is basically the standard way)
                - `name`: A name of your command.
                - `command`: This is holding your shell command, write down `|` in order to add multiple lines.

Let's push our code and see what happens on circleci.
<Image HERE>
So we can see a job called build is passed (Green color refer to pass red is failed). Expand the job, as we can see the echo is printed in console.

Now we will extend our config little more based on our goal. So, what is our goal? Our goal is while someone commit code then 
1. Run lint to check programmatic and stylistic errors. (CI part)
2. Run tests to verify integrity. (CI part)
3. If tests are passed then build and upload image. (CD part)
4. Deploy it when it is realise branch. (CD part)

okay, let's implement the linter and tests first, will do more incrementally.

```yaml
Will write it
```

Here we are seeing a couple of new things, what those are

- `Workflows` allow you to define the sequence of your job and concurrency. You can see under workflow we've written `build` and `test` job and both are running parallelly. We will talk more about workflow strategy in next config.

Let's commit some codes and see what happens.

`IMAGE`

So, as we can see our build is passed but test is failed. Why it failed? okay, let me explore a bit. We can clearly see that the command we are executing that is not available but why? in the `build` job we have pulled the code is not available to the `test` job? that means is it running under new environment? (container) So, do we need to checkout the code again and again install the dependency? Wait, no it should not be like that. Okay, then how we will persist our workspace so that it can be used by any other job? Let me add some new config to do so.

```yaml
Config will be written
```

Have a look newly added stuff
- `persist_to_workspace`: Special step used to persist a temporary file to be used by another job in the workflow.
- `attach_workspace`: Special step used to attach the workflow’s workspace to the current container. The full contents of the workspace are downloaded and copied into the directory the workspace is being attached at.

Commit it and see if it works.

`IMAGE`

It works. Okay, according to our plan now we will move to the `image-building` job.


