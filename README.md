# How-to PyPI

##### This is a small document for myself on how to create a release on PyPI, as well as creating a tag and release on GitHub.

#### I'm not an expert, so I won't take any responsibility if something goes wrong. But feel free to correct me.

## Cleaning

What? You've already screwed up your first release with `irelease` like I recently did? 
The releases on PyPI should be fine but the tags on GitHub are probably not.
In my case `irelease` pushed my whole virtual environment, which are more than 5000 files alone.
This can get disruptive when working with the module after this incident. (That's also the reason why I wrote all this).
If you don't care about the history and tags of your repository, then you can follow my steps.
Otherwise you might want to take a look at [git-filter-repo](https://github.com/newren/git-filter-repo). But please don't ask me how to use it.


Clone the repository onto your local hard drive.
```bash
git clone github.com/<username>/<repository>
```
Delete the .git folder in your repository to start over and create a new repository:
```bash
git init -b main
```

Now let's add everything back into the repository and make our "first" commit:
(make sure you are using the correct git account and not some personal credentials like I used to)
```bash
git add .
git commit -m "init"
```

Now we need to overwrite the current repository.
Add a new remote repository:
```bash
git remote add origin github.com/<username>/<repository>
```

### ⚠️ DANGER ⚠️

If you're not sure what you're doing, you might want to create a clone of your repository.
The next steps will replace the entire repository.

```bash
git push origin main --force
```

If you have other branches, remove them like this:
```bash
git push --delete origin <branch>
```

Deleting tags and releases is easy on GitHub. Go to the releases and delete them before the tags. Then the tags.

#### And you're done.


## Before the storm

Now let's get ready for the next (or first) PyPI release.

Create the following files:

`setup.py`
```python3
from setuptools import setup

with open("README.md", "r") as f:
    long_description = f.read()



setup(
    name="your_module",  # no "-" allowed
    version="0.1.0",
    description="A short description of what your module/library does.",
    long_description=long_description,
    long_description_content_type="text/markdown",
    author="your name (or username)",
    author_email="your@email.com",  # may be optional
    url="URL to your website or the GitHub repo",
    project_urls={
        "Source": "URL to your source code",
    },
    install_requires=["requests>=2.30.0"],  # Try to specify the latest version or higher
    license="MIT",  # your license of choice
    packages=["your_module"])  # the actual location of your module.
```
Make sure that you have a README.md for the long description.
If you want a "-" in your name, I have no idea how that works. Maybe it pulls the name from the GitHub repo or just replaces "_" with "-", but seriously, no idea.


`your_module/__init__.py`
```python3
from module import StuffYouNeed  # Make sure you import everything you want to make accessible.

__version__ = "0.1.0"  # must be the same as in the setup.py
# There are other values you can set, but are optional
```

## Create a release on GitHub

Make sure that your files are up to date with the remote repository including the two new files.
Then create a tag like this:
```bash
git tag -a v0.1.0 -m 0.1.0
```
And don't forget to push.
```bash
git push origin v0.1.0
```

Then switch to GitHub, open the repository and do the following:
- Navigate to releases
- draft a new release
- select the tag you've just created
- use the same title as the tag

Everything else is up to you, but you can now click `Publish release` and you're done!

## Create a release for PyPI

First, let's install the only requirement:
```bash
pip install twine
```

Build your package by executing
```bash
python setup.py sdist
```
This command will create a dist directory containing a compressed archive of your package.

Then upload the files with twine
```bash
cd dist
twine upload *
```

It is going to ask for your PyPI login and after that, it is done.
Thanks for reading.
