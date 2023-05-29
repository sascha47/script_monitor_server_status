# script_monitor_server_status
his script will check if the servers in the file servers.csv are up or down every 10 seconds and write the results to updown.csv 


```

#!/usr/bin/python3
# Pinglib.py library returns an IP and the time of a ping after 10 second wait
# 2023155

# Globals
import subprocess
import sys
import csv
import time
from datetime import datetime




# checks server status and returns up or down depeneding on the return code
def check_server_status(server):
   # return True if subprocess.call(["ping", "-c", "1", server]) == 0 else print(server + "down")
    proc = subprocess.call(["ping", "-c", "1", server])
    if proc == 0:
        return "up"
    else:
       return "down"
    
    

# main function to define the start and run the pingthis function
def main():

    pingthis()

        
    # pingthis read from a file and write to a file for each ping 
def pingthis():  
# write to a file called updown
        with open("updown.csv", "w") as file:
            writer = csv.writer(file)
            while True: # this creates an infinite loop
                with open("server.csv", "r") as server_file: # this reads from a file
                    server_csv = csv.reader(server_file) 
                    for row in server_csv: # for each row in file
                        server = row[1].strip() # strip and colect the ip
                        timestamp = datetime.now().isoformat() # creat the time
                        status = check_server_status(server) # up or down is returned
                        server_status = [timestamp, server, status] # putt it all in a list
                        print(", ".join(server_status)) # print to cli
                        writer.writerow(server_status) # write to file
                    time.sleep(10)# wait ten seconds between pings






if __name__ == "__main__":
    main()







```
