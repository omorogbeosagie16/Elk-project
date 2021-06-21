#!/bin/bash
echo "New Scripts"
echo "Machine Type\n" $MACHTYPE
date
echo "IP ADDRESS" $(ip addr | tail -2 | head -1 | awk '{print $2}')
echo "HOSTNAME:" $HOSTNAME
systemd-resolve --status | head -2 | tail -1 | awk '{print $1,$2,$3}'
echo "FREE SPACE"
free -h
echo "Disk Usage"
df -H | head -2
echo "CPU INFO"
lscpu | grep CP
echo "Current user is" $(whoami)