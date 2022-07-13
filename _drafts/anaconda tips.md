PATH is an environment variable that is a list of directories that contain executables. So when you execute a program it runs through all the directories in your PATH and executes the first one with the same name.


## Where is it installed?

The installation location will depend on your operating system and how it's installed.

You will have a directory called `Anaconda3`. Note that this will be different for Miniconda.


### Windows

It will be somewhere like:

`C:\Users\<USERNAME>\Anaconda3`

or 

`C:\Users\<USERNAME>\AppData\Local\Anaconda3`

### Linux

For linux, it's similar to mac so read that section for me.

It should be somewhere like `/home/<USERNAME>/Anaconda3`

### Mac

It will be somewhere like:

`/Users/<USERNAME>/Anaconda3`


If you install it with Homebrew:

`/opt/homebrew/anaconda3`

Also, might be in:

`/opt/anaconda3`

Sometimes conda will be in:

```
/Users/<USERNAME>/opt/anaconda3/bin/conda
/Users/<USERNAME>/opt/anaconda3/condabin/conda
```

## Finding it

Windows:

`where conda`

Unix:

`which conda`

## Adding to Path


sometimes this is placed in your path, sometimes not.

I recommend you place it in the front of your path so it picks up any calls to `python`.

There are pros and cons to putting it in your path. See [this SO answer](https://stackoverflow.com/questions/52664293/why-or-why-not-add-anaconda-to-path) for more.

Unix: 

I recommend you put it early in your PATH, like so:

`export PATH=/home/<USERNAME>/anaconda3/bin:$PATH`
export PATH="/Users/username/miniconda3/bin:$PATH"

