---
layout: default
title: Howto make a github course
notoc: 1
---

# Archive

The archive uses git submodules to refer to old instances of a course, a
submodule in git is a way of declaring a subdirectory of your project to refer
to another repository. You can read more about those in the manual pages or the
[git-scm book](https://git-scm.com/book/en/v2/Git-Tools-Submodules). Here I
will just list the important commands that are needed, look them up if you want
to know how they work.

In the following I will pretend that we will want to create an archive for the
course _workshop-howto_ that was given on 2019-10-09.

## Step 0: Clone your workshop repository

In case you haven't done this already make sure to have the workshop code on
your computer:

{% highlight bash %}
git clone git@github.com:NBISweden/workshop-howto.git
cd workshop-howto
{% endhighlight %}

*NB* Replace `workshop-howto.git` with whatever repository you have.


## Step 1: Create an archive branch in your workhop repository

When your course is finished, create a new branch that we will later use to
refer to this specific instance and then push it to github. I suggest you call
it something like `archive/<YEAR>-<MONTH>-<DAY>`:

{% highlight bash %}
git branch archive/2019-10-09
git push origin archive/2019-10-09
{% endhighlight %}


## Step 2: Clone the workshop-archive

(do this in some other directory from your workshop directory)

The code here also creates a new branch for your work that you can then later
submit as a Pull Request on the repository.

{% highlight bash %}
git clone --recurse git@github.com:NBISweden/workshop-archive.git
cd workshop-archive
git checkout -b feature/adding-workshop-howto-archive
{% endhighlight %}

Recurse makes sure that git also clones and fetches all other repositories
referenced by the archive.

If you want you can run a preview local version of the archive on your computer
by following the [Run offline](labs/run_offline) tutorial.


## Step 3: Add your newly created archive branch as a submodule

This is the general pattern:

{% highlight bash %}
git submodule add -b <ARCHIVE_BRANCH> -- <REPOSITORY> <LOCAL PATH>
git commit -m 'Added <WORKSHOP> submodule to the archive'
{% endhighlight %}

In our case it will look like this:

{% highlight bash %}
git submodule add -b archive/2019-10-09 -- https://github.com/NBISweden/workshop-archive.git workshop-archive/2019-10-09
git commit -m 'Added workshop-howto/2019-10-09 submodule to the archive'
{% endhighlight %}

> *IMPORTANT*: Use the `https` clone url and not the `git` one.

## Step 4: Add an entry to the index.md to refer to the course

Edit the file `index.md` to have a reference to your course, just follow the
pattern that is already there. Then commit it:

{% highlight bash %}
git add index.md
git commit -m 'Added workshop-howot/2019-10-09 to the index`
{% endhighlight %}

## Step 5: Push your changes and make a pull request


{% highlight bash %}
git push origin workshop-howto/2019-10-09
{% endhighlight %}

Replace _workshop-howto/2019-10-09_ with the name of the branch you created in
step 2.

Then go to [github](https://github.com/NBISweden/workshop-howto) and create a
pull request to the master branch. To get the PR reviewd quickly please notify
@viklund in the NBIS slack.
