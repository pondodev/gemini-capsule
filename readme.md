# gemini capsule

## dependencies
- [agate](https://github.com/mbrubeck/agate)
- python 3
- some gemini-compatible browser (i use [lagrange](https://github.com/skyjake/lagrange))

## usage

### remote host setup
you'll first need to set up a directory to hold your server data. the project folder is already set up to work as a local testing environment (more on this later) so you can simply copy the repo to your remote host if you want. my suggested structure though is as follows:
```
root/
|- run-server
|- kill-server
|- server-settings.conf
|- content/
    |- index.gmi
```

### config files
next you can set up your config files--both `deploy_config.py` and `server-settings.conf`. both of these are based off their relevant template files. the former should only be in your local environment, with the latter in both your local and remote environments.

note that it isn't necessary to use an ipv6 address, and as such can be safely excluded from `server-settings.conf` and `run-server`. (TODO: add an easier way to exclude this!)

### development
to be able to see your capsule, you will need to first deploy your content and then run the server.

to deploy the content simply run `deploy -l` to get a local deployment. this will process the contents of `working_content/` and dump the output into `content/`.

locally you should be able to run the server with the `run-server` script, which will launch `agate` as a background process and dump log contents into `server.log`. once the server is running you will be able to view your capsule within the gemini-compatible browser of your choice with either the ip or domain name provided in `server-settings.conf`.

### remote deployment
once you decide to deploy to your live capsule, simply run `deploy`. this will process your capsule content source and then upload it to your remote server

## current parser features

### domain interpolation
doing the following in your source `.gmi` file
```
=> gemini://<DOMAIN>
```
will interpolate to this when deployed locally
```
=> gemini://example.local
```
or this when deployed publicly
```
=> gemini://example.com
```

### hexdump display
honestly i just think this looks cool. if you do the following
~~~markdown
```hex
here is some text.
and here is some text that is significantly longer so that it might cover multiple lines.
```
~~~
it will be converted to this
```
0x0000: here is some text...............................................
0x0040: and here is some text that is significantly longer so that it mi
0x0080: ght cover multiple lines........................................
```

## TODO
- add an easier way to exclude ipv6 configuration
- add an upload only flag for the deploy script
- stop `run-server` from creating a lock when the server fails to launch
- make first time deployment a bit better
