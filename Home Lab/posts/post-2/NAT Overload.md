# Configuring Port Address Translation on IOS

October 7 2025

This post discusses a bit about my learning journey, a bit about NAT, and then dives into Port Address Translation on Cisco IOS. Feel free to use the table of contents to jump around and find what you're after!

### Table of Contents

- [[#Introduction]]
- [[#What is NAT?]]
- [[#Configuration (legacy)]]
- [[#Configuration (modern)]]
- [[#Conclusion]]
- [[#Further Reading]]
### Introduction

When I first began trying to learn networking in earnest I brought home an old Cisco ISR 2900 router and plugged it into my home router on g0/0, configured the interface as a DHCP client, and then set up a network in g0/1 and a static default route expecting to have connectivity with the rest of my home network. I was flabbergasted when I did not.

The reason that it did not work was because my home router had no route to the network on the inside interface of the ISR 2900. I was able to figure this much out, but when I realized that my consumer-grade home router provided no way to configure a static route, I gave up.

I had taken Cisco networking in school, but the courses were poorly taught and moved at break-neck speed with no chance for students to really digest any important networking theory before being thrown into the deep end configuring real networks. Those courses turned out to be an exercise in raw survival rather than anything else.

One topic that was never covered was network address translation. This is an important and ubiquitous technology that I only learned when I took a CCNA course online a few years after finishing school. Had I understood NAT and known how to configure it the day I brought home an old router from work, I would have known what to do to provide connectivity between the inside network on g0/1 and the rest of my home network.

### What is NAT?

There are several types of network address translation, but in this post I am going to focus on a form of source NAT called Port Address Translation - a technology used everywhere in IPv4 networks.

Source NAT modifies the source IP address in the IPv4 header. Cisco uses the terminology of *inside local* and *inside global* addresses. The inside local address is the address of the LAN interface on the router - typically the interface that serves as the default gateway for a local network. The inside global address it the address on the outside interface - in your home network, this would be the WAN interface on your router, but it doesn't have to be a WAN interface. In port address translation, all traffic leaving the local network for some external destination has the source IP address translated to the address on the outside global interface. In your home network, this means your device will 'borrow' the public IP address on the WAN interface of your ISP-provided router to communicate out to hosts on the Internet. Put really simply, PAT is what allows your device with it's private, non-globally routable IP address to access the Internet using the public IP address on your home router.

Since all devices will have their traffic translated to the same inside global IP address, the router needs a way to determine which traffic is destined for which host on the inside network. This is where layer 4 ports come in. Each host on the inside network will send its traffic using a random ephemeral source port number, and the router tracks which traffic goes to which host using these port numbers. This is important, because without tracking the source port numbers that hosts are using, the router would have no way of knowing which host to send reply traffic back to.

If the previous discussion leaves you feeling a bit confused, I recommend watching Professor Messer's [short video](https://www.youtube.com/watch?v=UILwCNOC5EI) on this topic.

OK, enough jibber jabber. Let's re-create the problem I had with my ISR 2900 and the solution in EVE-NG.

### Configuration (legacy)

There are two ways to perform NAT - a 'legacy' method, and the new method. Without breaking Cisco's NDA, I will say that I was not tested on the new method of configuring NAT when I wrote the CCNA 300-201 exam in July of 2025. However, the new way of configuring NAT does solve certain problems and I was interested to learn about it when performing research for this blog post, so it does merit presentation.

To configure PAT using the legacy method, I do the following:

- Ensure the inside global and inside local interfaces are `up/up` and each have an IP address
- Ensure the LAN uses an RFC 1918 private addressing scheme with an appropriate prefix length
- Create a standard numbered ACL to define the traffic that should be translated
- Run `ip nat inside` in interface configuration mode on the inside interface
- Run `ip nat outside` in interface configuration mode on the outside interface
- Run `ip nat inside source list {list} interface {interface} overload` in global configuration mode
- Configure a static default route for all traffic destined for external addresses

![[post2_screenshot1.png]]

Aside from the inside and outside terminology, the most confusing aspect of NAT configuration for beginners will likely be the required ACL. Students are used to thinking of ACLs as a way to restrict traffic on interfaces, but in the case of NAT, the ACL is used to specify which traffic will be translated. ACLs are not generally relied upon for real security, but there are a number of areas where they are used to specify matching traffic for certain purposes.

If NAT overload is properly configured, then you should be able to ping an external IP address from the client device and see the translations on the router.

![[Pasted image 20251005201817.png]]
![[Pasted image 20251005201838.png]]

Curiously, pings from the virtual PC appear to time out, yet we can see that they are in fact being translated by the router. I'm not entirely sure the reason for this, but we can nonetheless verify that NAT is working correctly. Observe that when running `show ip nat translations` we can see that the inside local address `10.0.1.2` is being translated to the inside global address on the router and that each translation is being tracked using a random ephemeral source port selected by the router (note that ICMP operates at layer 3, so the client itself does not select a random source port. Instead, this is provided by the router). Why the pings timed out will be something I need to investigate further. When testing using trace route, this did not happen. Unfortunately I did not get a screenshot of this before I tore down the legacy NAT configuration (perhaps another NAT post is coming!).

### Configuration (modern)

If you are early on in your learning journey and your only experience is utilizing network emulators like Cisco Packet Tracer, then you may be surprised to find that after configuring NAT on real IOS a new interface appears called `NVI0`. In legacy NAT, the problem arises of when translation is applied: before or after routing. In certain situations, traffic could be routed, translated, and then routed again based on the translated address. Modern NAT using the NAT Virtual Interface (NVI) uses a more predictable order of operations, routing traffic first and then applying translation if needed. 

When configuring modern NAT, inside and outside interfaces are no longer specified. Instead, participating interfaces are configured with `ip nat enable`. An ACL is still used to match traffic for translation, but the command to configure NAT in global configuration mode no longer uses the inside and outside terminology. Instead, the command only specifies a source list and an interface to overload on.

![[Pasted image 20251005211423.png]]

Interestingly, I noticed that after removing the legacy NAT configuration from my router and reconfiguring to use the NVI, I no longer had the issue of pings mysteriously appearing to time out while still being translated.

![[Pasted image 20251005211553.png]]

To see the translations on the router, we can no longer use `show ip nat translations` but instead `show ip nat nvi translations.`

![[Pasted image 20251005211721.png]]

NAT using NVI is admittedly a weak point in my knowledge, and it's something I look forward to learning in greater detail during my CCNP studies.

### Conclusion

In this post I gave an overview of what NAT is and specifically what NAT overload, or port address translation is. NAT is used everywhere in IPv4 networks to conserve address space using RFC 1918 addresses. To my knowledge, NAT is largely unused and unnecessary in IPv6 networks, and IPv6 is indeed the future. However, IPv4 likely isn't going anywhere and the world will continue to use both for some time to come, meaning NAT continues to be an essential skill for network administrators.

### Further Reading

- [IP Addressing: NAT Configuration Guide, Cisco IOS Release 15M&T](https://www.cisco.com/c/en/us/td/docs/ios-xml/ios/ipaddr_nat/configuration/15-mt/nat-15-mt-book/iadnat-addr-consv.html)