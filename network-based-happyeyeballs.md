# Network-based Happy eyeballs :Better connectivity with IPv6/IPv4 Performance Measurement by Network operators
 
## Introduction & problem statement

Happy Eyeballs(HE) in RFC8305 provide an approach to enable clients to attempt multiple connections in parallel. It is helpful to work around the blocked, broken, or sub-optimal network. Taken IPv6 priority consideration in design, HE helps increase IPv6 traffic. However, in the early phase of IPv6 deployment, content and application providers still hesitate to migrate their application to IPv6 by implementing HE due to several reasons.

Firstly, HE will add additional complexity to application development loop which brings uncertainties for both development and operation. Secondly, people without much experience on IPv6 operation may have fears on performance degradation which will cause them to lose their customer. The third, paralleled connections emitted by HE will produce larger volume of traffic which is not welcomed by mobile application developers who cares about the consumption of both power of handset and wireless traffic consumption in economic aspect. Although RC8305 proposes 50 ms for Resolution Delay and 250ms for Connection Attempt Delay, but it is argued that it is much desirable that those parameters could be tunable and adapted to network changes. It is hard for every application developer from small ICPs to achieve that.

Instead of requiring changes on client, this draft is intend to proposed an network-based Happy eyeballs approach to address the same problem targeted by Happy eyeballs(RFC8305) for dual-stack users. The rationale of this approach is simple that ISPs typically the mobile Network provider has more resource to do precise IPv6 performance measurement for the good of their users. They can filter the AAAA record if IPv6 performance is not as good as IPv4 below a certain threshold value. It can help the client to fall back faster to IPv4 if the users does not support HE. It also helps reduce the unnecessary traffic emitted by HE client. 

Note that the threshold value should be tunable by network provider to gain a better tradeoff between IPv6 performance and IPv6 priority practice. It is worth to mention that with network-based Happy eyeballs approach, the operational issues will not be hidden and ISP will be crystal clear about their IPv6 network performance and improve it with lots of information.

## High level description of Network-based Happy eyeballs

Figure 1 shows the high level diagram of Network-based Happy eyeballs. There is one key network element adding to current access network topology which does IPv6 and IPv4 transport measurement for various domains cached by the resolver. The metric of the transport performance is to be defined such RTT via UDP/TCP/DIG. The algorithm of comparing IPv6 and IPv4 is also to be defined which finally return a value as a decision telling resolver to filter AAAA record or not.  


                       +--------------------------------+
                       |                                |
                       |  v6/v4 Performance Measurement |
                       |                                |
                       +--------------+-----------------+
                                      |
                                      |  Domains with poor IPv6 connections
                                      |
                                      v
             No data      +-----------------------+
           <-----------+  |                       |
                          |     DNS Resolver      |
           +------------> |                       |
        AAAA for a domain +-----------------------+

Figure 1. High level of how Network-based Happy Eyeballs works

Note that some implementation consideration is to be discussed and defined later. For example whether active or passive measurement is more suitable for this case? Whether black-list or white-list is more suitable for this case? (in figure 1 shows the black-list approach).
