---
title: Start a TOR relay at home on a Raspberry Pi
date: 2020-10-11 13:45:49
tags:
---

I recently created a TOR relay on a raspberry pi at home. It sits quietly next to my router supporting free speech and privacy and costing me nothing daily.

Here's how I did it.

### 1. I bought a raspbery pi

I bought a kit kinda like [this](https://www.adafruit.com/product/3058) that has a charger, case, some heat syncs, an SD card, and some other stuff that is needed to do much with the pi.

### 2. It sat in a drawer for 4 years. 

I don't recommend following this step!

### 3. I finally decided to take it out and set it up.

It is easiest to do this if you have a usb keyboard/mouse and monitor with HDMI cable. I tried before with a headless setup, and while there are instructions for that, sometimes troubleshooting is easier if you can see the pi's homescreen.

### 4. Read a little about TOR relays

The "Deciding to run a relay" section on the [Tor Relay Guide](https://trac.torproject.org/projects/tor/wiki/TorRelayGuide) is a great resource to understand what you are doing and why.
 
### 5. Install an OS on the raspberry pi (if it doesn't have one already).

Plug it in to let it boot up if you're not sure. If it doesn't boot up, you'll probably need to write an OS to your SD card.

Here are instructions on [installing the OS](https://www.raspberrypi.org/downloads/), which you can make CLI only/headless later if you want.

If you would perfer a minimal installation, the first part of this [Headless tutorial](https://3os.org/raspberryPi/TOR-Pi/) will show you how to install the minimal OS if you're interested in that route. The rest of these instructions can also be followed using SSH on the cli.

### 6. Setup a static IP with your router

This is important for port forwarding, or how your going to get the TOR traffic through your router and to the raspberry pi. Here are instructions on [setting up a static IP on the raspberry pi's OK](https://pimylifeup.com/raspberry-pi-static-ip-address/).

Make sure you are reserving the IP for your raspberry pi on your router. I had an issue with this.  This will be different on different routers, so you should just google 'reserve static ip' for your particular router model.

Also make sure to test your internet connection after this step by opening a webpage, running `ping google.com`, or something else.

### 7. Open ports 443 and 8080 on your router and forward to the static ip of your raspbery pi.

This will again be different for different routers, so google 'port forwarding' for your router model.

### 8. Run these scripts

You could just follow the official Tor [documentation](https://trac.torproject.org/projects/tor/wiki/TorRelayGuide#Parttwo:technicalsetup), but these [scripts](https://www.linux.com/news/turn-your-raspberry-pi-tor-relay-node/) set up several other things for you like updating your system, ensuring automatic updates, and configuring the ports. Make sure you read through the `bootstrap.sh` after you clone that repo so that you exactly know what you are executing on your pi.

Make sure you follow the instructiosn about limiting the bandwidth, or the relay may take up all your bandwidth and make your network slow. The default settings already limit the bandwidth, but you may want to follow the instructions on the [scripts](https://www.linux.com/news/turn-your-raspberry-pi-tor-relay-node/) page to customize it for your connection.

Note: the address you add to the torrc file is published, so I used a [Firefox relay address](https://relay.firefox.com/accounts/profile/) instead of my main address.

### 9. Check the logs to make sure it is working!

If you `tail /var/log/tor/notices.log`, you should see

`Self-testing indicates your ORPort is reachable from the outside. Excellent. Publishing server descriptor.`

If you see another message, the service may still be coming up. If you see a notice that it can't reach the ports, then there is probably an issue with your port forwarding.

### 10. setup nyx

You can use Tor [Nyx](https://nyx.torproject.org/) to monitor Tor. In order for it to work, you'll need to add this to your `torrc` file

```
ControlPort 9051
CookieAuthentication 1
CookieAuthFileGroupReadable 1
DataDirectoryGroupReadable 1
```

And you'll need to [add your logged in user to the debian-tor group](https://tor.stackexchange.com/questions/21109/debian-tor-vs-user):

```
sudo usermod -a -G debian-tor your_default_user
```

Use `getent group debian-tor` to make sure the user is showing up in the groups. You will need to restart tor and possibly log in and out of the user.

### 12. Look up your relay in the registry

In about 3 hours, your relay should be listed in the registry. In the [Relay Search](https://metrics.torproject.org/rs.html), you can search by ip or nickname of your node.

[Here's mine!](https://metrics.torproject.org/rs.html#details/7DADCE22CFB595E235C55F9F285CFE440802BE65)

### 13. Probably set up SSH so you can log in to your pi and check on it

[Set up SSH on the raspberry pi](https://www.raspberrypi.org/documentation/remote-access/ssh/) so you can access it using `ssh pi@<static ip address>` from a terminal on your computer

[Generate a public key and copy it to the pi](https://3os.org/linux/SSH_Service_Security/#ssh_service_security)

When I was done, I also went into the raspberry pi settings and switched it to boot up to CLI instead of desktop, since I will be running it without a monitor from now on.rap



And that's it. I hope you try it out! Happy Torring!