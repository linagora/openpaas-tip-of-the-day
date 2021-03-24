![Archived](https://img.shields.io/badge/Current_Status-archived-blue?style=flat)

# OpenPaas one tip per day

Here you will find a great daily OpenPaas tip that will teach you everything about this very big project.

## How to contribute here?

Do not hesitate to send a beautiful PR!
Follow those [instructions](https://github.com/linagora/openpaas-tip-of-the-day/blob/gh-pages/CONTRIBUTING.md).

Then your tip will directly be added and displayed with your name, in the [official blog](http://linagora.github.io/openpaas-tip-of-the-day/).

### Running locally

This blog is powered by the awesome [jekyll-now](https://github.com/barryclark/jekyll-now).
You can run it locally by using [Jekyll](https://jekyllrb.com) or [Docker](https://docker.com) as described below.

#### Using Jekyll

Install the `github-pages` gem:

```sh
gem install github-pages
```

Then add a **Gemfile** file at the root of the repository with this content:

```
source 'https://rubygems.org'
gem 'pygments.rb'
gem 'github-pages'
```

Finally run:

```sh
gem install bundler
bundle install
jekyll serve
```

And browse http://127.0.0.1:4000/

#### Docker FTW

You can render the current repository in a Docker container with this single command:

```
docker run --rm --label=jekyll --volume=$(pwd):/srv/jekyll -it -p 4000:4000 jekyll/jekyll
```

Then browse http://DOCKER_IP:4000/openpaas-tip-of-the-day/

# Tips list

- 07 - [How to ease your OpenPaas ESN development with docker-compose](https://github.com/linagora/openpaas-tip-of-the-day/blob/gh-pages/_posts/2016-2-23-docker-compose-dev.md)
- 06 - [How to get and set configurations in the ESN](https://github.com/linagora/openpaas-tip-of-the-day/blob/gh-pages/_posts/2016-2-16-esn-config.md)
- 05 - [How to contribute to mail server](https://github.com/linagora/openpaas-tip-of-the-day/blob/gh-pages/_posts/2016-2-6-contribute-to-mail-server.md)
- 04 - [How to use Dynamic Directives](https://github.com/linagora/openpaas-tip-of-the-day/blob/gh-pages/_posts/2016-2-5-dynamic-directives.md)
- 03 - [How to launch job in jobqueue](https://github.com/linagora/openpaas-tip-of-the-day/blob/gh-pages/_posts/2016-2-4-use-jobqueue-module.md)
- 02 - [How to mobile template](https://github.com/linagora/openpaas-tip-of-the-day/blob/gh-pages/_posts/2016-2-3-how-to-mobile-template.md)
- 01 - [Create an OpenPaas module](https://github.com/linagora/openpaas-tip-of-the-day/blob/gh-pages/_posts/2016-2-2-create-a-new-module.md)
