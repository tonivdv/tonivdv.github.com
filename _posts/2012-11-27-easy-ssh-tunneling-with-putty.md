---
title: Easy SSH tunneling with Putty
author: Van de Voorde Toni
layout: post
permalink: /2012/11/27/easy-ssh-tunneling-with-putty/
shareaholic_disable_share_buttons:
  - 0
shareaholic_disable_open_graph_tags:
  - 0
dsq_thread_id:
  - 3828085698
categories:
  - database
  - DEVelopment EXPerience
  - General
  - mysql
  - Tips and Tricks
tags:
  - forwarding
  - localhost
  - mysql
  - port
  - port forwarding
  - putty
  - rabbitmq
  - remote
  - ssh
---
<img alt="" src="http://cs.payap.ac.th/bob/images/putty.png" title="Putty Logo" class="alignright" width="128" height="128" />Need to access a MySQL server, a RabbitMQ management server or any other (web) server configured on a dedicated/virtual server which is not (or you don&#8217;t want to) open to the outside world?

A simple SSH connection and Putty makes this possible:

**1. Requirements**

  * Putty SSH client (<a title="download" href="http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html" target="_blank">download</a>)
  * SSH access to a server

**2. Configure Putty**

Open the Putty Configuration client, and fill in the field &#8216;*Host Name (or IP address)*&#8216; with the IP or host name of your remote server. In the *Connection type* section you should select *SSH* which will set the port automatically to *22*.

[<img class="aligncenter size-full wp-image-1069" title="Putty IP configuration screen" src="http://www.devexp.eu/wp-content/uploads/2012/11/putty-ip-screen.png" alt="" width="466" height="448" />][1]

Once this is done, open the *Tunnels* section in the menu *Connection -> SSH*. In this section we are going to configure the port forwarding. Assume we want to access the RabbitMQ management web server which runs on the port 55672. We need to tell Putty to listen on a *Source port* (=local), and to forward it to the *Destination* (=remote). 

For this example we will configure putty to forward port *1234* to the remote port *55672*. 

Fill in the ports as displayed in following screen and then click on *Add*:

[<img src="http://www.devexp.eu/wp-content/uploads/2012/11/putty-ssh-tunnel-screen.png" alt="" title="Putty SSH tunnel screen" width="466" height="448" class="aligncenter size-full wp-image-1077" />][2]

If everything is correctly configured click *Open*. This will open a SSH connection with your remote server and at the same time the configured port forwarding. The major drawback is that you cannot see the port forwarding from that screen. If you also use putty to manage your remote server, you will not see the difference. Of course you can always configure the putty screen to be a different color (if you know alternative tricks, please share 😉 )

[<img src="http://www.devexp.eu/wp-content/uploads/2012/11/putty-open-connection-screen.png" alt="" title="Putty open connections screen" width="847" height="314" class="aligncenter size-full wp-image-1084" />][3]

While the putty connection screens remains open you should be able to access the RabbitMQ management web server by calling *http://localhost:1234* in your favorite browser.

[<img src="http://www.devexp.eu/wp-content/uploads/2012/11/putty-port-rabbitmq.png" alt="" title="Putty port to RabbitMQ" width="825" height="541" class="aligncenter size-full wp-image-1088" />][4]

As you can see the procedure is really simple and opens a lot of possibilities without having to open different ports on your remote server. 

Enjoy

 [1]: http://www.devexp.eu/wp-content/uploads/2012/11/putty-ip-screen.png
 [2]: http://www.devexp.eu/wp-content/uploads/2012/11/putty-ssh-tunnel-screen.png
 [3]: http://www.devexp.eu/wp-content/uploads/2012/11/putty-open-connection-screen.png
 [4]: http://www.devexp.eu/wp-content/uploads/2012/11/putty-port-rabbitmq.png