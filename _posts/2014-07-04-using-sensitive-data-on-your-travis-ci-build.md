---
published: true
layout: post
title: Using sensitive data on your Travis CI build
date: 2014-07-14T00:00:00.000Z
type: post
tags: 
  - Continuous Integration
  - Functional Tests
---





You have programmed an amazing functional tests to validate your infrastructure service that connect on Google Docs.

These functional tests run perfect well on your development machine and **now you want to run them on Travis CI**, but how to do this **without reveal your Google's username and password worldwide?**

**Travis CI [Encription Keys](http://docs.travis-ci.com/user/encryption-keys) goes to the rescue!** With them you can encrypt your sensitive data and read them inside your tests running on Travis CI.

In this post I will show you a very simple and real sample of using encryption keys to read username and password from environment variables encripted on .travis.yml file

### Step 1:  Encrypting your environment variables
To perform the encryption using Travis CLI you will need to setup a Ruby environment on your dev machine. If you are using Windows and do not have a Ruby environment, the easiest way is use [RubyInstaller](http://rubyinstaller.org/) (don't be afraid, because it works very well...it's a fully automatic installation).

After the RubyInstaller finish his job, open the "Start Command Prompt with Ruby" and type:

```
travis encrypt GDataUsername=[your username] -r [owner/repository]
travis encrypt GDataPassword=[your password] -r [owner/repository]
```

### Step 2:  Adding your encrypted variables to the .travis.yml file
Open your .travis.yaml file and add the encrypted values from previous step to the file, like the sample below:

```
env:
   global:
      - secure: "The GDataUsername encrypted value"
      - secure: "The GDataPassword encrypted value"
```

The tabs are very important to the .yml file, so you should respect them. Here is [my real .yml file](https://github.com/skahal/Skahal.Infrastructure.Repositories/blob/master/.travis.yml) to help.

### Step 3:  Reading the enviroment variables on your functional test
Now you can read those environment variable in your code, the sample code below shows how to do this in C#:

```csharp
var username = Environment.GetEnvironmentVariable("GDataUsername");
var password = Environment.GetEnvironmentVariable("GDataPassword");
```

The values of username and password variables will be the decrypted values that Travis CI has set to you on the environment.

### Step 4: Testing on Travis CI
Commit your files to GitHub and take a look on Travis CI build log, if you have set everything ok, you should see lines as below on log:

```
$ export GDataUsername=[secure]
$ export GDataPassword=[secure]
```

### That's all!
Now your functional tests should run on your dev machine (don't forget to set the environment variables on it too) and on Travis CI as well.
