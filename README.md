# Test repository for distributed DAM and darktable

This is a test implementation of distributed Digial Asset Management using git-annex
for managing RAW files across multiple locations and editing with darktable.

Locations (each a git repository):

### MacBook Pro [ davemac ]
- repo location: ~/Photography/gnx/testannex
- remotes
  - **hub** git@github.com:daveham/testannex.git (fetch)
  - **hub** git@github.com:daveham/testannex.git (push)
  - **photo5drive** /Volumes/Photos5/Photography/gnx/testannex (fetch)
  - **photo5drive** /Volumes/Photos5/Photography/gnx/testannex (push)
  - **photoSSDdrive** /Volumes/PhotoSSD/Photography/gnx/testannex (fetch)
  - **photoSSDdrive** /Volumes/PhotoSSD/Photography/gnx/testannex (push)

### WD USB Drive [ photo5drive ]
- repo location /Volumes/Photos5/Photography/gnx/testannex
- remotes
  - **davemac** /Users/dave/Photography/gnx/testannex (fetch)
  - **davemac** /Users/dave/Photography/gnx/testannex (push)
  - **origin** /Users/dave/Photography/gnx/testannex (fetch)
  - **origin** /Users/dave/Photography/gnx/testannex (push)
  - **photoSSDdrive** /Volumes/PhotoSSD/Photography/gnx/testannex (fetch)
  - **photoSSDdrive** /Volumes/PhotoSSD/Photography/gnx/testannex (push)

### Samsung USB SSD Drive [ photoSSDdrive ]
- repo location /Volumes/PhotoSSD/Photography/gnx/testannex
- remotes
  - **davemac** /Users/dave/Photography/gnx/testannex (fetch)
  - **davemac** /Users/dave/Photography/gnx/testannex (push)
  - **origin** /Users/dave/Photography/gnx/testannex (fetch)
  - **origin** /Users/dave/Photography/gnx/testannex (push)
  - **photo5drive** /Volumes/Photos5/Photography/gnx/testannex (fetch)
  - **photo5drive** /Volumes/Photos5/Photography/gnx/testannex (push)

### Creation Log
```shell
# -- on laptop --
mkdir testannex
cd testannex
git init
git annex init
cp ../testannex/.gitignore .
git add .
git commit -a -m "Setup ignores."
git config annex.largefiles "include=*.DNG or include=*.CR2"
git annex config --set annex.largefiles "include=*.DNG or include=*.CR2"

# -- add photo5 --
cd /Volumes/Photos5/Photography/gnx
git clone ~/Photography/gnx/testannex
cd testannex
git annex init "photo5 test"
git config annex.largefiles "include=*.DNG or include=*.CR2"

# link back to davemac
git remote add davemac ~/Photography/gnx/testannex

# link davemac to photo5drive
cd ~/Photography/gnx/testannex
git remote add photo5drive /Volumes/Photos5/Photography/gnx/testannex

# -- add photoSSD --
cd /Volumes/PhotoSSD/Photography/gnx
git clone ~/Photography/gnx/testannex
cd testannex
git annex init "photoSSD test"
git config annex.largefiles "include=*.DNG or include=*.CR2"

# link back to davemac and photo5drive
git remote add davemac ~/Photography/gnx/testannex
git remote add photo5drive /Volumes/Photos5/Photography/gnx/testannex

# link davemac to photoSSDdrive
cd ~/Photography/gnx/testannex
git remote add photoSSDdrive /Volumes/PhotoSSD/Photography/gnx/testannex

# link photo5drive to photoSSDdrive
cd /Volumes/Photos5/Photography/gnx/testannex
git remote add photoSSDdrive /Volumes/PhotoSSD/Photography/gnx/testannex
```

### Ingest with exiftool
Example:
```shell
# copy/arrange if file contains DateTimeOriginal (get all CR2 files and some XMP files)
exiftool -m -ext cr2 -ext xmp -if '$exif:DateTimeOriginal' \
  -o ~/dummy/ \
  '-Directory<DateTimeOriginal' \
  -d testannex/Nashville/%Y-%m-%d  \
  /Volumes/PhotoSSD/Photography/Imports/tmp

# copy/arrange by CreateDate if file does not contain DateTimeOriginal (get remaining XMP files)
exiftool -m -ext xmp -if 'not $exif:DateTimeOriginal' \
  -o ~/dummy/ \
  '-Directory<CreateDate' \
  -d testannex/Nashville/%Y-%m-%d  \
  /Volumes/PhotoSSD/Photography/Imports/tmp
```

### Useful git & git annex Commands
```shell
# add/commit files in current directory (in annex)
git annex add .
git commit -m "message about files added"

# Copy large files to a remote
git annex copy Nashville --to=photo5drive

# pull current state of remote
git annex sync photo5drive
```

### darktable
```shell
open -a darktable --args --library ~/Photography/dt/Test/library.db
```

### SmugMug CLI
```shell
smugcli sync --force ./ 'Test/Nashville'
```