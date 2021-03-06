# IPBOT

This is a simple telegram chat bot to manage our [RaspberryPI-based WAN emulator](https://github.com/arubaiberia/piwem). The bot accepts a few commands described below.

## Commands

### ip

Enumerates all the IP addresses of the device. The IP address of interface **eth0** is the uplink IP address, you can use it to ssh into the raspberry in case there is any problem.

### vlan

Send the command `vlan <VLAN_number>` to select a particular VLAN. The bot remembers the VLAN you selected, any order you send to the bot after this one will be applied to the selected VLAN.

### up

Send the command `up <delay(ms)> <jitter(ms)> <packet loss(%)> <correlation(%)>` to apply a particular delay, jitter and packet loss (percentage and correlation) to the vlan selected by the latest `vlan` command, in the **upstream** direction (traffic flowing from the VLAN to the uplink).

Only the *delay* parameter is mandatory. You can type just one, two or three parameters, or all four but set to 0 the things you don't want to use. for instance, if you want to set packet loss with no delay, send:

```
up 0 0 5 10
```

You can send `up 0` to remove all impairments in the upstream direction.

### in

This is a synonym of the `up` command. Kept for historical reasons.

### down

Send the message `down <delay(ms)> <jitter(ms)> <packet loss(%)> <correlation(%)>` to apply a particular delay, jitter and packet loss (percentage and correlation) to the vlan selected by the latest `vlan` command, in the **downstream** direction (traffic flowing from the uplink to the VLAN).

Only the *delay* parameter is mandatory. You can type just one, two or three parameters, or all four but set to 0 the things you don't want to use. for instance, if you want to set packet loss with no delay, send:

```
down 0 0 5 10
```

You can send `down 0` to remove all impairments in the downstream direction.

### out

This is a synonym of the `down` command. Kept for historical reasons.

## Chaining commands

You can chain several commands in the same message, e.g.

- Set 100 ms delay and 10ms jitter upstream in vlan 4094: `vlan 4094 up 100 10`
- Set 200 ms delay (100 upstream, 100 downstream) in vlan 4093: `vlan 4093 up 100 down 100`
- Clear all impairments in vlan 4094: `vlan 4094 up 0 down 0`

## Running the bot

To run the bot, you have to provide the telegram API key, either:

- In the command line, with the "-token" parameter.
- In the environment variable IPBOT_API_KEY

If you installed the bot by using the bootstrap script in the [piwem project](https://github.com/arubaiberia/piwem) and provided your key during installation, then don't worry. The script created a service to start the application on boot, and saved the API key for you.

If you didn't provide the API key during installation or want to change it afterwards, please check the README at [piwem](https://github.com/arubaiberia/piwem)
