# Docker Cronicle

My personal fork. Changes only to the Dockerfile, and they are:

- Updated to support Cronicle 0.9.59 : https://github.com/jhuckaby/Cronicle (Nothing was needed, just change the dockerfile ENV variable and fix the hardcoded URL)
- Added Python 3 including Pip. All my scripts are in Python.
- Added MySQL client. I wanted to run SQL backups and this makes it easy!
