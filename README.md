# access_log-hitcounter

A complete python script for finding IP addresses higher than a given hit. Using switches. 

hitcounter.py is the python script for finding the IP address from which we recieved higher hits than a given number. 

---
This program takes arguments using the switches. the switches are -l/--log for specifying the logfile and -c/--count for specifying the minHit value(Default set to 2500). This also uses the logparser we created earlier [link](https://github.com/j4jewel/python-logparser)
---

### hitcounter.py

```sh
import sys
import os
import logparser
import argparse

parser = argparse.ArgumentParser()

parser.add_argument("-l",
                    "--log",
                    dest = "logfile",
                    help = "Path to access file",
                    required = True )

parser.add_argument("-c",
                    "--count",
                    dest = "counter",
                    help = "Minimum number of hits",
                    default = "2500")

arguments = parser.parse_args()

logFile = arguments.logfile
minHit = arguments.counter

usage = "Error!! Usage: python3 hitcounter.py -l /Path/To/Log/File -c MinHit"

if os.path.exists(logFile) and os.path.isfile(logFile) and minHit.isdigit():



    ipCounter = {}

    lineCounter = 0

    with open(logFile,"r") as fh:


        for logLine in fh:


            lineCounter = lineCounter + 1

            ip = logparser.parser(logLine)["host"]

            if ip not in ipCounter:

                ipCounter[ip] = 1

            else:

                ipCounter[ip] = ipCounter[ip] + 1

        for ip in ipCounter:

            hit = ipCounter[ip]

            if hit >= int(minHit):

                print("{:20} : {}".format(ip,hit))
        print()
        print("Total Line Processed : {}".format(lineCounter))
else:
    print(usage)

```

Usage Example
```sh
root@SDTEAM:~/jewel/python# python3 hitcounter.py -l /root/jewel/python/python_log/access_log -c 5000
184.75.214.66        : 8229
209.126.120.29       : 7118
64.237.55.3          : 7120
178.255.152.2        : 5509
96.47.225.18         : 7800
109.123.101.103      : 9869
76.164.194.74        : 5562
76.72.167.154        : 5961

Total Line Processed : 1160421

```
