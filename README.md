[CV Analyze](http://addons.heroku.com/cvanalyze) is an [add-on](http://addons.heroku.com) for analyzing CVs.

CV Analyzing (also known as Resume Analyzing, CV Parsing, Resume Parsing, CV Extraction, Resume Extraction) is the conversion of free-form resume into structured information suitable for storage, reporting and manipulation by a computer.

CV Analyze simultaneously and automatically converts every CV/social media profile into a complete candidate profile in your own system, saving up to 99.9% time in the administrative processing of applications.

CV Analyze is accessible via a restful API and has supported client libraries for Ruby, Python, Java, Node.js, Scala, Play!, Grails, Clojure...etc.

## Provisioning the add-on

CV Analyze can be attached to a Heroku application via the  CLI:

<div class="callout" markdown="1">
A list of all plans available can be found [here](http://addons.heroku.com/cvanalyze).
</div>

    :::term
    $ heroku addons:add cvanalyze
    -----> Adding cvanalyze to sharp-mountain-4005... done, v18 (free)

Once CV Analyze has been added a `CVANALYZE_URL` setting will be available in the app configuration and will contain the canonical URL used to access the newly provisioned CV Analyze service instance. This can be confirmed using the `heroku config:get` command.

    :::term
    $ heroku config:get CVANALYZE_URL
    http://www.cvanalyze.com/resources/7761b044e6ff01ddcec0e7f2e66d2bf137553eebb76a3f5248ecb68e2e2d0e3f

After installing CV Analyze the application should be configured to fully integrate with the add-on.

## Local setup

### Environment setup

After provisioning the add-on it’s necessary to locally replicate the config vars so your development environment can operate against the service.

<div class="callout" markdown="1">
Though less portable it’s also possible to set local environment variables using `export CVANALYZE_URL=value`.
</div>

Use [Foreman](config-vars#local_setup) to reliably configure and run the process formation specified in your app’s [Procfile](procfile). Foreman reads configuration variables from an .env file. Use the following command to add the CVANALYZE_URL values retrieved from heroku config to `.env`.

    :::term
    $ heroku config -s | grep CVANALYZE_URL >> .env
    $ more .env

<p class="warning" markdown="1">
Credentials and other sensitive configuration values should not be committed to source-control. In Git exclude the .env file with: `echo .env >> .gitignore`.
</p>

### Service setup

## Using with HTML

    <form action="CVANALYZE_URL" method="post">
      <input name="content" value="刘春涛 男 28岁 南京大学 软件工程专业 6年工作经验"/>
      <input type="submit"/>
    </form>

## Using with Ruby/Rails

    #coding=utf-8

    require "rest_client"
    require "json"

    url = ENV["CVANALYZE_URL"]
    resume_content = "刘春涛 男 28岁 南京大学 软件工程专业 6年工作经验"

    response = RestClient.post url, content: resume_content
    candidate = JSON.load(response.to_str.force_encoding("utf-8"))
    puts JSON.pretty_generate(candidate).strip

## Using with Python / Django

    #coding=utf-8

    import os
    import urllib

    resume_content = '刘春涛 男 28岁 南京大学 软件工程专业 6年工作经验'
    params = urllib.urlencode({'content': resume_content})
    f = urllib.urlopen(os.environ['CVANALYZE_URL'], params)
    print f.read()

## Using with CSharp
    using RestSharp;
    ...
    var restClient = new RestClient(CVANALYZE_URL)
    var request = new RestRequest("", Method.POST);
    var resumeContent = "刘春涛 男 28岁 南京大学 软件工程专业 6年工作经验";
    request.AddParameter("content", resumeContent);
    var response = restClient.Execute(request);
    Console.WriteLine(response.content);

## Using other Languages

We provide standard restful interface. Just HTTP Post the content of the resume to CVANALYZE_URL using any programming language you like.

## Monitoring & Logging

Stats and the current state of CV Analyze can be displayed via the CLI.

    :::term
    $ heroku cvanalyze:command
    example output

CV Analyze activity can be observed within the Heroku log-stream.

    :::term
    $ heroku logs -t | grep 'cvanalyze'

## Migrating between plans

<div class="note" markdown="1">Application owners should carefully manage the migration timing to ensure proper application function during the migration process.</div>

Use the `heroku addons:upgrade` command to migrate to a new plan.

    :::term
    $ heroku addons:upgrade cvanalyze:newplan
    -----> Upgrading cvanalyze:newplan to sharp-mountain-4005... done, v18 ($49/mo)
           Your plan has been updated to: cvanalyze:newplan

## Removing the add-on

CV Analyze can be removed via the  CLI.

<div class="warning" markdown="1">This will destroy all associated data and cannot be undone!</div>

    :::term
    $ heroku addons:remove cvanalyze
    -----> Removing cvanalyze from sharp-mountain-4005... done, v20 (free)

## Support

All CV Analyze support and runtime issues should be submitted via on of the [Heroku Support channels](support-channels). Any non-support related issues or product feedback is welcome at http://en.cvanalyze.com/contact.