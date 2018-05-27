# sshtunnel

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
          "host": "ec2-111-222-333-444.us-west-2.compute.amazonaws.com",
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
to port 8000 on `ec2-111-222-333-444.us-west-2.compute.amazonaws.com`,
logging into `aws` as `ec2-user` using the specified private key.
After which, pointing your browser at `http://localhost:9100` will act as
though it was pointing at `http://ec2-111-222-333-444.us-west-2.compute.amazonaws.com:8000`,
even though that port is not accessible directly.

If there are multiple params, all of the tunnels will be started.

Note: you can alias the host names in `~/.ssh/config`.

## Usage

List the tunnels:

    $ sshtunnel list
    test
    aws

Start a tunnel:

    $ sshtunnel start aws
    Starting AWS (17716)

Check what tunnels are running:

    $ sshtunnel running
    Running AWS (17716) since 13:04:37

Stop a tunnel:

    $ sshtunnel stop aws
    Terminating AWS (17716)

## Credits

The tunnels configuration file is inspired by the `gSTM` configuration file.

## TODO?

This script just runs the tunnel in the background and exists. It
could be structured to watch the tunnels and automatically restart
them.

No doubt there are aspects of ssh tunnels that I haven’t considered.
Feature requests accepted. Pull requests too.









