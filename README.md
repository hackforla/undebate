# undebate
Not debates, but a place where voters can get to know their candidates and candidates can connect to voters

![November 5, 2019 San Francisco Districe Attorney Race](https://octodex.github.com/images/yaktocat.png)
https://res.cloudinary.com/hf6mryjpf/image/upload/c_scale,w_360/v1573682312/2019Nov5_San_Francisco_Districe_Attorney_rtexr1.png
See our MVP/demo of the November, 5, 2019 San Francisco District Attorney race at: https://undebate.herokuapp.com/san-francisco-district-attorney


Copyright 2019 EnCiv, Inc. This work is licensed under the terms described in license.txt


# getting started
You will need a github.com account, and a heroku.com account.

Install
you will need to install the following, if you do not already have them.
Git: https://git-scm.com/book/en/v2/Getting-Started-Installing-Git
Node.js: https://nodejs.org/en/download/
Heroku: https://devcenter.heroku.com/articles/heroku-cli

I use visual studio code, but you can use other environments, but you will need to be able to run git-bash terminal windows in your environment.
https://code.visualstudio.com/

go to your github account and login

open a git-bash shell - on VSC use Control-`
Control-Shift-P
in the input field type "Select Default Shell"
Choose "Git Bash"

    mkdir enciv
    cd enciv
    git clone https://github.com/EnCiv/undebate
    cd undebate
    npm install

Note - if you are using multiple accounts with heroku, make sure that on your browser you are logged into the account that you want to use on

This is where we add the MongoDB database.  You will be able to use this one database when you are running locally, and when you are running in the cloud.

    heroku create undebate-<something unique>
    heroku addons:create mongolab:sandbox

This is where we get the environment variable with the URI for that database and store it in your bash configuration file so you can use it when you run locally.  This string has a password in it and it should never be shared or commited to a repo.  The .gitignore file ignores .bashrc so it won't get pushed into a repo - just make sure it stays that way.

    echo "export MONGODB_URI="\"`heroku config:get MONGODB_URI`\" >> .bashrc

Now we will add Cloudinary - a content delivery network that had image and video manipulation features.  A CDN give you a place to store these things, and deliver them quickly over the internet.   Your node server would be slower.

    heroku addons:create cloudinary:starter

Again we get the environment variable with the URL for cloudinary and store it in your config file for local use.  There's a password in the string, so keep it secret.

    echo "export CLOUDINARY_URL="\"`heroku config:get CLOUDINARY_URL`\" >> .bashrc

Now we just tell  node and the application we are in development mode.  Later we will set it to production

    echo "export NODE_ENV=development" >> .bashrc
    heroku config:set NODE_ENV=development

Then to get all these environment variable set in your current instance of bash do this. But you won't have to it the next time you start up.

    source .bashrc

Now you should be able to run it.

    npm run dev

You should no be able to browser to localhost:3011/schoolboard-conversation and see an undebate.  This is running locally on our machine.  It's using webpack, where is really neat bacuase when you make changes to the source code, they will complile and be applied to the server and to the application in your browser.   You may still have to refresh your browser page though

You can use <Control>C to terminate the server

To run this in the cloud:

    git push heroku HEAD:master

Then you will be able to browse to https://undebate-<something unique>.herokuapp.com/schoolboard-conversation and see the same thing.

Then, to record your own part in the schoolboard conversation browser to: localhost:3011/schoolboard-conversation-candidate-recorder or https://undebate-<something unique>.herokuapp.com/schoolboard-conversation-candidate-recorder


# EMAIL Setup
Setting up email from the server is not required, and is kind of hard. For information on how to do it for g-suite see https://medium.com/@imre_7961/nodemailer-with-g-suite-oauth2-4c86049f778a
If you do, edit the .bashrc file and add these lines

    export NODEMAILER_SERVICE="gmail"
    NODEMAILER_USER="no-reply@yourdomain.com"
    NODEMAILER_SERVICE_CLIENT="XXXXXXXXXXXXXXXXXXXXX"
    NODEMAILER_PRIVATE_KEY="-----BEGIN PRIVATE KEY-----\nXXXXX........XXXXX\n-----END PRIVATE KEY-----\n

If you use Zoho, you can do it like this.

    export NODEMAILER_SERVICE="Zoho"
    NODEMAILER_USER="no-reply@yourdomain.com"
    NODEMAILER_PASS="xxxxxxxxx"

If you use some other service, or 'things change' as they always do, go to app/server/util/send-mail.js and address them, but don't break the above configurations
After you make changes to the .bashrc file you will need to heroku config:set them to get them to heroku








