= PyPassport v1.0.0

Before running the docker image you should write a script using pypassport (https://github.com/oroger/pypassport)

== Default script - 01.ReadDG1.py

A default script that reads the first datagroup (DG1) is provided as example in scripts/01.ReadDG1.py. 
If you want to use it please replace the MRZ with the one of the passport you gonna read.

== Build and run the image

In order to access the USB reader, the image must be start with "privileged" access to host (where the USB reader is located)

$ docker build .
$ docker run -it --privileged -v /dev/bus/usb:/dev/bus/usb <imageId>

Example:

	~/git/pypassport/docker$ docker build -t pypassport:1.0.0 .

	Sending build context to Docker daemon 4.608 kB
	Sending build context to Docker daemon 
	Step 0 : FROM ubuntu:14.04
	 ---> ab035c88d533
	Step 1 : ENV refreshed_at 20160415
	 ---> Using cache
	 ---> 7fc853da7bf2
	Step 2 : RUN apt-get update
	 ---> Using cache
	 ---> 5cf97e66b95e
	Step 3 : RUN apt-get install -y pcscd libpcsclite1 pcsc-tools libccid git wget swig usbutils
	 ---> Using cache
	 ---> 8e62de3bfeb0
	Step 4 : RUN apt-get install -y python python-imaging python-pyscard python-crypto python-pyasn1
	 ---> Using cache
	 ---> 3ad600360053
	Step 5 : RUN git clone https://github.com/oroger/pypassport.git
	 ---> Using cache
	 ---> 1efe2f27d9d3
	Step 6 : WORKDIR pypassport/pypassport/src
	 ---> Using cache
	 ---> d03a498da5e3
	Step 7 : ADD script.py ./script.py
	 ---> e3396e510fc5
	Removing intermediate container 0c8b7fbab87c
	Step 8 : ADD run.sh ./run.sh
	 ---> b60fbae5fe6f
	Removing intermediate container deee25c0d0b6
	Step 9 : CMD ./run.sh
	 ---> Running in 76f645544c88
	 ---> 2c15c78bff0f
	Removing intermediate container 76f645544c88
	Successfully built 2c15c78bff0f

	$ docker run -it --privileged -v /dev/bus/usb:/dev/bus/usb pypassport:1.0.0

	{'5F05': '8', '5F04': '9', '5F07': '6', '5F06': '8', '59': '150701', '5F03': 'P<', '5F02': '0', '5F5B': 'ROGER<<OLIVIER<VINCENT<MICHAEL<<<<<<<<<', '5F1F': 'P<BELROGER<<OLIVIER<VINCENT<MICHAEL<<<<<<<<<EI123456<9BEL8001018M1507010<<<<<<<<<<<<<<06', '53': '<<<<<<<<<<<<<<', '5F2C': 'BEL', '5F57': '800101', '5F28': 'BEL', '5F35': 'M', '5A': 'EI123456<'}

= Using the image from the repository

Use a volume to mount your scripts instead of the default scripts as follows :

	$ docker run -it --privileged -v /dev/bus/usb:/dev/bus/usb -v </absolute/path/to/your/scripts>:/pypassport/pypassport/src/scripts pypassport:1.0.0

	Result of the execution (stdout and stderr) will be shown on the screen in also kept in a file using the same name as the script with ".out" extension in the scripts directory.
	So 01.readDG1.py becomes 01readDG1.py.out.

	All .out files in the script directory are removed when running the image, do not count on that directory for backup.