# Satellites Above
Alexa skill to tell you the number of satellites overhead.

[This AWS Blog Post](https://developer.amazon.com/blogs/alexa/post/8e8ad73a-99e9-4c0f-a7b3-60f92287b0bf/new-alexa-tutorial-deploy-flask-ask-skills-to-aws-lambda-with-zappa) is a good introduction to creating an Alexa Skill with Zappa and Lambda.

To get started:
```
$ pip install flask-ask zappa requests awscli
```

I had previously created a `zappa-deploy` IAM user w/ `AdministratorAccess` privledges. I did add a `[zappa-deploy]` section with the appropriate keys to `~/.aws/credentials`.

```
$ zappa init
```

Mostly selected the defaults, although I did tell it to use the `zappa-deploy` config. This creates a `zappa_settings.json`. I don't track this file in git because it contains a sensitive environmental variable. 

What not to do is add it to `.gitignore` and remove it with

```
$ git rm --cached zappa_settings.json
```

as this will delete the file... which is wrong, because you need it for zappa to run.

I should have just done:

```
$ git update-index --assume-unchanged zappa_settings.json
```

I order to supply the N2YO api key in a marginally secure manner, I added it as an environment variable that is set via the `zappa_settings.json` ([instructions](https://github.com/Miserlou/Zappa#setting-environment-variables)):

```
        "environment_variables": {
            "N2YO_API_KEY": "KEYVALUE"
        }
```

To actually push your endpoint to production:

```
$ zappa deploy dev
```



# Troubleshooting

Kept getting errors like:

```
(satellite_pass) satellite_pass $ pip install flask-ask
Collecting flask-ask
  Using cached https://files.pythonhosted.org/packages/6a/f5/d4709ae94584a0b1541e9b52b2d25a8a1bdb6e2da9d6870f23fdd0523a30/Flask-Ask-0.9.8.tar.gz
    Complete output from command python setup.py egg_info:
    Traceback (most recent call last):
      File "<string>", line 1, in <module>
      File "/private/var/folders/fn/2wspq0zs4qx9s5wyct0k87bc0000gn/T/pip-install-pbzq0cuf/flask-ask/setup.py", line 8, in <module>
        from pip.req import parse_requirements
    ModuleNotFoundError: No module named 'pip.req'

    ----------------------------------------
Command "python setup.py egg_info" failed with error code 1 in /private/var/folders/fn/2wspq0zs4qx9s5wyct0k87bc0000gn/T/pip-install-pbzq0cuf/flask-ask/
```

Fixed it by specifically installing the latest versino of `flask-ask`:

`$ pip install flask-ask==0.9.7`


# References

* https://developer.amazon.com/blogs/alexa/post/8e8ad73a-99e9-4c0f-a7b3-60f92287b0bf/new-alexa-tutorial-deploy-flask-ask-skills-to-aws-lambda-with-zappa
* https://www.n2yo.com/api/
* https://github.com/dronir/N2YOtools
* https://data-dive.com/alexa-get-device-location-from-custom-skill
