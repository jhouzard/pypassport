FROM ubuntu:14.04

ENV refreshed_at 20160415

RUN apt-get update

# Install dependencies for USB Smartcard Reader
RUN apt-get install -y pcscd libpcsclite1 pcsc-tools libccid git wget swig usbutils 

# Install pypassport dependencies
RUN apt-get install -y python python-imaging python-pyscard python-crypto python-pyasn1 

# Checkout sources from Github
RUN git clone https://github.com/oroger/pypassport.git
WORKDIR pypassport/pypassport/src

# Add Docker run script to start the pcscd service before running the script
ADD script.py ./script.py
ADD run.sh ./run.sh
# Default entry point
CMD ./run.sh