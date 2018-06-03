s# sshtunnel

Script for managing SSH tunnels.

SSH tunnels are very useful. They allow you to establish a secure
connection to a port on one machine through a port on a different
machine.

The command line syntax for setting them up is a bit tedious so I
wrote a python script to manage them.

## ~/.sshtunnels

Configure your tunnels by creating a JSON file.

    {
      "tunnels": [
        {
          "name": "test",
          "host": "localhost",
          "login": "ndw",
          "privkey": "",
          "params": [
            {
              "port1": "8081",
              "host": "localhost",
              "port2": "9081"
            },
            {
              "port1": "8082",
              "host": "localhost",
              "port2": "9082"
            }
          ]
        },
        {
          "name": "AWS",
          "host": "ec2-1-2-3-4.amazonaws.com",
          "login": "ec2-user",
          "privkey": "/home/ndw/.ssh/AWS-EC2.pem",
          "params": [
            {
              "port1": "9100",
              "host": "localhost",
              "port2": "8000"
            }
          ]
        }
      ]
    }

Keys:

* `name`: the name of the tunnel (used to start/stop it)
* `host`: the host to which ssh will connect
* `login`: the login to use
* `privkey`: the private key to use
* `params`: an array of tunnel parameters
  * `port1`: the port on “this” end of the tunnel
  * `host`: the tunnel host
  * `port2`: the port on “the other” end of the tunnel

In other words, the “AWS” tunnel above will connect port 9100 on `localhost`
to port 8000 on `ec2-1-2-3-4.amazonaws.com`,
logging into `aws` as `ec2-user` using the specified private key.
After which, pointing your browser at `http://localhost:9100` will act as
though it was pointing at `http://ec2-1-2-3-4.amazonaws.com:8000`,
even though that port is not accessible directly.

If there are multiple params, all of the tunnels will be started.

Note: you can alias the host names in `~/.ssh/config`.

## Usage

List the tunnels:

    $ sshtunnel tunnels
    test
    aws

Start a tunnel:

    $ sshtunnel start aws
    Starting AWS (17716)

Check what tunnels are running:

    $ sshtunnel list
    AWS:
            localhost:9100 → ec2-1-2-3-4.amazonaws.com:8000 (pid: 1234)

Stop a tunnel:

    $ sshtunnel stop aws
    Terminating AWS (17716)

## Credits

The tunnels configuration file is inspired by the
[gSTM](https://sourceforge.net/projects/gstm/) configuration file.

## TODO?

This script just runs the tunnel in the background and exists. It
could be structured to watch the tunnels and automatically restart
them.

No doubt there are aspects of ssh tunnels that I haven’t considered.
Feature requests accepted. Pull requests too.

## Changes

### 1.1.0, 3 June 2018

* Changed default behavior from show ‘list of tunnels’ to show ‘list of running tunnels’
* Changed meaning of ‘list’ command to show ‘list of running tunnels’.
  The ‘running’ command remains, for backwards compatibility. (Like this has any users!)
* Added ‘tunnel’ command to show ‘list of tunnels’
* Added ‘restart’ command to restart tunnels listedin PIDFILE that no longer appear
  to be running.
* Improved the formatting of the output of the ‘list’ command.
* Changed format of PIDFILE

### 1.0.0, 27 May 2018

* Released
