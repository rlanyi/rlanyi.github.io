---
layout: post
title: "Deal with temperv14 USB dongle errors like <code>USB interrupt read: Resource temporarily unavailable</code>"
category: Monitoring
---

A few days ago I got my first temperv14 device, which I planned to use with Check_MK to monitor temperature in my server room. After some experimenting I realized 
that sometimes it behaves strangely, the USB device (`/dev/bus/usb...`) seems to be unreachable.
For this reason, you have to reset the USB device and try a few times to get the valid temperature.

A few typical errors are:

```
USB interrupt read: Resource temporarily unavailable
Fatal error> USB read failed
```

```
Resetting USB device /dev/bus/usb/002/088
Error in ioctl: No such device
```

The following script solves these problems with [usbreset](https://askubuntu.com/questions/645/how-do-you-reset-a-usb-device-from-the-command-line).
([temperv14](https://github.com/silverfisk/home-automation/tree/master/temperv14) is also used, obviously.) It's also a Check_MK plugin and a modification of
[DMIR's Check_MK additions](https://bitbucket.org/dmir_wue/check_mk-additions/src/7f7abb5b1f4d?at=default) (see the original [here](https://bitbucket.org/dmir_wue/check_mk-additions/src/7f7abb5b1f4daeb6951167e387fd7f94fc87213b/agents/plugins/local-temp?at=default&fileviewer=file-view-default)).


{% highlight shell %}
#!/bin/bash
# for temperv14 see https://github.com/silverfisk/home-automation/tree/master/temperv14
echo "<<<local-temp>>>"

# we try to get the temp 10 times; see https://github.com/padelt/pcsensor-temper/issues/1
for i in `seq 1 10`; do
  if [ "$i" -ne "1" ]; then
    sudo usbreset $(lsusb | grep -i microdia | head -1 | awk {'print "/dev/bus/usb/" $2 "/" gensub(/\:$/, "", "g", $4)'}) > /dev/null 2>&1
    sleep 1
  fi
  RESULT=`sudo /usr/local/bin/temperv14 -c 2>/dev/null`
  if [ "$?" -eq "0" ];  then
    break;
  else
    RESULT="0.00"
  fi
done

echo $RESULT
{% endhighlight %}