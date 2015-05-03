# scientific-git

normal usage of git
advantages of using VCS
should not add binary data [blows up repository]

problems: analysis result files not kept
[why is this a problem? if scripts are kept, could just rerun -> but: takes time, etc.]

manual: rsync backups to server

github lfs: only load files that are checked out, stored outside repo
problems: 1gb traffic/month, confidential data?

ideal solution:
- keep all versions of code
- but also: keep all results + store them internally
- be able to access them with revisions

alternative: annex
- provides exactly that

```bash
git annex init
git annex untrust .
git remote add ebi ssh://ebi/path/to/repository # clone this before on the server
git annex trust ebi
```

this will tell git to keep a copy of all your binary files on the server, tied to the revision

```bash
add annex add myplot.pdf # use --force if they are filtered in your .gitignore
git commit -m "adding plot"
git push origin
```

after that, all data files are backed up to ebi; all code files go to github [+can be pulled at ebi]

when checkout out a revision, code+links to binary files are checked out
links may be broken, if they are need to copy files to local store:

```bash
# remove a file from the local store - this will never remove the last copy
git drop myplot.pdf
# get a file from a remote where it is available
git get myplot.pdf
```

if you like to save space on your local machine, you can move all cached files that are not currently used by any branch HEAD

```bash
git annex move --unused --to ebi
```
