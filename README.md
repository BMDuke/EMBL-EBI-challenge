sudo docker build ega_download_client/ -t ega_download

sudo docker run -it ega_download bash

sudo docker-compose run ega_download bash

## 2. Test python client has been installed correctly
```
pyega3 -h
```

## 3. 
Skip

## 4. List all the files for test dataset EGAD00001003338
```
pyega3 files EGAD00001003338
```

## 5. Find out the filename, size and checksum of the file EGAF00001753752
```
pyega3 files EGAD00001003338 2>&1 | tee data/md5sums.txt
echo -e 
grep "File ID" data/md5sums.txt 
grep "EGAF00001753752" data/md5sums.txt 
```

## 6. Download the file EGAF00005007330
```
pyega3 fetch EGAF00005007330 -c 5 --output-dir ./data
```

## 7. Download a paired cram and crai file 
```
## Check out what cram files there are and pick one
grep ".cram" data/md5sums.txt

## Lets pick NA19238 for .cram and .crai
pyega3 -c 5 fetch EGAF00001753740 --output-dir ./data
pyega3 -c 5 fetch EGAF00001753741 --output-dir ./data
```

## 8. Download a paired bam bai file using ~20-30 connections
```
## Check out what cram files there are and pick one
grep ".bam" data/md5sums.txt

## Lets pick NA19239 for .bam and .bai and use '-c 30'
pyega3 -c 30 fetch EGAF00001753754 --output-dir ./data
pyega3 -c 30 fetch EGAF00001753755 --output-dir ./data
```

# Running the FUSE client

## Checking out the help options
sudo docker-compose up ega_fuse -d
