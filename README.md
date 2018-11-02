Dupro - Duplicity Profile
=========================

Dupro (DU-plicity PRO-file) is a simple tool that allows you to create a simple
profile to easily run [Duplicity](http://duplicity.nongnu.org/) commands with
the same target and options.

So, let's say you setup a profile called `myserverbackup` that targets a
particular S3 bucket, using a particular encryption passphrase, and some other
options. You can then run:

```
# Do a full backup.
dupro myserverbackup full

# See the collection status.
dupro myserverbackup collection-status

# Remove backups older than 30 days.
dupro myserverbackup remove-older-than 30D
```

The sub-commands `full`, `collection-status` and `remove-older-than` aren't
anything special from dupro - those are just plain duplicity commands. See
[the man page](http://duplicity.nongnu.org/duplicity.1.html).

Creating a profile
------------------

Profiles live under `~/.dupro`, for example, if your profile was called
`myserverbackup`, it'd be a file called `~/.dupro/myserverbackup`.

Profiles are just bash scripts that set some variables that Dupro uses,
or export variables that go directly to Duplicity.

Here's an example that backs up the `/etc` directory to S3:

```
SOURCE=/etc
TARGET=s3://s3.amazonaws.com/myawesome-server-backup

# These options are passed on every command.
DUPLICITY_OPTIONS="--s3-use-new-style"

# These options are only passed on backup commands.
DUPLICITY_BACKUP_OPTIONS="--full-if-older-than 30D"

# These get passed directly to Duplicity. Different targets and encryption
# methods take different variables - please see the Duplicity docs.
export AWS_ACCESS_KEY_ID="xxx"
export AWS_SECRET_ACCESS_KEY="xxx"
export PASSPHRASE="xxx"
```

You just create the profile in your text editor - that's it!

Comparisons to other tools
--------------------------

Dupro was inspired by [Duply](https://duply.net/), which also allows you to
create profiles in order to run duplicity commands on the same target.

However, Duply doesn't let you directly run Duplicity commands - it has its
own commands. It also tries to do some additional things beyond what Duplicity
does, like exporting the GPG keys used for encryption and storing them with the
profile.

It's not as lightweight as Dupro, and is full of magic. Some of that magic is
cool, some of it I disagree with (like the GPG key thing).

There's a great list of other
[alternatives on duply.net](http://duply.net/#Alternatives) that I won't copy
here, but all of them add some sort of cleverness and heaviness.

I just want to run straight up Duplicity commands, but without having to
repeat the target and options, and that's all that Dupro does.

No magic. No cleverness. Lightweight.

