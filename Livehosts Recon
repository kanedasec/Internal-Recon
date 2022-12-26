#!/bin/bash

# Check if a file was provided as parameter
if [ $# -lt 1 ]; then
  echo "Error: missing file parameter"
  exit 1
fi

# Check if the provided file exists and is a regular file
if [ ! -f $1 ]; then
  echo "Error: $1 is not a regular file"
  exit 1
fi

# Read each line of the file and store the network ip range in a variable
while read -r line; do
  ip_range=$line
  #Run nmap ping scan on the network ip range and Filter the output to get only the live hosts
  printf "\n\n Scanning Live hosts on $ip_range"
  nmap_output=$(nmap -n -sn --unprivileged $ip_range -oG - | awk '/Up$/{print $2}')

  # Output the live hosts to a file
  echo "$nmap_output" >> ./"live_hosts.txt"
  printf "\n\n $ip_range Live hosts stored in file: live_hosts.txt\n\n Proceeding with the Recon of the following range\n\n"
done < $1

  # Read each line of the live hosts file and store the live host in a variable
  while read -r live_host; do
  printf "\n\n Scanning $live_host\n\n"
  nmap -sV -O --script vulners --script-args mincvss=7.0 -e cscotun0 --top-ports 1000 -oG - $live_host >> Fullscan.txt
  done < live_hosts.txt
