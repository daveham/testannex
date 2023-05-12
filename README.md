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
