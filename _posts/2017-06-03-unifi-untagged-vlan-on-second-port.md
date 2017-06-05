---
layout: post
title: How to configure untagged VLAN interface on the second port of a Unifi UAP-Outdoor+ AP?
---

{% highlight shell %}
BZ.v3.3.22# brctl show
bridge name	bridge id		STP enabled	interfaces
br0		ffff.24a43c98481d	no		eth0
							eth1
br0.384		ffff.24a43c98481d	no		ath0
							eth0.384
							eth1.384
br0.385		ffff.24a43c98481d	no		ath1
							eth0.385
							eth1.385
br0.388		ffff.24a43c98481d	no		ath2
							eth0.388
							eth1.388

BZ.v3.3.22# brctl delif br0 eth1
BZ.v3.3.22# brctl delif br0.384 eth1.384
BZ.v3.3.22# brctl delif br0.385 eth1.385
BZ.v3.3.22# brctl delif br0.388 eth1.388

BZ.v3.3.22# brctl show
bridge name	bridge id		STP enabled	interfaces
br0		ffff.24a43c98481d	no		eth0
br0.384		ffff.24a43c98481d	no		ath0
							eth0.384
br0.385		ffff.24a43c98481d	no		ath1
							eth0.385
br0.388		ffff.24a43c98481d	no		ath2
							eth0.388

BZ.v3.3.22# brctl addif br0.384 eth1 # THIS IS THE IMPORTANT PART

BZ.v3.3.22# brctl show
bridge name	bridge id		STP enabled	interfaces
br0		ffff.24a43c98481d	no		eth0
br0.384		ffff.24a43c98481d	no		ath0
							eth0.384
							eth1
br0.385		ffff.24a43c98481d	no		ath1
							eth0.385
br0.388		ffff.24a43c98481d	no		ath2
							eth0.388
{% endhighlight %}