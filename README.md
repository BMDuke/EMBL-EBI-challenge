# EMBL-EBI Challenge: Exoploring genome-phenome data with EGA
The EGA repo is a public archive of genetic variation data which is paired with its resulting phenotype. For more information, visit their website:
- https://ega-archive.org/
<!--- end of list --->
Here, we demonstrate their clients which are used to download and explore the data. There are two clients:
- download client: This is used to fetch datasets from the repository
- fuse client: This is used to explore the repository using FUSE to create a virtual file system
<!--- end of list --->

## Requirements
- Docker v20.10.0 and up 
- Docker-compose v2.5.0 and up (probably can get away with [v1.28.0+](https://docs.docker.com/compose/gpu-support/))

## Set up
Before you use the clients you need to build the docker images which can be done by:
```
sudo docker build ega_download_client/ -t download_client

sudo docker build ega_fuse_client/ -t fuse_client
```

## Exploring the data
To explore the data, we need to use the fuse client. So to fire this up we run 
```
sudo docker-compose run ega_fuse bash
```
### Starting the fuse client
Now we need to start the fuse client. For more information on how the fuse client can be used, check out the original docs [here](https://github.com/EGA-archive/ega-fuse-client#running-the-project). To simplify this interface, I have made an alias for these commands in the [dockerfile](https://github.com/BMDuke/EMBL-EBI-challenge/blob/main/ega_fuse_client/Dockerfile), so that the fuse client can be used as follows
```
fuse_start                  # start the client
fuse_stop                   # stop the client
fuse_restart                # restart the client
```
This automatically handles the user credentials and any configuration settings. To start the process run `fuse start`. 

### Exploring the files with the FUSE client
The virtual filesystem is mounted at `/tmp/mnt`, so we need to go there in order to be able to explore the filesystem 
```
cd /tmp/mnt/
```
Initially, there will be nothing there in the directory. To make the virtual FS visible, we need to list its contents
```
ls
```
And we can do the same on its subdirectories etc... until we find the files we are looking for 
```
cd EGAD00001003338/
ls
```
For more information, see the original [docs](https://github.com/EGA-archive/ega-fuse-client#making-use-of-the-mounted-environment).

### Viewing and analysing the files
Supposing we want to have a quick peek at the files available to us. We can use `bcftools` to explore BCF and VCF files. First, lets find a good example file. We're looking for a .vcf file. 
```
bcftools view <filename>
```

### Copy a file to your local machine
If you want to copy a file to your local machine, you can do this with
```
cp <filename> /app/data/
```
This will locate the file in the `/data/` directory outside of the container. 


## Exploring the files with the python client
To explore the data, we need to use the python client. So to fire this up we run 
```
sudo docker-compose run ega_download bash
```
Full documentation for the python client can be found [here](https://github.com/EGA-archive/ega-download-client)

### Test installation was successful
Before we begin, lets introduce ourselves to the client and make sure that it was installed successfully.
```
pyega3 -h
```

### List all files in a directory
Now, lets list all the files that are available in the test dataset EGAD00001003338
```
pyega3 files EGAD00001003338
```
Note that datasets have a name encoded by EGS*D*.....


### Describe a file
Now, lets have a look inside that dataset and a describe a file. First, lets save the description in a file for us to parse.
```
pyega3 files EGAD00001003338 2>&1 | tee data/md5sums.txt
```
Then lets checkout one row of data.
```
echo -e 
grep "File ID" data/md5sums.txt 
grep "EGAF00001753752" data/md5sums.txt 
```
Here, we can see the file ID, the filesize, its name and its checksum. 

### Download a file
Now, to do some analysis you will probably want to download a file. Supposing we want to download the file EGAF00005007330. To do that run the command
```
pyega3 -c 5 fetch EGAF00005007330 --output-dir ./data
```
Note the `-c` flag tells the client how many connections we want to make. The `--output-dir` flag specifies the directory where we would like to save our data. This will resolve to `/data` in the project root, outside of the container. 
