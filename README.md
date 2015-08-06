# git-annex

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
git push origin git-annex:git-annex # this is required to transparently pull files (better solution?)
```

after that, all data files are backed up to ebi; all code files go to github [+can be pulled at ebi]

when checkout out a revision, code+links to binary files are checked out
links may be broken, if they are need to copy files to local store:

```bash
# get a file from a remote where it is available
git get myplot.pdf
# drop a file locally to save space
git annex move --unused --to ebi
```

(this might be better done as a commit hook if annex+ebi remote is avail?)
