
# ![GA Cog](https://ga-dash.s3.amazonaws.com/production/assets/logo-9f88ae6c9c3871690e33280fcf557f33.png) Express Full-Stack Deployment


#### What we aim to accomplish:
- Deploy to Heroku using a virtual instance of MongoDB


### Learning Objectives
By the end of this lesson, students will know how to:

- Deploy a full-stack application to Heroku.


### Full Stack Deployment:

To start things off, we just need to configure a few things:
  - If you don't have an account with heroku, you'll need to set one up.
    * You can go [here](https://www.heroku.com/) to set one up.
  - We'll need to configure our database so that we're not using a local instance of MongoDB.

  - We'll need to configure our environment variables including our Port variable

 
  
**First, we'll configure our Port variable like this:**

```js
// This is just how Heroku wants us to configure our app

const port = process.env.PORT || 3000;


app.listen(port, () => {
  console.log(`Server is locked and loaded on port ${port}`);
});

```




**Excellent, now we can set up our database!** :smiley:




#### We'll use [mLAB](https://mlab.com).


_mLab is a fully managed cloud database service featuring automated provisioning and scaling of MongoDB databases, backup and recovery, 24/7 monitoring and alerting, web-based management tools, and expert support. mLab's Database-as-a-Service platform powers hundreds of thousands of databases across AWS, Azure, and Google and allows developers to focus their attention on product development instead of operations._


`mLab` provides an excellent solution in terms of giving us a virtual instance of `MongoDB`.



### Let's make sure we can use our Heroku CLI

First we install the CLI like this:

```bash
npm install -g heroku
```

Now, we can test to make sure it was installed properly:

```bash
heroku --version
```
- As long as we didn't get an error, we can login like so:

```bash
heroku login
```
- Go ahead and enter your credentials.

- Let's create an app instance for Heroku first.

  - You can do that using the following command:

```bash
heroku create
```

- Perfect, you should have an app instance created now!



## There are two ways to create our virtual MongoDB


#### Option A
This option is the easiest option of the two, but there's a small trade off. Although Heroku does more work for us with this option, they will need to trust us even further.


- We're going to search for the `mLab MongoDB` Add-On, or here: https://elements.heroku.com/addons/mongolab


- Selecting the _addons_ account option requires what's known as initializing a `verified` account status, which requires having a credit card on file.

- Keep in mind that this is **FREE**.

- This is just Heroku's policy, so there's no other way around it.

- Go ahead and set up your db config variable:

```js

mongoose.connect(process.env.MONGODB_URI || 'mongodb://localhost:27017/yourDbName');

```

```bash
heroku addons:create mongolab:sandbox
```


- This command will set the Addon to mLab and choose the sandbox plan for us.


- Now you can run the config command if you'd like to see and take note of your db config variable value:


```bash
heroku config
```

[Here's the docs](https://devcenter.heroku.com/articles/mongolab)

#### Option B

This option requires more steps, but it makes it so that you won't need to enter a credit card.

- First, we'll need to go to mLab and set up an account:
[mLab](https://mlab.com)
  - I will demonstrate this for you.

- Once we set up an account, we'll need to create a sandbox database.

- Once our sandbox database is created, we'll need to create a database `user` account so `heroku` can connect to it:

  - It's recommended to name your `user` **heroku**
  - Now you can set your password

- The last thing you'll need to do is copy your database `URI` and head back to the terminal.

- You'll need to run this command to set your database `URI` config variable.

```git
heroku config:set MONGOLAB_URI=mongodb://heroku:<yourpassword>@ds117336.mlab.com:17336/messageapp
```

- Lastly, now go back to your express app and prep your db connection:

```js

mongoose.connect(process.env.MONGOLAB_URI || 'mongodb://localhost:27017/yourDbName');

```

Now you can `git` add and commit your stages.

## Deployment: all you need to do is run:

```git

git push heroku master

```

- Perfect, heroku will respond by reading your scripts and get everything set up configured for you.


- If you have an issue, you can also run: `heroku logs` to see your log history - if you haven't already gotten familiar with heroku already, this command can print valuable information that can help you debug issues with deployment


## Optionals

Perhpas you need to take care of keeping your environment variables a secret?

You can get started on that by installing this module:

[dot env](https://www.npmjs.com/package/dotenv)

_As early as possible in your application, require and configure dotenv._

```js
require('dotenv').config()
// Create a .env file in the root directory of your project. Add environment-specific variables on new lines in the form of NAME=VALUE. For example:

DB_HOST=localhost
DB_USER=root
DB_PASS=s1mpl3
```
That's it.

process.env now has the keys and values you defined in your .env file.

```js
const db = require('db')
db.connect({
  host: process.env.DB_HOST,
  username: process.env.DB_USER,
  password: process.env.DB_PASS
})_
```

**The above section was directly sourced from the dot env page of [npmjs.com](https://www.npmjs.com/package/dotenv)**




