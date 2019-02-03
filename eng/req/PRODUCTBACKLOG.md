### Goals
- Reasons
  - Product owner, working on a new idea, want to deploy their application quickly. They want to validate their product.
  - Developing a solution that scales vertically allows developers to keep code simple.
  - Development time for software that will be scaled vertically is less compare to the development time for the software that scales horizontally.
  - Infrastructure need to deploy vertical application is less complicated compared to application that is design with horizontal scaling in mind.
  - Asp.net Core can handle tens of thousands requests per seconds with vertical scaling. Here is the [bechmark](https://www.techempower.com/benchmarks/) and [article](https://www.techempower.com/blog/2016/11/16/framework-benchmarks-round-13/). If you move your application to a large machine on a public cloud, then it can handle thousands to customers without you needing to scale horizontally for a long time.
- Strategy we will follow
  - Vertical scaling over horizontal scaling.(If application is successful, then developer can spend time fixing scaling problem.)
  - Deployment ease should be pay a role when choosing an implementation for a feature.

### Stories
- Should enable Docker support in visual studio project. So, the developers will not need to download .NET SDK to development on their machine.
  - **Will not be implemented yet.** Looks like Visual Studio support for Docker is in primitive stage. Currently, there is no way to run and debug unit tests in Docker container. More, [here](https://techblog.dorogin.com/running-and-debugging-net-core-unit-tests-inside-docker-containers-48476eda2d2a) and [here](https://github.com/Microsoft/DockerTools/issues/77)
  - Development SQL server should be started on Docker when project starts.
  - Development SQL server should be migrated on project startup.
  - Development SQL server should be seeded on project startup.
- Allow developers to use this project by simply copying files from this project to their project.
  - We have to allow ```webapp``` project name to be customized in docker file. We can implement this using ARG and ENV in docker file.
  - Stages that we want to have
    - Minimum for deployment: ```DockerfileWebapp``` and ```Heroku.yml```
    - Release phase and automatic migrations: ```release_phase.sh```
    - Partial CI - test code before deplyment: ```tests.sh``` 
- Look into a way to monitor application server's resource usage.(RAM, bandwidth, CPU usage)
  - Look into possibility to enable new Relic for Heroku. More [here](http://blog.avenuecode.com/tricks-for-configuring-new-relic-for-.net-core) and [here](https://docs.newrelic.com/docs/agents/net-agent/installation/new-relic-net-agent-install-introduction#common-installs)
  - Let developer know when it is time to scale fleet of dynos.
  - Let developer know about inefficient parts of their application.
- Look into a way to monitor resources used by DB server.
- Add files that list all tools needed when using the project and when developing the project.
- This about creating a NuGet package that will add the functionalities to existing project.
- Add steps to upgrade version of .NET
- Add steps to change project name. 
- Analyze how Heroku's [build limits](https://devcenter.heroku.com/articles/limits#build) can potentially effect our project.
- Make a note of Heroku's [load testing](https://devcenter.heroku.com/articles/load-testing-guidelines#common-runtime-limits) limits.
- How to async lock. One of the way is to use [Nito.AsyncEx](https://www.nuget.org/packages/Nito.AsyncEx/).

### Implemented
- Create initial implementation of the project based on Ikechi Michael's [article](https://blog.devcenter.co/deploy-asp-net-core-2-0-apps-on-heroku-eea8efd918b6) to deploy Asp.net Core app with docker on Heroku.
- Provide steps to perform logging.
  - There are a lot of third party addins. Currently, we are looking into implementing this by enabling [Logentries](https://elements.heroku.com/addons/logentries) addin over [Logplex](https://devcenter.heroku.com/articles/logplex).