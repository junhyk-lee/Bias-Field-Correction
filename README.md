Bias Field Correction with Hampel random noise
============
by
Lee,Junhyeok
Kang,Junghwa



Abstract
============

> Objective and quantitative analysis using MRI is challenging due to factors such as field inhomogeneities and motion artifacts. MRI is not inherently quantitative, and the bias field, a low-frequency multiplicative field that varies smoothly, can cause inaccuracies in quantitative analysis. To achieve accuracy in measurement, the bias field needs to be estimated and corrected. This study investigates the impact of the bias field on the analysis of MR images, and deep learning based approach to correct the inhomogeneity for more reliable and reproducible results. Specifically, we propose a model of non gaussian denoising diffusion model. Investigating the data distribution prior to denoise, we have established Hampel mixture distribution of both Gaussian distribution and Cauchy distribution. Through the Forward process of diffusion, we add hampel random noise instead of gaussian random noise to converge that corrupted MRI data to its true distribution noise. Rather than denoising from the gaussian noise, the proposed method effectively corrects the inhomogeneity fields and generates MRI without such inhomogeneities. Statistically, it has been demonstrated to perform equally or better than N4 in both simulated and real datasets.


InstallAbstract
=======

To install, try out the following commands:

```bash
$ pip install papergit
```


Usage
=====
Web Interface
-------------

Paper-to-Git comes with a neat little Web Frontend written in Flask. You can run
that by using:

```bash
$ paper-git serve
```

If this is your first time, you will have to authorize usage of your dropbox
account. See more on that below.

Commandline
-----------

You can run `paper-git --help` in the console to print out the help text.

The first time you use `paper-git`, it will create a `var` directory in the
current working directory. This directory will hold the important files and
database related to `paper-git`.

On the first run of any commnad, like `paper-git list` will run the
authorization workflow for you to allow `paper-git` access to your Dropbox
account. Not that even if you allow, I cannot access anything in your account as
the authorization token is stored locally only on your machine.

The flow looks something like this:
```bash
$ paper-git list
1. Go to: https://www.dropbox.com/oauth2/authorize?client_id=<client_id>&response_type=code
2. Click "Allow" (you might have to log in first).
3. Copy the authorization code.
Enter the authorization code here: <authorization code>
```

This will store your authorization token in the user configuration file at
`var/etc/paper-git.cfg`.

If you want this configuration to persist from anywhere, just copy
`var/etc/paper-git.cfg` to `/etc/paper-git.cfg`.

After this, you can run `update command to pull the list of all the docs`:

```
$ paper-git update
```

Note that update command doesn't pull changes in the already exsting docs in the
database. To update an existing document in the database try:

```
$ paper-git update <doc-id>
```

You can list all the docs in the database using:

```
$ paper-git list --docs
```

or you can list all the folders using:

```
$ paper-git list --folder
```

You can also add a `Sync` object to a tie together a path in a git repo to a
Paper Folder so that they can be synchronized automatically.

```
$ paper-git add --repo <path/to/git/repo/root/> --path <path/in/git/repo> --folder <paper-folder-name>
```

Note that the `<paper-folder-name>` in the above command is
case-insensitive. Once you replace all the variables and the above command runs
successfully, you have added a new rule to sync the documents.

After you have add a new rule, now you can publish your documents to their
respective. `publish` command will copy the downloaded files to their new
destination along with some added metadata that most common static site
generators expect.

While the addition of metadata can be turned off if needed, it is turned on by
defualt for now.

```
$ paper-git publish <doc-id>
```

Note that this will only work for changes that have already been pulled down
using the `update` command.

You can play `paper-git` using an interactive shell by using the `shell`
command:

```bash
$ paper-git shell
```

This will a start an interactive `ipython` shell with an initialized `config`
object.

```python
In [1]: config
Out[1]: <paper_to_git.config.config.BaseConfig at 0x7f2ca4cd6cc0>
In [2]: config.db
Out[2]: <paper_to_git.database.BaseDatabase at 0x7f2ca4cd64a8>
```

You can play with the models and interact with the database:

```python
In [3]: from paper_to_git.models import PaperDoc
In [4]: for doc in PaperDoc.select():
   ...:     print(doc)
   ...:
1: Document <1>
2: Document <2>
3: Document <3>
```

You can also force sync of a Doc, which you have changed:

```python
In [5]: doc = PaperDoc.get_by_paper_id('<paper_doc_id>')
In [6]: doc.get_changes()
Update revision for doc <Doc> from 54 to 69
```
License:
========
This project is licensed under Apache License 2.0. Please see the LICENSE file for a
complete copy of license.
[0]: https://paper.dropbox.com
[1]: https://github.com/dropbox/dropbox-sdk-python#updating-api-specification
[2]: https://github.com/pypa/virtualenv
