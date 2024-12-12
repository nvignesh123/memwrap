#!/usr/bin/env python3

import os
import psutil
import shlex
import signal
import sys
import subprocess
import time

"""
Description : Wrapper script to launch a command with a hard memory limit - Helps to kill the command if it crosses a memory threshold. Useful for Cloud VMS.

Available Environment Variables:
    MEMWRAP_LIMIT        -> Value in MB     -> Controls the max memory limit the command can use
    MEMWRAP_POLLING      -> Value in secs   -> Controls the sleep time to check for process info
    MEMWRAP_DEBUG        -> 0/1             -> Prints add. info
    #TODO MEMWRAP_TRACE        -> 0/1             -> Saves memory used by command
    #TODO MEMWRAP_TRACEFILE    -> Valid file path to save memory usage
    #TODO MEMWRAP_TRACKER      -> 0/1             -> Enable memory tracking of a different PID
    #TODO MEMWRAP_TRACKER_PID  -> Valid PID value 
"""

if "MEMWRAP_LIMIT" in os.environ:
    MEMWRAP_LIMIT = float(os.environ["MEMWRAP_LIMIT"])
else:
    MEMWRAP_LIMIT = 500 # in MBs

if "MEMWRAP_POLLING" in os.environ:
    MEMWRAP_POLLING = float(os.environ["MEMWRAP_POLLING"])
else:
    MEMWRAP_POLLING = 1 # in Seconds

if "MEMWRAP_DEBUG" in os.environ:
    MEMWRAP_DEBUG = int(os.environ["MEMWRAP_DEBUG"])
else:
    MEMWRAP_DEBUG = 0 # Int

if "MEMWRAP_TRACE" in os.environ:
    MEMWRAP_TRACE = int(os.environ["MEMWRAP_TRACE"])
else:
    MEMWRAP_TRACE = 0 # Int

if "MEMWRAP_TRACEFILE" in os.environ:
    MEMWRAP_TRACEFILE = os.environ["MEMWRAP_TRACEFILE"]
else:
    MEMWRAP_TRACEFILE = None


def get_memory_usage(pid):
    try:
        process = psutil.Process(pid)
        memory_info = process.memory_info()
        return memory_info.rss / (1024 * 1024)
    except psutil.NoSuchProcess:
        return 0

def main():
    cmd = " ".join(shlex.quote(arg) for arg in sys.argv[1:])
    process = subprocess.Popen(cmd, shell=True)
    cmd_pid = process.pid

    print(f"INFO: Started Command {cmd} with PID => {cmd_pid}")

    while True:
        if process.poll() is not None:
            print("INFO: Command has Finished Execution.")
            break

        memory_usage = get_memory_usage(cmd_pid)

        if memory_usage > MEMWRAP_LIMIT:
            print(f"FATAL: Memory usage of Command ({memory_usage:.2f} MB) has crossed the threshold ({MEMWRAP_LIMIT} MB). Killing Command.")
            os.kill(cmd_pid, signal.SIGKILL)
            break
        else:
            if MEMWRAP_DEBUG == 1:
                print(f"INFO: Memory usage of Command is within the limit: {memory_usage:.2f} MB / {MEMWRAP_LIMIT} MB")

        time.sleep(MEMWRAP_POLLING)
    
    sys.exit(process.wait())

if __name__ == "__main__":
    main()