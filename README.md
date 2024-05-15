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
Delete the .git folder as well as unwanted files/folders in your repository to start over and create a new repository:
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
This part is somewhat deprecated, instead of the setup.py I am going to use [pyproject.toml](https://pip.pypa.io/en/stable/reference/build-system/pyproject-toml/) with python -m [build](https://pypi.org/project/build)
I will update this document at some point to include releases on GitHub (not that hard, really), but first I have to figure out the new way of publishing packages.

### Create a workflow

Create a new file under `.github/workflows/release.yml`. For the file content itself you can use [release.yml from spotipy-anon](https://github.com/dieser-niko/spotipy-anon/blob/main/.github/workflows/release.yml).
Then go to your repository settings and create an environment called "release".

If you already have a project, follow [this tutorial](https://docs.pypi.org/trusted-publishers/adding-a-publisher/), but you can also follow [this tutorial](https://docs.pypi.org/trusted-publishers/creating-a-project-through-oidc/) if you don't have one yet.

### Create a new release

Go onto the releases site for your repository and create a new release and a tag with it. Make sure that the new version number is also mentioned in the `setup.py` file.
Hit create and wait until the workflow did its job.

Thanks for reading.
