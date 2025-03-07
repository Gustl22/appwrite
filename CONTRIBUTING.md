# Contributing

We would ❤️ for you to contribute to Appwrite and help make it better! We want contributing to Appwrite to be fun, enjoyable, and educational for anyone and everyone. All contributions are welcome, including issues, new docs as well as updates and tweaks, blog posts, workshops, and more.

## How to Start?

If you are worried or don’t know where to start, check out our next section explaining what kind of help we could use and where can you get involved. You can reach out with questions to [Eldad Fux (@eldadfux)](https://twitter.com/eldadfux) or [@appwrite_io](https://twitter.com/appwrite_io) on Twitter, and anyone from the [Appwrite team on Discord](https://discord.gg/GSeTUeA). You can also submit an issue, and a maintainer can guide you!

## Code of Conduct

Help us keep Appwrite open and inclusive. Please read and follow our [Code of Conduct](/CODE_OF_CONDUCT.md).

## Submit a Pull Request 🚀

Branch naming convention is as following 

`TYPE-ISSUE_ID-DESCRIPTION`

example:
```
doc-548-submit-a-pull-request-section-to-contribution-guide
```

When `TYPE` can be:

- **feat** - is a new feature
- **doc** - documentation only changes
- **cicd** - changes related to CI/CD system
- **fix** - a bug fix
- **refactor** - code change that neither fixes a bug nor adds a feature

**All PRs must include a commit message with the changes description!** 

For the initial start, fork the project and use git clone command to download the repository to your computer. A standard procedure for working on an issue would be to:

1. `git pull`, before creating a new branch, pull the changes from upstream. Your master needs to be up to date.
```
$ git pull
```
2. Create new branch from `master` like: `doc-548-submit-a-pull-request-section-to-contribution-guide`<br/>
```
$ git checkout -b [name_of_your_new_branch]
```
3. Work - commit - repeat ( be sure to be in your branch )

4. Push changes to GitHub 
```
$ git push origin [name_of_your_new_branch]
```

5. Submit your changes for review
If you go to your repository on GitHub, you'll see a `Compare & pull request` button. Click on that button.
6. Start a Pull Request
Now submit the pull request and click on `Create pull request`.
7. Get a code review approval/reject
8. After approval, merge your PR
9. GitHub will automatically delete the branch after the merge is done. (they can still be restored).

## Setup From Source

To set up a working **development environment**, just fork the project git repository and install the backend and frontend dependencies using the proper package manager and create run the docker-compose stack.

> If you just want to install Appwrite for day-to-day usage and not as a code maintainer use this [installation guide](https://github.com/appwrite/appwrite#installation).

Please note that these instructions are for setting a functional dev environment. If you want to set up an Appwrite instance to integrate into your app, you should probably try and install Appwrite by using the instructions in the [getting started guide](https://appwrite.io/docs/getting-started-for-web) or in the main [README](README.md) file.

```bash
git clone git@github.com:[YOUR_FORK_HERE]/appwrite.git

cd appwrite

docker-compose up -d
```

After finishing the installation process, you can start writing and editing code. To compile new CSS and JS distribution files, use 'less' and 'build' tasks using gulp as a task manager.

## Architecture

Appwrite's current structure is a combination of both [Monolithic](https://en.wikipedia.org/wiki/Monolithic_application) and [Microservice](https://en.wikipedia.org/wiki/Microservices) architectures, but our final goal, as we grow, is to be using only microservices.

### File Structure

```bash
.
├── app # Main application
│   ├── config # Config files
│   ├── controllers # API & dashboard controllers
│   │   ├── api
│   │   ├── shared
│   │   └── web
│   ├── db # DB schemas
│   ├── sdks # SDKs generated copies (used for generating code examples)
│   ├── tasks # Server CLI commands
│   ├── views # HTML server-side templates
│   └── workers # Background workers
├── bin # Server executables (tasks & workers)
├── docker # Docker related resources and configs
├── docs # Docs and tutorials
│   ├── examples
│   ├── references
│   ├── sdks
│   ├── services
│   ├── specs
│   └── tutorials
├── public # Public files
│   ├── dist
│   ├── fonts
│   ├── images
│   ├── scripts
│   └── styles
├── src # Supporting libraries (each lib has one role, common libs are released as individual projects)
│   └── Appwrite
│       ├── Auth
│       ├── Database
│       ├── Docker
│       ├── Event
│       ├── Extend
│       ├── Network
│       ├── OpenSSL
│       ├── Preloader
│       ├── Resize
│       ├── Storage
│       ├── Swoole
│       ├── Task
│       ├── Template
│       ├── URL
│       └── Utopia
└── tests # End to end & unit tests
    ├── e2e
    ├── resources
    └── unit
```

---
![Appwrite](docs/specs/overview.drawio.svg)
---

### The Monolithic Part

Appwrite's main API container is designed as a monolithic app. This is a decision we made to allow us to develop the project faster while still being a very small team.

Although the Appwrite API is a monolithic app, it has a very clear separation of concern as each internal service or worker is separated by its container, which will allow us as we grow to start breaking services for better maintenance and scalability.

### The Microservice Part

Each container in Appwrite is a microservice on its own. Each service is an independent process that can scale without regard to any of the other services.

Currently, all of the Appwrite microservices are intended to communicate using the TCP protocol over a private network. You should be aware to not expose any of the services to the public-facing network, besides the public port 80 and 443, who, by default, are used to expose the Appwrite HTTP API.

## Technology Stack

To start helping us to improve the Appwrite server by submitting code, prior knowledge of Appwrite's technology stack can help you with getting started.

Appwrite stack is combined from a variety of open-source technologies and tools. Appwrite backend API is written primarily with PHP version 7 and above on top of the [Utopia PHP framework](https://github.com/utopia-php/framework). The Appwrite frontend is built with tools like gulp, less, and [litespeed.js](https://github.com/litespeed-js). We use Docker as the container technology to package the Appwrite server for easy integration on-cloud, on-premise, or on-localhosts.

### Other Technologies

* Redis - for managing cache and in-memory data (currently, we do not use Redis for persistent data)
* MariaDB - for database storage and queries
* InfluxDB - for managing stats and time-series based data
* Statsd - for sending data over UDP protocol (using Telegraf)
* ClamAV - for validating and scanning storage files
* Imagemagick - for manipulating and managing image media files.
* Webp - for better compression of images on supporting clients
* SMTP - for sending email messages and alerts
* Resque - for managing data queues and scheduled tasks over a Redis server

## Package Managers

Appwrite uses a package manager for managing code dependencies for both backend and frontend development. We try our best to avoid creating any unnecessary, and any new dependency to the project is subjected to a lead developer review and approval.

Many of Appwrite's internal modules are also used as dependencies to allow other Appwrite's projects to reuse them and as a way to contribute them back to the community.

Appwrite uses [PHP's Composer](https://getcomposer.org/) for managing dependencies on the server-side and [JS NPM](https://www.npmjs.com/) for managing dependencies on the frontend side.

## Coding Standards

Appwrite is following the [PHP-FIG standards](https://www.php-fig.org/). Currently, we are using both PSR-0 and PSR-4 for coding standards and autoloading standards. Soon we will also review the project for support with PSR-12 (Extended Coding Style).

We use prettier for our JS coding standards and auto-formatting our code.

## Scalability, Speed, and Performance

Appwrite is built to scale. Please keep in mind that the Appwrite stack can run in different environments and different scales.

We wish Appwrite will be as easy to set up and in a single, localhost, and easy to grow to a large environment with dozens and even hundreds of instances.

When contributing code, please take into account the following considerations:

* Response Time
* Throughput
* Requests per Seconds
* Network Usage
* Memory Usage
* Browser Rendering
* Background Jobs
* Task Execution Time

## Security & Privacy

Security and privacy are extremely important to Appwrite, developers, and users alike. Make sure to follow the best industry standards and practices.

<!-- To help you make sure you are doing as best as possible, we have set up our security checklist for pull requests and contributors. Please make sure to follow the list before sending a pull request. -->

## Dependencies

Please avoid introducing new dependencies to Appwrite without consulting the team. New dependencies can be very helpful but also introduce new security and privacy issues, complexity, and impact total docker image size.

Adding a new dependency should have vital value on the product with minimum possible risk.

## Introducing New Features

We would 💖 you to contribute to Appwrite, but we would also like to make sure Appwrite is as great as possible and loyal to its vision and mission statement 🙏.

For us to find the right balance, please open an issue explaining your ideas before introducing a new pull request.

This will allow the Appwrite community to have sufficient discussion about the new feature value and how it fits in the product roadmap and vision.

This is also important for the Appwrite lead developers to be able to give technical input and different emphasis regarding the feature design and architecture.

## Build

To build a new version of the Appwrite server, all you need to do is run the build.sh file like this:

```bash
bash ./build.sh 1.0.0
```

Before running the command, make sure you have proper write permissions to the Appwrite docker hub team.

**Build for multicore**

```bash
docker buildx build --platform linux/amd64,linux/arm64,linux/arm/v7 -t appwrite/multicore:0.0.0 --push
```

## Tests

To run tests manually, use the Appwrite Docker CLI from your terminal:

```bash
docker exec appwrite test
```

## Code Maintenance  

We use some automation tools to help us keep a healthy codebase.

Improve PHP execution time by using [fully-qualified function calls](https://veewee.github.io/blog/optimizing-php-performance-by-fq-function-calls/):

```bash
php-cs-fixer fix src/ --rules=native_function_invocation --allow-risky=yes
```

Coding Standards:

```bash
php-cs-fixer fix app/controllers --rules='{"braces": {"allow_single_line_closure": true}}'
```

```bash
php-cs-fixer fix src --rules='{"braces": {"allow_single_line_closure": true}}'
```

## Tutorials

From time to time, our team will add tutorials that will help contributors find their way in the Appwrite source code. Below is a list of currently available tutorials:

* [Adding Support for a New OAuth2 Provider](./docs/tutorials/add-oauth2-provider.md)
* [Appwrite Environment Variables](./docs/tutorials/environment-variables.md)
* [Running in Production](./docs/tutorials/running-in-production.md)


## Other Ways to Help

Pull requests are great, but there are many other areas where you can help Appwrite. 

### Blogging & Speaking

Blogging, speaking about, or creating tutorials about one of Appwrite’s many features. Mention [@appwrite_io](https://twitter.com/appwrite_io) on Twitter and/or email team [at] appwrite [dot] io so we can give pointers and tips and help you spread the word by promoting your content on the different Appwrite communication channels. Please add your blog posts and videos of talks to our [Awesome Appwrite](https://github.com/appwrite/awesome-appwrite) repo on GitHub.

### Presenting at Meetups

Presenting at meetups and conferences about your Appwrite projects. Your unique challenges and successes in building things with Appwrite can provide great speaking material. We’d love to review your talk abstract/CFP, so get in touch with us if you’d like some help!

### Sending Feedbacks & Reporting Bugs

Sending feedback is a great way for us to understand your different use cases of Appwrite better. If you had any issues, bugs, or want to share about your experience, feel free to do so on our GitHub issues page or at our [Discord channel](https://discord.gg/GSeTUeA).

### Submitting New Ideas

If you think Appwrite could use a new feature, please open an issue on our GitHub repository, stating as much information as you can think about your new idea and it's implications. We would also use this issue to gather more information, get more feedback from the community, and have a proper discussion about the new feature.

### Improving Documentation

Submitting documentation updates, enhancements, designs, or bug fixes. Spelling or grammar fixes will be very much appreciated.

### Helping Someone

Searching for Appwrite on Discord, GitHub, or StackOverflow and helping someone else who needs help. You can also help by reaching others how to contribute to Appwrite's repo!

