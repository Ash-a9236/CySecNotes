<!--@ash-a9236 2025 : please see licence for -->

<!--VARIABLES-->

<style>

  @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&family=Fira+Code&display=swap');

  :root {
    --text: #DED9E2;
    --title: #80A1D4;
    --highlight: #75C9C8;
    --link: #b99eea;
  }

  body {
    font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
  }
</style>

[//]: # (<span style="color: var&#40;--text&#41;">)

# <span style="color: var(--title)">CYSEC 00</span>

## <span style="color: var(--title)">PASSIVE RECONNAISSANCE</span>

<br>
<hr>

### <a name="base-concepts"><span style="color: var(--title)">00.00 INTRODUCTION</span></a>

<hr>

<br>
Passive reconnaissance is a set of techniques where attackers gather information on their target <span style="color: var(--highlight)">without having direct contact</span>, or in other words, without actively engaging with the target's systems.

Typically passive reconnaissance techniques involve <span style="color: var(--highlight)">OSINT</span>, <span style="color: var(--highlight)">environmental assessments</span> (such as OS, software, tools, organisation configuration, used by the target), <span style="color: var(--highlight)">network examination</span> (such as DNS), and <span style="color: var(--highlight)">physical searches</span>.


To install all the commands on a linux system (debian-based) : 

```bash
sudo apt install whois host dig nslookup dnsrecon -y
```


ressources to test : 

```testphp.vulnweb.com```
```demo.testfire.net```
```scanme.nmap.org```
```zonetransfer.me```
```owasp-juice.shop```


<br>
<hr>

### <a name="whois"><span style="color: var(--title)">01.00 WHOIS</span></a>

<hr>

```whois``` is used to query <span style="color: var(--highlight)">WHOIS databases</span>, which are database of domain names, IP addresses, a network block, and autonomous systems (ASNs). The command returns all the public information available on the demanded information.

It is important to note that ```whois``` doesn't query one database but rather contact different distributed registries depending on the demand. It basically asks <span style="color: var(--highlight)">who is responsible for this internet resource ?</span> 

As a whole, the internet has basically <span style="color: var(--highlight)">3 separate registries</span> which organize the internet and keep track of everything on it : <span style="color: var(--highlight)">domain</span>, <span style="color: var(--highlight)">IP allocation</span>, and <span style="color: var(--highlight)">ASN / BGP routing</span>.

| SYSTEM            | MANAGED BY                                                                                                                                                                                               | EXAMPLE                                                                | 
|-------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------|
| Domain Registry   | - <span style="color: var(--link)">[ICANN](https://www.icann.org/)</a></span> <br>- Top-Level Domains (TLD) Registies <br>- Registrars                                                                   | .com<br>.org<br>.net                                                   |
| IP Allocation     | Regional Internet Registries (RIR) such as : <br><br>- AFRINIC (Africa)<br>- APNIC (Asia - Pacific)<br>- ARIN (American)<br>- LACNIC (Latin America)<br>- RIPE NNC (Europe - Middle East - Central Asia) | IPv4<br>IPv6<br>ISPs (internet service providers)<br>hosting providers |
| ASN \ BRP Routing | RIRs as well                                                                                                                                                                                             | -                                                                      | 


<br>
<hr>

#### <a id="domain-registries"><span style="color: var(--title)">01.01 DOMAIN REGISTRIES</a>

<hr><br>

Domain Registries are often widely known as DNS (Domain Name System) but are actually composed of Registries, Registrars, and DNS. 

Registries manage TLDs (Top-Level Domains) such as .com, .org, .gov, or even .ca, .fr, etc. Each registry manages *one* domain, which will then keep track of every domain which has its extension. For example, .org will keep track of wikipedia.org.

Registrars are companies which will sell an manage domain registrations for end users. They check the availability of a name in a registry and register the name of the client on its behalf for a price. GoDaddy and Namecheap are example of registrars. The relationship between registries and registrars is managed by the ICANN to ensure the system's reliability. 

Domain Name System is basically the phone book of the internet. It resolves the domain name searched by an end-user by finding the associated IP address. To do the domain name resolve, it will recursively seach for the answer through the DNS hierarchy (Root Nameserver -> TLD -> Authoritative Nameserver). It is important to note that when running ```whois``` on a domain name it does not queries DNS but rather the registrar's WHOIS server.

```
whois google.com
   Domain Name: GOOGLE.COM
   Registry Domain ID: 2138514_DOMAIN_COM-VRSN
   Registrar WHOIS Server: whois.markmonitor.com
   Registrar URL: http://www.markmonitor.com
   Updated Date: 2019-09-09T15:39:04Z
   Creation Date: 1997-09-15T04:00:00Z
   Registry Expiry Date: 2028-09-14T04:00:00Z
   Registrar: MarkMonitor Inc.
   Registrar IANA ID: 292
   Registrar Abuse Contact Email: abusecomplaints@markmonitor.com
   Registrar Abuse Contact Phone: +1.2086851750
   Domain Status: clientDeleteProhibited https://icann.org/epp#clientDeleteProhibited
   Domain Status: clientTransferProhibited https://icann.org/epp#clientTransferProhibited
   Domain Status: clientUpdateProhibited https://icann.org/epp#clientUpdateProhibited
   Domain Status: serverDeleteProhibited https://icann.org/epp#serverDeleteProhibited
   Domain Status: serverTransferProhibited https://icann.org/epp#serverTransferProhibited
   Domain Status: serverUpdateProhibited https://icann.org/epp#serverUpdateProhibited
   Name Server: NS1.GOOGLE.COM
   Name Server: NS2.GOOGLE.COM
   Name Server: NS3.GOOGLE.COM
   Name Server: NS4.GOOGLE.COM
   DNSSEC: unsigned
   URL of the ICANN Whois Inaccuracy Complaint Form: https://www.icann.org/wicf/
>>> Last update of whois database: 2026-05-26T19:54:02Z <<<

The Registry database contains ONLY .COM, .NET, .EDU domains and
Registrars.
Domain Name: google.com
Registry Domain ID: 2138514_DOMAIN_COM-VRSN
Registrar WHOIS Server: whois.markmonitor.com
Registrar URL: http://www.markmonitor.com
Updated Date: 2024-08-02T02:17:33+0000
Creation Date: 1997-09-15T07:00:00+0000
Registrar Registration Expiration Date: 2028-09-13T07:00:00+0000
Registrar: MarkMonitor, Inc.
Registrar IANA ID: 292
Registrar Abuse Contact: https://corp.markmonitor.com/domain/ui/abuse-report
Registrar Abuse Contact Phone: +1.2086851750
Domain Status: clientUpdateProhibited (https://www.icann.org/epp#clientUpdateProhibited)
Domain Status: clientTransferProhibited (https://www.icann.org/epp#clientTransferProhibited)
Domain Status: clientDeleteProhibited (https://www.icann.org/epp#clientDeleteProhibited)
Domain Status: serverUpdateProhibited (https://www.icann.org/epp#serverUpdateProhibited)
Domain Status: serverTransferProhibited (https://www.icann.org/epp#serverTransferProhibited)
Domain Status: serverDeleteProhibited (https://www.icann.org/epp#serverDeleteProhibited)
Registrant Organization: Google LLC
Registrant Country: US
Registrant Email: Select Request Email Form at https://domains.markmonitor.com/whois/google.com
Tech Email: Select Request Email Form at https://domains.markmonitor.com/whois/google.com
Name Server: ns2.google.com
Name Server: ns3.google.com
Name Server: ns4.google.com
Name Server: ns1.google.com
DNSSEC: unsigned
URL of the ICANN WHOIS Data Problem Reporting System: http://wdprs.internic.net/
>>> Last update of WHOIS database: 2026-05-26T19:50:47+0000 <<<

For more information on WHOIS status codes, please visit:
  https://www.icann.org/resources/pages/epp-status-codes

If you wish to contact this domain’s Registrant or Technical
contact, and such email address is not visible above, you may do so via our web
form, pursuant to ICANN’s Temporary Specification. To verify that you are not a
robot, please enter your email address to receive a link to a page that
facilitates email communication with the relevant contact(s).

Web-based WHOIS:
  https://domains.markmonitor.com/whois/contact/google.com

If you have a legitimate interest in viewing the non-public WHOIS details, send
your request and the reasons for your request to whoisrequest@markmonitor.com
and specify the domain name in the subject line. We will review that request and
may ask for supporting documentation and explanation.

The data in MarkMonitor’s WHOIS database is provided for information purposes,
and to assist persons in obtaining information about or related to a domain
name’s registration record. While MarkMonitor believes the data to be accurate,
the data is provided "as is" with no guarantee or warranties regarding its
accuracy.


MarkMonitor Domain Management(TM)
Protecting companies and consumers in a digital world.

Visit MarkMonitor at https://www.markmonitor.com
Contact us at +1.8007459229
In Europe, at +44.02032062220
--
```

From the query, we can see that : 

| LINE                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | INFORMATION                                                                                                                                                                                                    | 
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| DNSSEC: unsigned                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | There is no way for the DNS resolve or client to know if the returned IP address from the DNS query is correct (in instances of a cache-poisoning attack for example) since there is no DNSSEC key associated  |
| Creation Date: 1997-09-15T04:00:00Z                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      | It has been created on Sept 15th, 1997 : It is therefore long-standing and less possible it being a phishing or scam website                                                                                   |
| Name Server: NS1.GOOGLE.COM<br>Name Server: NS2.GOOGLE.COM<br>Name Server: NS3.GOOGLE.COM<br>Name Server: NS4.GOOGLE.COM                                                                                                                                                                                                                                                                                                                                                                                                                 | Google has 4 authoritative DNS servers                                                                                                                                                                         |
| Domain Status: clientDeleteProhibited https://icann.org/epp#clientDeleteProhibited<br>Domain Status: clientTransferProhibited https://icann.org/epp#clientTransferProhibited<br>Domain Status: clientUpdateProhibited https://icann.org/epp#clientUpdateProhibited<br>Domain Status: serverDeleteProhibited https://icann.org/epp#serverDeleteProhibited<br>Domain Status: serverTransferProhibited https://icann.org/epp#serverTransferProhibited<br>Domain Status: serverUpdateProhibited https://icann.org/epp#serverUpdateProhibited | Regular Information to prevent the name to be transfered or deleted                                                                                                                                            |
| Registrar: MarkMonitor, Inc.<br>Registrar IANA ID: 292                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | The registrar is MarkMonitor Inc. and they have the 292 ID at the IANA                                                                                                                                         |
| Registrant Organization: Google LLC                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      | The domain is owned by the Google corporation                                                                                                                                                                  |
| Registrant Country: US                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | It is located in the US                                                                                                                                                                                        |


```
whois hackthissite.org
    Domain Name: hackthissite.org
    Registry Domain ID: REDACTED
    Registrar WHOIS Server: http://whois.porkbun.com
    Registrar URL: https://porkbun.com
    Updated Date: 2025-07-15T23:08:30Z
    Creation Date: 2003-08-10T15:01:25Z
    Registry Expiry Date: 2026-08-10T15:01:25Z
    Registrar: Porkbun LLC
    Registrar IANA ID: 1861
    Registrar Abuse Contact Email: abuse@porkbun.com
    Registrar Abuse Contact Phone: +1.8557675286
    Domain Status: clientDeleteProhibited https://icann.org/epp#clientDeleteProhibited
    Domain Status: clientTransferProhibited https://icann.org/epp#clientTransferProhibited
    Name Server: c.ns.buddyns.com
    Name Server: f.ns.buddyns.com
    Name Server: g.ns.buddyns.com
    Name Server: h.ns.buddyns.com
    Name Server: j.ns.buddyns.com
    DNSSEC: unsigned
    URL of the ICANN Whois Inaccuracy Complaint Form: https://icann.org/wicf/
>>> Last update of WHOIS database: 2026-05-26T20:25:13Z <<<
```


| LINE                                                                                                                                                              | INFORMATION                                      | 
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------------------------------------------|
| Registrar: Porkbun LLC<br>Registrar IANA ID: 1861                                                                                                                 | The registrar is Porkbun                         | 
| Creation Date: 2003-08-10T15:01:25Z                                                                                                                               | It has been created on Aug 10th, 2003            |
| Name Server: c.ns.buddyns.com<br>Name Server: f.ns.buddyns.com<br>Name Server: g.ns.buddyns.com<br>Name Server: h.ns.buddyns.com<br>Name Server: j.ns.buddyns.com | The DNS servers responsible for hackthissite.org |



<br>
<hr>

#### <a id="ip-allocation"><span style="color: var(--title)">01.02 IP ALLOCATION REGISTRIES</a>

<hr><br>

Since there is not a lot of IPv4 addresses available, the public IP addresses are specifically assigned to different companies or organisations by IP blocks

```
whois 8.8.8.8

#
# ARIN WHOIS data and services are subject to the Terms of Use
# available at: https://www.arin.net/resources/registry/whois/tou/
#
# If you see inaccuracies in the results, please report at
# https://www.arin.net/resources/registry/whois/inaccuracy_reporting/
#
# Copyright 1997-2026, American Registry for Internet Numbers, Ltd.
#


NetRange:       8.8.8.0 - 8.8.8.255
CIDR:           8.8.8.0/24
NetName:        GOGL
NetHandle:      NET-8-8-8-0-2
Parent:         NET8 (NET-8-0-0-0-0)
NetType:        Direct Allocation
OriginAS:       
Organization:   Google LLC (GOGL)
RegDate:        2023-12-28
Updated:        2023-12-28
Ref:            https://rdap.arin.net/registry/ip/8.8.8.0



OrgName:        Google LLC
OrgId:          GOGL
Address:        1600 Amphitheatre Parkway
City:           Mountain View
StateProv:      CA
PostalCode:     94043
Country:        US
RegDate:        2000-03-30
Updated:        2019-10-31
Comment:        Please note that the recommended way to file abuse complaints are located in the following links. 
Comment:        
Comment:        To report abuse and illegal activity: https://www.google.com/contact/
Comment:        
Comment:        For legal requests: http://support.google.com/legal 
Comment:        
Comment:        Regards, 
Comment:        The Google Team
Ref:            https://rdap.arin.net/registry/entity/GOGL


OrgTechHandle: ZG39-ARIN
OrgTechName:   Google LLC
OrgTechPhone:  +1-650-253-0000 
OrgTechEmail:  arin-contact@google.com
OrgTechRef:    https://rdap.arin.net/registry/entity/ZG39-ARIN

OrgAbuseHandle: ABUSE5250-ARIN
OrgAbuseName:   Abuse
OrgAbusePhone:  +1-650-253-0000 
OrgAbuseEmail:  network-abuse@google.com
OrgAbuseRef:    https://rdap.arin.net/registry/entity/ABUSE5250-ARIN


#
# ARIN WHOIS data and services are subject to the Terms of Use
# available at: https://www.arin.net/resources/registry/whois/tou/
#
# If you see inaccuracies in the results, please report at
# https://www.arin.net/resources/registry/whois/inaccuracy_reporting/
#
# Copyright 1997-2026, American Registry for Internet Numbers, Ltd.
#
```


From the query, we can see that :

| LINE                                                                                                                                                                                                  | INFORMATION                                                                                               | 
|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------|
| NetRange:       8.8.8.0 - 8.8.8.255                                                                                                                                                                   | The IP 8.8.8.8 belongs to an IP block of 255 addresses                                                    |
| CIDR:           8.8.8.0/24                                                                                                                                                                            | The subnet mask of this IP is 255.255.255.0                                                               |
| NetName:        GOGL                                                                                                                                                                                  | The name of that IP block is GOGL                                                                         | 
| Organization:   Google LLC (GOGL)                                                                                                                                                                     | The IP block belongs to Google corporation                                                                |
| RegDate:        2000-03-30                                                                                                                                                                            | It has belonged to google since Dec 28th, 2023                                                            |
| OrgName:        Google LLC<br>OrgId:          GOGL<br>Address:        1600 Amphitheatre Parkway<br>City:           Mountain View<br>StateProv:      CA<br>PostalCode:     94043<br>Country:        US | The Address of the person at which the IP block belongs. It can sometimes be redacted for privacy reasons |
| OrgTechHandle: ZG39-ARIN                                                                                                                                                                              | That IP block has been allocated by ARIN, and therefore belongs to America                                |


```
whois 104.18.32.47

#
# ARIN WHOIS data and services are subject to the Terms of Use
# available at: https://www.arin.net/resources/registry/whois/tou/
#
# If you see inaccuracies in the results, please report at
# https://www.arin.net/resources/registry/whois/inaccuracy_reporting/
#
# Copyright 1997-2026, American Registry for Internet Numbers, Ltd.
#


NetRange:       104.16.0.0 - 104.31.255.255
CIDR:           104.16.0.0/12
NetName:        CLOUDFLARENET
NetHandle:      NET-104-16-0-0-1
Parent:         NET104 (NET-104-0-0-0-0)
NetType:        Direct Allocation
OriginAS:       
Organization:   Cloudflare, Inc. (CLOUD14)
RegDate:        2014-03-28
Updated:        2024-09-04
Comment:        All Cloudflare abuse reporting can be done via https://www.cloudflare.com/abuse
Comment:        Geofeed: https://api.cloudflare.com/local-ip-ranges.csv
Ref:            https://rdap.arin.net/registry/ip/104.16.0.0



OrgName:        Cloudflare, Inc.
OrgId:          CLOUD14
Address:        101 Townsend Street
City:           San Francisco
StateProv:      CA
PostalCode:     94107
Country:        US
RegDate:        2010-07-09
Updated:        2024-11-25
Ref:            https://rdap.arin.net/registry/entity/CLOUD14


OrgAbuseHandle: ABUSE2916-ARIN
OrgAbuseName:   Abuse
OrgAbusePhone:  +1-650-319-8930 
OrgAbuseEmail:  abuse@cloudflare.com
OrgAbuseRef:    https://rdap.arin.net/registry/entity/ABUSE2916-ARIN

OrgRoutingHandle: CLOUD146-ARIN
OrgRoutingName:   Cloudflare-NOC
OrgRoutingPhone:  +1-650-319-8930 
OrgRoutingEmail:  noc@cloudflare.com
OrgRoutingRef:    https://rdap.arin.net/registry/entity/CLOUD146-ARIN

OrgNOCHandle: CLOUD146-ARIN
OrgNOCName:   Cloudflare-NOC
OrgNOCPhone:  +1-650-319-8930 
OrgNOCEmail:  noc@cloudflare.com
OrgNOCRef:    https://rdap.arin.net/registry/entity/CLOUD146-ARIN

OrgTechHandle: ADMIN2521-ARIN
OrgTechName:   Admin
OrgTechPhone:  +1-650-319-8930 
OrgTechEmail:  rir@cloudflare.com
OrgTechRef:    https://rdap.arin.net/registry/entity/ADMIN2521-ARIN

RAbuseHandle: ABUSE2916-ARIN
RAbuseName:   Abuse
RAbusePhone:  +1-650-319-8930 
RAbuseEmail:  abuse@cloudflare.com
RAbuseRef:    https://rdap.arin.net/registry/entity/ABUSE2916-ARIN

RNOCHandle: NOC11962-ARIN
RNOCName:   NOC
RNOCPhone:  +1-650-319-8930 
RNOCEmail:  noc@cloudflare.com
RNOCRef:    https://rdap.arin.net/registry/entity/NOC11962-ARIN

RTechHandle: ADMIN2521-ARIN
RTechName:   Admin
RTechPhone:  +1-650-319-8930 
RTechEmail:  rir@cloudflare.com
RTechRef:    https://rdap.arin.net/registry/entity/ADMIN2521-ARIN


#
# ARIN WHOIS data and services are subject to the Terms of Use
# available at: https://www.arin.net/resources/registry/whois/tou/
#
# If you see inaccuracies in the results, please report at
# https://www.arin.net/resources/registry/whois/inaccuracy_reporting/
#
# Copyright 1997-2026, American Registry for Internet Numbers, Ltd.
#
```


| LINE                                                                                                                                      | INFORMATION                                                   | 
|-------------------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------|
| NetRange:       104.16.0.0 - 104.31.255.255                                                                                               | IP block range                                                |
| Organization:   Cloudflare, Inc. (CLOUD14)                                                                                                | Organisation name and ID                                      |
| Address:        101 Townsend Street<br>City:           San Francisco<br>StateProv:      CA<br>PostalCode:     94107<br>Country:        US | The organisation is located in the US                         |
| Ref:            https://rdap.arin.net/registry/entity/CLOUD14                                                                             | The IP block is allocated by ARIN, therefore it is in America |



```
whois 137.74.187.100

#
# ARIN WHOIS data and services are subject to the Terms of Use
# available at: https://www.arin.net/resources/registry/whois/tou/
#
# If you see inaccuracies in the results, please report at
# https://www.arin.net/resources/registry/whois/inaccuracy_reporting/
#
# Copyright 1997-2026, American Registry for Internet Numbers, Ltd.
#


NetRange:       137.74.0.0 - 137.74.255.255
CIDR:           137.74.0.0/16
NetName:        RIPE
NetHandle:      NET-137-74-0-0-1
Parent:         NET137 (NET-137-0-0-0-0)
NetType:        Early Registrations, Transferred to RIPE NCC
OriginAS:       
Organization:   RIPE Network Coordination Centre (RIPE)
RegDate:        2016-08-29
Updated:        2025-02-10
Comment:        These addresses have been further assigned to users in the RIPE NCC region. Please note that the organization and point of contact details listed below are those of the RIPE NCC not the current address holder. ** You can find user contact information for the current address holder in the RIPE database at http://www.ripe.net/whois.
Ref:            https://rdap.arin.net/registry/ip/137.74.0.0



OrgName:        RIPE Network Coordination Centre
OrgId:          RIPE
Address:        P.O. Box 10096
City:           Amsterdam
StateProv:      
PostalCode:     1001EB
Country:        NL
RegDate:        
Updated:        2013-07-29
Ref:            https://rdap.arin.net/registry/entity/RIPE

ReferralServer:  whois.ripe.net
ResourceLink:  https://apps.db.ripe.net/db-web-ui/query

OrgAbuseHandle: ABUSE3850-ARIN
OrgAbuseName:   Abuse Contact
OrgAbusePhone:  +31205354444 
OrgAbuseEmail:  abuse@ripe.net
OrgAbuseRef:    https://rdap.arin.net/registry/entity/ABUSE3850-ARIN

OrgTechHandle: RNO29-ARIN
OrgTechName:   RIPE NCC Operations
OrgTechPhone:  +31 20 535 4444 
OrgTechEmail:  hostmaster@ripe.net
OrgTechRef:    https://rdap.arin.net/registry/entity/RNO29-ARIN


#
# ARIN WHOIS data and services are subject to the Terms of Use
# available at: https://www.arin.net/resources/registry/whois/tou/
#
# If you see inaccuracies in the results, please report at
# https://www.arin.net/resources/registry/whois/inaccuracy_reporting/
#
# Copyright 1997-2026, American Registry for Internet Numbers, Ltd.
#



Found a referral to whois.ripe.net.

% This is the RIPE Database query service.
% The objects are in RPSL format.
%
% The RIPE Database is subject to Terms and Conditions.
% See https://docs.db.ripe.net/terms-conditions.html

% Note: this output has been filtered.
%       To receive output for a database update, use the "-B" flag.

% Information related to '137.74.187.96 - 137.74.187.127'

% Abuse contact for '137.74.187.96 - 137.74.187.127' is 'abuse@ovh.net'

inetnum:        137.74.187.96 - 137.74.187.127
netname:        OVH_113911647
descr:          OVH Static IP
country:        NL
org:            ORG-SH80-RIPE
admin-c:        OTC7-RIPE
tech-c:         OTC7-RIPE
status:         ASSIGNED PA
mnt-by:         OVH-MNT
created:        2016-08-25T08:53:54Z
last-modified:  2016-08-25T08:53:54Z
source:         RIPE

organisation:   ORG-SH80-RIPE
org-name:       Staff HackThisSite
org-type:       OTHER
address:        Stadtmitte 1
address:        10117 Berlin
address:        DE
phone:          +49.151011011
mnt-ref:        OVH-MNT
mnt-by:         OVH-MNT
created:        2016-07-28T19:32:04Z
last-modified:  2017-10-30T16:51:28Z
source:         RIPE # Filtered

role:           OVH NL Technical Contact
address:        OVH BV
address:        Corkstraat 46
address:        3047 AC Rotterdam
address:        The Netherlands
admin-c:        OK217-RIPE
tech-c:         GM84-RIPE
nic-hdl:        OTC7-RIPE
abuse-mailbox:  abuse@ovh.net
mnt-by:         OVH-MNT
created:        2009-03-18T15:51:01Z
last-modified:  2009-03-18T15:51:01Z
source:         RIPE # Filtered

% Information related to '137.74.0.0/16AS16276'

route:          137.74.0.0/16
origin:         AS16276
descr:          OVH
mnt-by:         OVH-MNT
created:        2016-07-15T10:03:53Z
last-modified:  2016-07-15T10:03:53Z
source:         RIPE

% This query was served by the RIPE Database Query Service version 1.122.1 (DEXTER)
```


| LINE                                                           | INFORMATION                                                                                             | 
|----------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|
| Found a referral to whois.ripe.net.                            | The IP is allocated by RIPE, therefore in EUROPE. whois then queried whois.ripe to get more information |
| netname:        OVH_113911647<br>descr:          OVH Static IP | The IP address is managed by OVH (a hosting provider)                                                   |
| org-name:       Staff HackThisSite                             | The IP address is rented to hackthissite organisation                                                   |


<br>
<hr>

#### <a id="asn"><span style="color: var(--title)">01.03 AUTONOMOUS SYSTEM NUMBER</a>

<hr><br>


The internet is not a network of computer but rather a <span style="color: var(--highlight)">network of networks</span>, which are themselves referred to as <span style="color: var(--highlight)">autonomous networks</span> or <span style="color: var(--highlight)">autonomous systems</span>. Within (private) networks, information can be carried by IPs and simple routing protocols which are all handled internally by the organisation managing the network. But, to reach the internet, or in other words, to get out of the private network to reach another private network, information (packets) need to <span style="color: var(--highlight)">identify uniquely</span> the network they come from as well as the network they are trying to reach. To resolve this issue, each network is labelled with a 2-byte number (sometimes 4-byte for newer allocations), which is the <span style="color: var(--highlight)">ASN</span>.

To route packets between ASes (Autonomous Systems), routers use BGP (Border Gateway Protocol) which is a type of routing protocol (see router protocols for more info).


```
whois AS15169

#
# ARIN WHOIS data and services are subject to the Terms of Use
# available at: https://www.arin.net/resources/registry/whois/tou/
#
# If you see inaccuracies in the results, please report at
# https://www.arin.net/resources/registry/whois/inaccuracy_reporting/
#
# Copyright 1997-2026, American Registry for Internet Numbers, Ltd.
#


ASNumber:       15169
ASName:         GOOGLE
ASHandle:       AS15169
RegDate:        2000-03-30
Updated:        2012-02-24
Ref:            https://rdap.arin.net/registry/autnum/15169



OrgName:        Google LLC
OrgId:          GOGL
Address:        1600 Amphitheatre Parkway
City:           Mountain View
StateProv:      CA
PostalCode:     94043
Country:        US
RegDate:        2000-03-30
Updated:        2019-10-31
Comment:        Please note that the recommended way to file abuse complaints are located in the following links. 
Comment:        
Comment:        To report abuse and illegal activity: https://www.google.com/contact/
Comment:        
Comment:        For legal requests: http://support.google.com/legal 
Comment:        
Comment:        Regards, 
Comment:        The Google Team
Ref:            https://rdap.arin.net/registry/entity/GOGL


OrgTechHandle: ZG39-ARIN
OrgTechName:   Google LLC
OrgTechPhone:  +1-650-253-0000 
OrgTechEmail:  arin-contact@google.com
OrgTechRef:    https://rdap.arin.net/registry/entity/ZG39-ARIN

OrgAbuseHandle: ABUSE5250-ARIN
OrgAbuseName:   Abuse
OrgAbusePhone:  +1-650-253-0000 
OrgAbuseEmail:  network-abuse@google.com
OrgAbuseRef:    https://rdap.arin.net/registry/entity/ABUSE5250-ARIN

RTechHandle: ZG39-ARIN
RTechName:   Google LLC
RTechPhone:  +1-650-253-0000 
RTechEmail:  arin-contact@google.com
RTechRef:    https://rdap.arin.net/registry/entity/ZG39-ARIN


#
# ARIN WHOIS data and services are subject to the Terms of Use
# available at: https://www.arin.net/resources/registry/whois/tou/
#
# If you see inaccuracies in the results, please report at
# https://www.arin.net/resources/registry/whois/inaccuracy_reporting/
#
# Copyright 1997-2026, American Registry for Internet Numbers, Ltd.
#

```


| LINE                     | INFORMATION                                   | 
|--------------------------|-----------------------------------------------|
| ASName:         GOOGLE   | The number is allocated to Google corporation |
| OrgTechHandle: ZG39-ARIN | It is owned by ARIN, therefore American       |


```
whois AS16509

#
# ARIN WHOIS data and services are subject to the Terms of Use
# available at: https://www.arin.net/resources/registry/whois/tou/
#
# If you see inaccuracies in the results, please report at
# https://www.arin.net/resources/registry/whois/inaccuracy_reporting/
#
# Copyright 1997-2026, American Registry for Internet Numbers, Ltd.
#


ASNumber:       16509
ASName:         AMAZON-02
ASHandle:       AS16509
RegDate:        2000-05-04
Updated:        2012-03-02
Ref:            https://rdap.arin.net/registry/autnum/16509



OrgName:        Amazon.com, Inc.
OrgId:          AMAZON-4
Address:        1918 8th Ave
City:           SEATTLE
StateProv:      WA
PostalCode:     98101-1244
Country:        US
RegDate:        1995-01-23
Updated:        2022-09-30
Ref:            https://rdap.arin.net/registry/entity/AMAZON-4


OrgRoutingHandle: ARMP-ARIN
OrgRoutingName:   AWS RPKI Management POC
OrgRoutingPhone:  +1-206-555-0000 
OrgRoutingEmail:  aws-rpki-routing-poc@amazon.com
OrgRoutingRef:    https://rdap.arin.net/registry/entity/ARMP-ARIN

OrgNOCHandle: AANO1-ARIN
OrgNOCName:   Amazon AWS Network Operations
OrgNOCPhone:  +1-206-555-0000 
OrgNOCEmail:  amzn-noc-contact@amazon.com
OrgNOCRef:    https://rdap.arin.net/registry/entity/AANO1-ARIN

OrgAbuseHandle: AEA8-ARIN
OrgAbuseName:   Amazon EC2 Abuse
OrgAbusePhone:  +1-206-555-0000 
OrgAbuseEmail:  trustandsafety@support.aws.com
OrgAbuseRef:    https://rdap.arin.net/registry/entity/AEA8-ARIN

OrgTechHandle: ANO24-ARIN
OrgTechName:   Amazon EC2 Network Operations
OrgTechPhone:  +1-206-555-0000 
OrgTechEmail:  amzn-noc-contact@amazon.com
OrgTechRef:    https://rdap.arin.net/registry/entity/ANO24-ARIN

OrgRoutingHandle: IPROU3-ARIN
OrgRoutingName:   IP Routing
OrgRoutingPhone:  +1-206-555-0000 
OrgRoutingEmail:  aws-routing-poc@amazon.com
OrgRoutingRef:    https://rdap.arin.net/registry/entity/IPROU3-ARIN

RTechHandle: AC6-ORG-ARIN
RTechName:   Amazon-com Incorporated
RTechPhone:  +1-206-555-0000 
RTechEmail:  ipmanagement@amazon.com
RTechRef:    https://rdap.arin.net/registry/entity/AC6-ORG-ARIN


#
# ARIN WHOIS data and services are subject to the Terms of Use
# available at: https://www.arin.net/resources/registry/whois/tou/
#
# If you see inaccuracies in the results, please report at
# https://www.arin.net/resources/registry/whois/inaccuracy_reporting/
#
# Copyright 1997-2026, American Registry for Internet Numbers, Ltd.
#
```


| LINE                             | INFORMATION         | 
|----------------------------------|---------------------|
| OrgName:        Amazon.com, Inc. | Allocated to Amazon |
| Country:        US               | Is in the US        |


<br>
<hr>

#### <a id="rdap"><span style="color: var(--title)">01.04 REGISTRATION DATA ACCESS PROTOCOL</a>

<hr><br>

```whois``` is slowly being replaced by ```rdap```, which provides the same data as whois, but in a JSON format which is nicer to read for computers and humans.

```
rdap 8.8.8.8
IP Network:
  Handle: NET-8-8-8-0-2
  Start Address: 8.8.8.0
  End Address: 8.8.8.255
  IP Version: v4
  Name: GOGL
  Type: DIRECT ALLOCATION
  ParentHandle: NET-8-0-0-0-0
  Status: active
  Port43: whois.arin.net
  Notice:
    Title: Terms of Service
    Description: By using the ARIN RDAP/Whois service, you are agreeing to the RDAP/Whois Terms of Use
    Link: https://www.arin.net/resources/registry/whois/tou/
  Notice:
    Title: Whois Inaccuracy Reporting
    Description: If you see inaccuracies in the results, please visit: 
    Link: https://www.arin.net/resources/registry/whois/inaccuracy_reporting/
  Notice:
    Title: Copyright Notice
    Description: Copyright 1997-2026, American Registry for Internet Numbers, Ltd.
  Entity:
    Handle: GOGL
    Port43: whois.arin.net
    Remark:
      Title: Registration Comments
      Description: Please note that the recommended way to file abuse complaints are located in the following links. 
      Description: To report abuse and illegal activity: https://www.google.com/contact/
      Description: For legal requests: http://support.google.com/legal 
      Description: Regards, 
      Description: The Google Team
    Link: https://rdap.arin.net/registry/entity/GOGL
    Link: https://whois.arin.net/rest/org/GOGL
    Event:
      Action: last changed
      Date: 2019-10-31T15:45:45-04:00
    Event:
      Action: registration
      Date: 2000-03-30T00:00:00-05:00
    Role: registrant
    vCard version: 4.0
    vCard fn: Google LLC
    vCard kind: org
    Entity:
      Handle: ABUSE5250-ARIN
      Port43: whois.arin.net
      Remark:
        Title: Registration Comments
        Description: Please note that the recommended way to file abuse complaints are located in the following links.
        Description: To report abuse and illegal activity: https://www.google.com/contact/
        Description: For legal requests: http://support.google.com/legal 
        Description: Regards,
        Description: The Google Team
      Remark:
        Title: Unvalidated POC
        Description: ARIN has attempted to validate the data for this POC, but has received no response from the POC since 2025-08-01
      Link: https://rdap.arin.net/registry/entity/ABUSE5250-ARIN
      Link: https://whois.arin.net/rest/poc/ABUSE5250-ARIN
      Event:
        Action: last changed
        Date: 2024-08-01T17:54:23-04:00
      Event:
        Action: registration
        Date: 2015-11-06T15:36:35-05:00
      Role: abuse
      vCard version: 4.0
      vCard fn: Abuse
      vCard org: Abuse
      vCard kind: group
      vCard email: network-abuse@google.com
      vCard tel: +1-650-253-0000
    Entity:
      Handle: ZG39-ARIN
      Status: validated
      Port43: whois.arin.net
      Link: https://rdap.arin.net/registry/entity/ZG39-ARIN
      Link: https://whois.arin.net/rest/poc/ZG39-ARIN
      Event:
        Action: last changed
        Date: 2025-11-10T07:10:31-05:00
      Event:
        Action: registration
        Date: 2000-11-30T13:54:08-05:00
      Role: technical
      Role: administrative
      vCard version: 4.0
      vCard fn: Google LLC
      vCard org: Google LLC
      vCard kind: group
      vCard email: arin-contact@google.com
      vCard tel: +1-650-253-0000
  Link: https://rdap.arin.net/registry/ip/8.8.8.0
  Link: https://whois.arin.net/rest/net/NET-8-8-8-0-2
  Event:
    Action: last changed
    Date: 2023-12-28T17:24:56-05:00
  Event:
    Action: registration
    Date: 2023-12-28T17:24:33-05:00
  cidr0_cidrs:
    v4prefix: 8.8.8.0
    length: 24
```

<br><br>

```
rdap hackthissite.org
Domain:
  Domain Name: hackthissite.org
  Domain Name (Unicode): hackthissite.org
  Status: client delete prohibited
  Status: client transfer prohibited
  Conformance: rdap_level_0
  Conformance: icann_rdap_response_profile_1
  Conformance: icann_rdap_technical_implementation_guide_1
  Conformance: redacted
  Notice:
    Title: Terms of Service
    Description: Public Interest Registry provides this RDAP service for informational purposes only, and to assist persons in obtaining information about or related to a domain name registration record. Public Interest Registry does not guarantee its accuracy. Users accessing the Public Interest Registry RDAP service agree to use the data only for lawful purposes, and under no circumstances may this data be used to: a) allow, enable, or otherwise support the transmission by e-mail, telephone, or facsimile of mass unsolicited, commercial advertising or solicitations to entities other than the registrar\'s own existing customers and b) enable high volume, automated, electronic processes that send queries or data to the systems of Public Interest Registry or any ICANN-accredited registrar, except as reasonably necessary to register domain names or modify existing registrations. When using the Public Interest Registry RDAP service, please consider the following: the RDAP service is not a replacement for standard EPP commands to the SRS service. RDAP is not considered authoritative for registered domain objects. The RDAP service may be scheduled for downtime during production or OT&E maintenance periods. Queries to the RDAP services are throttled. If too many queries are received from a single IP address within a specified time, the service will begin to reject further queries for a period of time to prevent disruption of RDAP service access. Abuse of the RDAP system through data mining is mitigated by detecting and limiting bulk query access from single sources. Where applicable, the presence of a [Non-Public Data] tag indicates that such data is not made publicly available due to applicable data privacy laws or requirements. Should you wish to contact the registrant, please refer to the RDAP records available through the registrar URL listed above. Access to non-public data may be provided, upon request, where it can be reasonably confirmed that the requester holds a specific legitimate interest and a proper legal basis for accessing the withheld data. Access to this data can be requested by submitting a request to WHOISrequest@pir.org. Public Interest Registry Inc. reserves the right to modify these terms at any time. By submitting this query, you agree to abide by this policy.
    Link: https://thenew.org/org-people/about-pir/policies/
  Notice:
    Title: Status Codes
    Description: For more information on domain status codes, please visit https://icann.org/epp
    Link: https://icann.org/epp
  Notice:
    Title: RDDS Inaccuracy Complaint Form
    Description: URL of the ICANN RDDS Inaccuracy Complaint Form: https://icann.org/wicf
    Link: https://icann.org/wicf
  Link: https://cart-before.porkbun.horse/rdap/domain/hackthissite.org
  Link: https://rdap.publicinterestregistry.org/rdap/domain/hackthissite.org
  Event:
    Action: transfer
    Date: 2025-07-10T23:08:27.812Z
  Event:
    Action: expiration
    Date: 2026-08-10T15:01:25.621Z
  Event:
    Action: registration
    Date: 2003-08-10T15:01:25.621Z
  Event:
    Action: last changed
    Date: 2025-07-15T23:08:30.836Z
  Event:
    Action: last update of RDAP database
    Date: 2026-05-27T02:09:06.634Z
  Secure DNS:
    Delegation Signed: false
    Max Signature Life: 1
  Entity:
    Handle: 1861
    Public ID:
      Type: IANA Registrar ID
      Identifier: 1861
    Link: https://rdap.publicinterestregistry.org/rdap/entity/1861
    Link: https://cart-before.porkbun.horse/rdap
    Role: registrar
    vCard version: 4.0
    vCard fn: Porkbun LLC
    Entity:
      Role: abuse
      vCard version: 4.0
      vCard tel: tel:+1.8557675286
      vCard email: abuse@porkbun.com
  Nameserver:
    Nameserver: c.ns.buddyns.com
    Nameserver (Unicode): c.ns.buddyns.com
    Handle: 27fa27bceec04e7ea929e8c42f501ca9-LROR
    Status: associated
  Nameserver:
    Nameserver: f.ns.buddyns.com
    Nameserver (Unicode): f.ns.buddyns.com
    Handle: adaa64f8592f4ef1bc258223f5d294e0-LROR
    Status: associated
  Nameserver:
    Nameserver: g.ns.buddyns.com
    Nameserver (Unicode): g.ns.buddyns.com
    Handle: 4e73c0b93f2144c387983d9a44714b0a-LROR
    Status: associated
  Nameserver:
    Nameserver: h.ns.buddyns.com
    Nameserver (Unicode): h.ns.buddyns.com
    Handle: ac80e806768e406b81ed789fa1fe573f-LROR
    Status: associated
  Nameserver:
    Nameserver: j.ns.buddyns.com
    Nameserver (Unicode): j.ns.buddyns.com
    Handle: 5b5068329f0a43929b715c5e96f3a21c-LROR
    Status: associated
  redacted:
    name:
      type: Registry Domain ID
    prePath: $.handle
    pathLang: jsonpath
    method: removal
```

<br><br>

```
rdap AS13335
Autnum:
  Handle: AS13335
  Name: CLOUDFLARENET
  Status: active
  StartAutnum: 13335
  EndAutnum: 13335
  Conformance: nro_rdap_profile_0
  Conformance: rdap_level_0
  Conformance: nro_rdap_profile_asn_flat_0
  Port43: whois.arin.net
  Notice:
    Title: Terms of Service
    Description: By using the ARIN RDAP/Whois service, you are agreeing to the RDAP/Whois Terms of Use
    Link: https://www.arin.net/resources/registry/whois/tou/
  Notice:
    Title: Whois Inaccuracy Reporting
    Description: If you see inaccuracies in the results, please visit: 
    Link: https://www.arin.net/resources/registry/whois/inaccuracy_reporting/
  Notice:
    Title: Copyright Notice
    Description: Copyright 1997-2026, American Registry for Internet Numbers, Ltd.
  Remark:
    Title: Registration Comments
    Description: All Cloudflare abuse reporting can be done via https://www.cloudflare.com/abuse
  Link: https://rdap.arin.net/registry/autnum/13335
  Link: https://whois.arin.net/rest/asn/AS13335
  Event:
    Action: last changed
    Date: 2017-02-17T18:04:32-05:00
  Event:
    Action: registration
    Date: 2010-07-14T18:35:57-04:00
  Entity:
    Handle: CLOUD14
    Port43: whois.arin.net
    Link: https://rdap.arin.net/registry/entity/CLOUD14
    Link: https://whois.arin.net/rest/org/CLOUD14
    Event:
      Action: last changed
      Date: 2024-11-25T11:09:46-05:00
    Event:
      Action: registration
      Date: 2010-07-09T14:10:42-04:00
    Role: registrant
    vCard version: 4.0
    vCard fn: Cloudflare, Inc.
    vCard kind: org
    Entity:
      Handle: CLOUD146-ARIN
      Status: validated
      Port43: whois.arin.net
      Link: https://rdap.arin.net/registry/entity/CLOUD146-ARIN
      Link: https://whois.arin.net/rest/poc/CLOUD146-ARIN
      Event:
        Action: last changed
        Date: 2025-08-20T23:29:26-04:00
      Event:
        Action: registration
        Date: 2021-07-01T14:10:58-04:00
      Role: routing
      Role: noc
      vCard version: 4.0
      vCard fn: Cloudflare-NOC
      vCard org: Cloudflare-NOC
      vCard kind: group
      vCard email: noc@cloudflare.com
      vCard tel: +1-650-319-8930
    Entity:
      Handle: ABUSE2916-ARIN
      Port43: whois.arin.net
      Remark:
        Title: Registration Comments
        Description: All Cloudflare abuse reporting can be done via https://www.cloudflare.com/abuse
      Remark:
        Title: Unvalidated POC
        Description: ARIN has attempted to validate the data for this POC, but has received no response from the POC since 2025-09-04
      Link: https://rdap.arin.net/registry/entity/ABUSE2916-ARIN
      Link: https://whois.arin.net/rest/poc/ABUSE2916-ARIN
      Event:
        Action: last changed
        Date: 2024-09-04T13:14:53-04:00
      Event:
        Action: registration
        Date: 2011-02-14T19:00:47-05:00
      Role: abuse
      vCard version: 4.0
      vCard fn: Abuse
      vCard org: Abuse
      vCard kind: group
      vCard email: abuse@cloudflare.com
      vCard tel: +1-650-319-8930
    Entity:
      Handle: ADMIN2521-ARIN
      Status: validated
      Port43: whois.arin.net
      Remark:
        Title: Registration Comments
        Description: All Cloudflare abuse reporting can be done via https://www.cloudflare.com/abuse
      Link: https://rdap.arin.net/registry/entity/ADMIN2521-ARIN
      Link: https://whois.arin.net/rest/poc/ADMIN2521-ARIN
      Event:
        Action: last changed
        Date: 2025-06-12T09:59:51-04:00
      Event:
        Action: registration
        Date: 2011-04-19T16:11:36-04:00
      Role: administrative
      Role: technical
      vCard version: 4.0
      vCard fn: Admin
      vCard org: Admin
      vCard kind: group
      vCard email: rir@cloudflare.com
      vCard tel: +1-650-319-8930
  Entity:
    Handle: ABUSE2916-ARIN
    Port43: whois.arin.net
    Remark:
      Title: Registration Comments
      Description: All Cloudflare abuse reporting can be done via https://www.cloudflare.com/abuse
    Remark:
      Title: Unvalidated POC
      Description: ARIN has attempted to validate the data for this POC, but has received no response from the POC since 2025-09-04
    Link: https://rdap.arin.net/registry/entity/ABUSE2916-ARIN
    Link: https://whois.arin.net/rest/poc/ABUSE2916-ARIN
    Event:
      Action: last changed
      Date: 2024-09-04T13:14:53-04:00
    Event:
      Action: registration
      Date: 2011-02-14T19:00:47-05:00
    Role: abuse
    vCard version: 4.0
    vCard fn: Abuse
    vCard org: Abuse
    vCard kind: group
    vCard email: abuse@cloudflare.com
    vCard tel: +1-650-319-8930
  Entity:
    Handle: ADMIN2521-ARIN
    Status: validated
    Port43: whois.arin.net
    Remark:
      Title: Registration Comments
      Description: All Cloudflare abuse reporting can be done via https://www.cloudflare.com/abuse
    Link: https://rdap.arin.net/registry/entity/ADMIN2521-ARIN
    Link: https://whois.arin.net/rest/poc/ADMIN2521-ARIN
    Event:
      Action: last changed
      Date: 2025-06-12T09:59:51-04:00
    Event:
      Action: registration
      Date: 2011-04-19T16:11:36-04:00
    Role: technical
    vCard version: 4.0
    vCard fn: Admin
    vCard org: Admin
    vCard kind: group
    vCard email: rir@cloudflare.com
    vCard tel: +1-650-319-8930
  Entity:
    Handle: NOC11962-ARIN
    Status: validated
    Port43: whois.arin.net
    Remark:
      Title: Registration Comments
      Description: All Cloudflare abuse reporting can be done via https://www.cloudflare.com/abuse
    Link: https://rdap.arin.net/registry/entity/NOC11962-ARIN
    Link: https://whois.arin.net/rest/poc/NOC11962-ARIN
    Event:
      Action: last changed
      Date: 2026-04-30T05:15:18-04:00
    Event:
      Action: registration
      Date: 2011-04-19T16:25:31-04:00
    Role: noc
    vCard version: 4.0
    vCard fn: NOC
    vCard org: NOC
    vCard kind: group
    vCard email: noc@cloudflare.com
    vCard tel: +1-650-319-8930
```


<br> <br>

<hr>

### <a id="host"><span style="color: var(--title)">02.00 HOST</a>

<hr><br>


```host``` command is used to replace a full DNS dig query (done through the ```dig``` command). host basically asks <span style="color: var(--highlight)">wha IP belongs to X domain ?</span>
It can also ask about a domain for an IP or a mail server for a domain.

At the opposite of ```whois```, ```host``` queries <span style="color: var(--highlight)">DNS records directly</span>.

It is important to note : 

| RECORD | TYPE         |
|--------|--------------|
| A      | IPv4 address |
| AAAA   | IPv6 address |
| MX     | Mail server  |
| NS     | Nameserver   |
| TXT    | Text records |
| CNAME  | Alias        |
| PTR    | Reverse DNS  |


<br>
<hr>

#### <a id="dig"><span style="color: var(--title)">02.01 DIG</a>

<hr><br>

For example, the dig command does a full DNS lookup for google.com

```
dig google.com

; <<>> DiG 9.20.23-1~deb13u1-Debian <<>> google.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 3647
;; flags: qr rd ra; QUERY: 1, ANSWER: 6, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 65494
;; QUESTION SECTION:
;google.com.			IN	A

;; ANSWER SECTION:
google.com.		28	IN	A	142.250.137.101
google.com.		28	IN	A	142.250.137.102
google.com.		28	IN	A	142.250.137.138
google.com.		28	IN	A	142.250.137.139
google.com.		28	IN	A	142.250.137.100
google.com.		28	IN	A	142.250.137.113

;; Query time: 7 msec
;; SERVER: 127.0.0.53#53(127.0.0.53) (UDP)
;; WHEN: Tue May 26 22:17:56 EDT 2026
;; MSG SIZE  rcvd: 135
```
<br><br>

or with the trace, which shows the whole process of a DNS resolve (from root -> TLD -> Authoritative Name) : 

```
dig +trace google.com

; <<>> DiG 9.20.23-1~deb13u1-Debian <<>> +trace google.com
;; global options: +cmd
.			179179	IN	NS	k.root-servers.net.
.			179179	IN	NS	j.root-servers.net.
.			179179	IN	NS	i.root-servers.net.
.			179179	IN	NS	d.root-servers.net.
.			179179	IN	NS	b.root-servers.net.
.			179179	IN	NS	e.root-servers.net.
.			179179	IN	NS	a.root-servers.net.
.			179179	IN	NS	f.root-servers.net.
.			179179	IN	NS	c.root-servers.net.
.			179179	IN	NS	h.root-servers.net.
.			179179	IN	NS	m.root-servers.net.
.			179179	IN	NS	l.root-servers.net.
.			179179	IN	NS	g.root-servers.net.
.			179179	IN	RRSIG	NS 8 0 518400 20260608200000 20260526190000 54393 . tBvBcoYihTlZpdyG2fU16cZp15Qdgc2XmSmJGNap2Zhu8qm4NjgMEEBH h7ZJkKfjh5DnZEpOrE2MpULPOFISyrR38MnVeHbFh4rd1HQ+9T3po3L4 Nu3Ls3pDDRCZZsLNomNfeLBYZ7ptW4VeCkJpyBQHOIw8dmryx08hECPe IG1pF2KjuTk/JHjAWSACLUDrHcBYMPvWS8ySs5bfFV+ldP/Wbx4bNOuN YJizc+WRWhYZ/uQ+60C6ZdrO2CRNupA1chKfScoCNnKWnNLkOpkLM65g my1Mg+Loug/6twwovV6sji1E0tqd0feeCHNsvMa4aXZxUXVGNOiwiUeg wg2heg==
;; Received 1097 bytes from 127.0.0.53#53(127.0.0.53) in 7 ms

com.			172800	IN	NS	a.gtld-servers.net.
com.			172800	IN	NS	b.gtld-servers.net.
com.			172800	IN	NS	c.gtld-servers.net.
com.			172800	IN	NS	d.gtld-servers.net.
com.			172800	IN	NS	e.gtld-servers.net.
com.			172800	IN	NS	f.gtld-servers.net.
com.			172800	IN	NS	g.gtld-servers.net.
com.			172800	IN	NS	h.gtld-servers.net.
com.			172800	IN	NS	i.gtld-servers.net.
com.			172800	IN	NS	j.gtld-servers.net.
com.			172800	IN	NS	k.gtld-servers.net.
com.			172800	IN	NS	l.gtld-servers.net.
com.			172800	IN	NS	m.gtld-servers.net.
com.			86400	IN	DS	19718 13 2 8ACBB0CD28F41250A80A491389424D341522D946B0DA0C0291F2D3D7 71D7805A
com.			86400	IN	RRSIG	DS 8 1 86400 20260608200000 20260526190000 54393 . eFvQYurDYLr49o/rqjGqFhjbi92D+6Zkwf4SK6q3Lo4ymD8Wy2K67Xoi msWHZ4ejFdqMTUuA+12Ja6k1soANpEiu4CD5x8OP1nHqBmj/OUY5mlNP vSXbvA4txC85aWFKwJe9m9hN9U8PqJDjGO2w3jj1U5Tt+9r2Doqx+lV5 XE5dl14hczc+DBRivurHFWkQocEQUfUzyOlgzqQxhjR8frDtqgbSi71c A1T36bAknqBuFYQu4lDCS+i9iaOkTK+N4F94D2yXTpxUm3DvF1GMaIdC WHjujWpTw55Tk8OPxhvaHzxufVW7aZnhX3K9VDjU4m1OmxD63ZzZOx4t f3rrlw==
;; Received 1170 bytes from 192.203.230.10#53(e.root-servers.net) in 7 ms

;; UDP setup with 2001:503:a83e::2:30#53(2001:503:a83e::2:30) for google.com failed: network unreachable.
;; no servers could be reached
;; UDP setup with 2001:503:a83e::2:30#53(2001:503:a83e::2:30) for google.com failed: network unreachable.
;; no servers could be reached
;; UDP setup with 2001:503:a83e::2:30#53(2001:503:a83e::2:30) for google.com failed: network unreachable.
google.com.		172800	IN	NS	ns2.google.com.
google.com.		172800	IN	NS	ns1.google.com.
google.com.		172800	IN	NS	ns3.google.com.
google.com.		172800	IN	NS	ns4.google.com.
CK0POJMG874LJREF7EFN8430QVIT8BSM.com. 900 IN NSEC3 1 1 0 - CK0Q3UDG8CEKKAE7RUKPGCT1DVSSH8LL NS SOA RRSIG DNSKEY NSEC3PARAM
CK0POJMG874LJREF7EFN8430QVIT8BSM.com. 900 IN RRSIG NSEC3 13 2 900 20260603002629 20260526231629 27677 com. 6+jB85bROu7dTdJgWzCJbp/adTRkbbdbWClI68kqsP6l/KVFa1Ltul80 Q3VIJpb52l4TDceSKOShLb1DjHDHYg==
S84BOR4DK28HNHPLC218O483VOOOD5D8.com. 900 IN NSEC3 1 1 0 - S84BR9CIB2A20L3ETR1M2415ENPP99L8 NS DS RRSIG
S84BOR4DK28HNHPLC218O483VOOOD5D8.com. 900 IN RRSIG NSEC3 13 2 900 20260531012227 20260524001227 27677 com. gFXXm7Dh/KP4FRLkEZbuxPdOQUhQYU6PEJh0cbv5pKlxp7eZGYgVQ8gC iM+Ak1QR3GlzmHOg0wjvejYPKWoNDQ==
;; Received 644 bytes from 192.52.178.30#53(k.gtld-servers.net) in 79 ms

;; UDP setup with 2001:4860:4802:36::a#53(2001:4860:4802:36::a) for google.com failed: network unreachable.
google.com.		300	IN	A	142.250.69.78
;; Received 55 bytes from 216.239.34.10#53(ns2.google.com) in 3 ms

```

<br>
<hr>

#### <a id="host-domain"><span style="color: var(--title)">02.01 HOST DOMAIN</a>

<hr><br>

Requesting the domain will return <span style="color: var(--highlight)">all the IPs associated to the domain</span>. Certain domain have multiple IPs for redundancy, load balancing, or to help prevent DDoS attacks.

```
host hackthissite.org
hackthissite.org has address 137.74.187.104
hackthissite.org has address 137.74.187.101
hackthissite.org has address 137.74.187.100
hackthissite.org has address 137.74.187.102
hackthissite.org has address 137.74.187.103
hackthissite.org mail is handled by 30 aspmx3.googlemail.com.
hackthissite.org mail is handled by 30 aspmx5.googlemail.com.
hackthissite.org mail is handled by 20 alt1.aspmx.l.google.com.
hackthissite.org mail is handled by 30 aspmx2.googlemail.com.
hackthissite.org mail is handled by 10 aspmx.l.google.com.
hackthissite.org mail is handled by 30 aspmx4.googlemail.com.
hackthissite.org mail is handled by 20 alt2.aspmx.l.google.com.
```

From the query, we can learn that hackthissite.org has 5 IP addresses (137.74.187.100 - 104) and 7 mail servers. However, for the mail servers, it is not that hackthissite has 7 dedicated mails servers, but rather lets .google.com mail service handle its mails. The mails servers are ordered in priority (the lower number, the higher the priority), which means that when a use will send an email to hackthissite.org, the mail server will perform a DNS lookup and check to which server to send the email and will then try aspmx.l.google.com since it has the highest priority (10).

<br>

```
host google.com
google.com has address 142.250.69.110
google.com has IPv6 address 2607:f8b0:4020:800::200e
google.com mail is handled by 10 smtp.google.com.
google.com has HTTP service bindings 1 . alpn="h2,h3"
```

From this query, we can see that google.com is hosted at 142.250.69.110 and has 1 mail server. the last line ```google.com has HTTP service bindings 1 . alpn="h2,h3"``` is google describing which HTTP protocols (alp = Application-Layer Protocol Negotiation, and h2 = HTTP/2) and services clients should use when connecting (because it is on newer DNS records). This line shows to client that google accepts HTTP/2 and HTTP/3 protocols, which creates faster interactions when doing HTTP connections since the client already knows the protocol and its version rather than discovering it when receiving the packets.

<br>
<hr>

#### <a id="host-ip"><span style="color: var(--title)">02.02 HOST IP</a>

<hr><br>

Using host with an IP is a reverse DNS lookup, which is therefore done through <span style="color: var(--highlight)">PTR records</span> (Pointer Records).

```
host 8.8.8.8
8.8.8.8.in-addr.arpa domain name pointer dns.google.
```

or

```
host 137.74.187.104
104.187.74.137.in-addr.arpa domain name pointer hackthissite.org.
```

<br>
<hr>

#### <a id="host-other"><span style="color: var(--title)">02.03 HOST OTHER</a>

<hr><br>

While ```host``` is more consice than ```dig```, it also means we need to add arguments to have a more precise answer or check if specific records are there.

Getting mail servers : 

```
host -t MX hackthissite.org
    hackthissite.org mail is handled by 30 aspmx4.googlemail.com.
    hackthissite.org mail is handled by 10 aspmx.l.google.com.
    hackthissite.org mail is handled by 20 alt2.aspmx.l.google.com.
    hackthissite.org mail is handled by 20 alt1.aspmx.l.google.com.
    hackthissite.org mail is handled by 30 aspmx2.googlemail.com.
    hackthissite.org mail is handled by 30 aspmx5.googlemail.com.
    hackthissite.org mail is handled by 30 aspmx3.googlemail.com.
```

Check if IPv6 exists for a domain : 

```
host -t AAAA google.com
    google.com has IPv6 address 2607:f8b0:4023:1807::65
    google.com has IPv6 address 2607:f8b0:4023:1807::8b
    google.com has IPv6 address 2607:f8b0:4023:1807::66
    google.com has IPv6 address 2607:f8b0:4023:1807::64
```

Who is the DNS provider for a domain : 

```
host -t NS hackthissite.org
    hackthissite.org name server h.ns.buddyns.com.
    hackthissite.org name server j.ns.buddyns.com.
    hackthissite.org name server f.ns.buddyns.com.
    hackthissite.org name server c.ns.buddyns.com.
    hackthissite.org name server g.ns.buddyns.com.
```

Check TXT records : 

```
host -t TXT google.com
    google.com descriptive text "apple-domain-verification=30afIBcvSuDV2PLX"
    google.com descriptive text "google-site-verification=wD8N7i1JTNTkezJ49swvWW48f8_9xveREV4oB-0Hf5o"
    google.com descriptive text "globalsign-smime-dv=CDYX+XFHUw2wml6/Gb8+59BsH31KzUr6c1l2BPvqKX8="
    google.com descriptive text "google-site-verification=TV9-DBe4R80X4v0M4U_bd_J9cpOJM0nikft0jAgjmsQ"
    google.com descriptive text "onetrust-domain-verification=6d685f1d41a94696ad7ef771f68993e0"
    google.com descriptive text "docusign=05958488-4752-4ef2-95eb-aa7ba8a3bd0e"
    google.com descriptive text "facebook-domain-verification=22rm551cu4k0ab0bxsw536tlds4h95"
    google.com descriptive text "docusign=1b0a6754-49b1-4db5-8540-d2c12664b289"
    google.com descriptive text "google-site-verification=4ibFUgB-wXLQ_S7vsXVomSTVamuOXBiVAzpR5IZ87D0"
    google.com descriptive text "v=spf1 include:_spf.google.com ~all"
    google.com descriptive text "MS=E4A68B9AB2BB9670BCE15412F62916164C0B20BB"
    google.com descriptive text "onetrust-domain-verification=0d477fe608074e6f9c12bca7826035cc"
    google.com descriptive text "cisco-ci-domain-verification=47c38bc8c4b74b7233e9053220c1bbe76bcc1cd33c7acf7acd36cd6a5332004b"
```

TXT records store the metadata for a lot of policies. They are often used to prove ownership, secure email, integrating services, publishing policies of the company, etc.


<br> <br>

<hr>

### <a id="nslookup"><span style="color: var(--title)">03.00 NSLOOKUP</a>

<hr><br>

```nslookup``` does a DNS query and checks which server handled the request as well as what the response is.

Important DNS servers which can ber queried : 

| SERVER     | ADDRESS        |
|------------|----------------|
| Google     | 8.8.8.8        |
| Cloudflare | 1.1.1.1        |
| Quad9      | 9.9.9.9        |
| OpenDNS    | 208.67.222.222 |


For the following query, I can see that I already queried google.com since it is my local DNS server that handled the DNS resolve, therefore, the record was already in my cache.

```127.0.0.X``` is also known as the <span style="color: var(--highlight)">loopback address</span> or <span style="color: var(--highlight)">localhost</span>. It is basically the address the computer uses to refer to itself. It is also ```.53``` because it used port 53, used for DNS.

```
nslookup google.com
    Server:		127.0.0.53
    Address:	127.0.0.53#53
    
    Non-authoritative answer:
    Name:	google.com
    Address: 142.250.69.110
    Name:	google.com
    Address: 2607:f8b0:4023:1807::66
    Name:	google.com
    Address: 2607:f8b0:4023:1807::64
    Name:	google.com
    Address: 2607:f8b0:4023:1807::8a
    Name:	google.com
    Address: 2607:f8b0:4023:1807::71
```

<br>

There I queried google's server directly, thus not resolving the query with my own DNS resolver but rather google's.

```
nslookup google.com 8.8.8.8
    `Server:		8.8.8.8
    Address:	8.8.8.8#53
    
    Non-authoritative answer:
    Name:	google.com
    Address: 142.250.69.78
    Name:	google.com
    Address: 2607:f8b0:4020:801::200e
```

Using cloudfare's DNS server : 

```
nslookup google.com 1.1.1.1
    Server:		1.1.1.1
    Address:	1.1.1.1#53
    
    Non-authoritative answer:
    Name:	google.com
    Address: 64.233.178.102
    Name:	google.com
    Address: 64.233.178.138
    Name:	google.com
    Address: 64.233.178.139
    Name:	google.com
    Address: 64.233.178.113
    Name:	google.com
    Address: 64.233.178.100
    Name:	google.com
    Address: 64.233.178.101
    Name:	google.com
    Address: 2607:f8b0:4020:c07::8b
    Name:	google.com
    Address: 2607:f8b0:4020:c07::71
    Name:	google.com
    Address: 2607:f8b0:4020:c07::66
    Name:	google.com
    Address: 2607:f8b0:4020:c07::65
```


<br> <br>

<hr>

### <a id="dnsrecon"><span style="color: var(--title)">04.00 DNSRECON</a>

<hr><br>

```dnsrecon``` is used to get as much DNS information as possible about a domain. It helpful since it automates all the dig or host queries such as A/AAAA records, MX, SOA, TXT, ect. and dumps them in one place and one query. It also bruteforces subdomains which can show forgotten loose ends from companies.

```
dnsrecon -d google.com

[*] std: Performing General Enumeration against: google.com...
[-] All nameservers failed to answer the DNSSEC query for google.com
[*] 	 SOA ns1.google.com 216.239.32.10
[*] 	 SOA ns1.google.com 2001:4860:4802:32::a
[*] 	 NS ns2.google.com 216.239.34.10
[*] 	 NS ns2.google.com 2001:4860:4802:34::a
[*] 	 NS ns1.google.com 216.239.32.10
[*] 	 NS ns1.google.com 2001:4860:4802:32::a
[*] 	 NS ns4.google.com 216.239.38.10
[*] 	 NS ns4.google.com 2001:4860:4802:38::a
[*] 	 NS ns3.google.com 216.239.36.10
[*] 	 NS ns3.google.com 2001:4860:4802:36::a
[*] 	 MX smtp.google.com 64.233.178.27
[*] 	 MX smtp.google.com 2607:f8b0:4020:c07::1a
[*] 	 A google.com 142.250.69.110
[*] 	 AAAA google.com 2607:f8b0:4020:803::200e
[*] 	 TXT google.com globalsign-smime-dv=CDYX+XFHUw2wml6/Gb8+59BsH31KzUr6c1l2BPvqKX8=
[*] 	 TXT google.com docusign=1b0a6754-49b1-4db5-8540-d2c12664b289
[*] 	 TXT google.com docusign=05958488-4752-4ef2-95eb-aa7ba8a3bd0e
[*] 	 TXT google.com google-site-verification=4ibFUgB-wXLQ_S7vsXVomSTVamuOXBiVAzpR5IZ87D0
[*] 	 TXT google.com apple-domain-verification=30afIBcvSuDV2PLX
[*] 	 TXT google.com google-site-verification=wD8N7i1JTNTkezJ49swvWW48f8_9xveREV4oB-0Hf5o
[*] 	 TXT google.com v=spf1 include:_spf.google.com ~all
[*] 	 TXT google.com onetrust-domain-verification=0d477fe608074e6f9c12bca7826035cc
[*] 	 TXT google.com google-site-verification=TV9-DBe4R80X4v0M4U_bd_J9cpOJM0nikft0jAgjmsQ
[*] 	 TXT google.com facebook-domain-verification=22rm551cu4k0ab0bxsw536tlds4h95
[*] 	 TXT google.com MS=E4A68B9AB2BB9670BCE15412F62916164C0B20BB
[*] 	 TXT google.com onetrust-domain-verification=6d685f1d41a94696ad7ef771f68993e0
[*] 	 TXT google.com cisco-ci-domain-verification=47c38bc8c4b74b7233e9053220c1bbe76bcc1cd33c7acf7acd36cd6a5332004b
[*] 	 TXT _dmarc.google.com v=DMARC1; p=reject; rua=mailto:mailauth-reports@google.com
[*] Enumerating SRV Records
[+] 	 SRV _ldap._tcp.google.com ldap.google.com 216.239.32.58 389
[+] 	 SRV _ldap._tcp.google.com ldap.google.com 2001:4860:4802:32::3a 389
[+] 	 SRV _caldav._tcp.google.com calendar.google.com 142.250.69.110 80
[+] 	 SRV _caldav._tcp.google.com calendar.google.com 2607:f8b0:4020:802::200e 80
[+] 	 SRV _caldavs._tcp.google.com calendar.google.com 142.250.69.110 443
[+] 	 SRV _caldavs._tcp.google.com calendar.google.com 2607:f8b0:4020:802::200e 443
[+] 	 SRV _carddavs._tcp.google.com google.com 142.250.69.110 443
[+] 	 SRV _carddavs._tcp.google.com google.com 2607:f8b0:4020:803::200e 443
[+] 8 Records Found`
```


| LINE                                                                                      | INFORMATION                                                                     | 
|-------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| [-] All nameservers failed to answer the DNSSEC query for google.com                      | No DNSSEC certificate could be found                                            |
| [*] 	 SOA ns1.google.com 216.239.32.10<br>[*] 	 SOA ns1.google.com 2001:4860:4802:32::a   | SOA = Start of Authority, therefore, it is the primary authoritative DNS server |
| [*] 	 NS ns2.google.com 216.239.34.10<br>[*] 	 NS ns2.google.com 2001:4860:4802:34::a     | Name Servers (DNS) for google.com                                               |
| [*] 	 MX smtp.google.com 64.233.178.27<br>[*] 	 MX smtp.google.com 2607:f8b0:4020:c07::1a | Mail servers for google.com and their IPs (IPv4 and IPv6)                       |
| [*] 	 A google.com 142.250.69.110<br>[*] 	 AAAA google.com 2607:f8b0:4020:803::200e       | The IP address of google.com                                                    |


All TXT records show the infrastructure's metadata of google.com and all the 'rules' google has for if you want to interact with it with certain protocls.

All SRV (Service Records) show the services, infrastructures, protocols, and the internal architecture used by google with the IP and the port associated.

<br>
<hr>

#### <a id="dnsrecon-zone-transfer"><span style="color: var(--title)">04.01 AUTHORITATIVE ZONE TRANSFER</a>

<hr><br>

DNS servers sometime proceed with a data synchronisation between themselves to ensure their data is complete and correct and this synchronisation is called <span style="color: var(--highlight)">Authoritative Zone Transfer</span> or <span style="color: var(--highlight)">AXFR</span>. 

Basically, when the synchronisation happens, the primary DNS server copies its entire database to each of its secondary DNS server. But, if it is misconfigured, anyone could request the entire primary DNS server's database which can expose many things such as : 
- all subdomains ;
- internal servers ;
- staging environments ;
- VPN portals ;
- mail infrastructures ;
- etc.


> zonetransfer.me is left misconfigured on purpose for learning

```
dnsrecon -d zonetransfer.me -t axfr
[*] Checking for Zone Transfer for zonetransfer.me name servers
[*] Resolving SOA Record
[+] 	 SOA nsztm1.digi.ninja 81.4.108.41
[*] Resolving NS Records
[*] NS Servers found:
[+] 	 NS nsztm1.digi.ninja 81.4.108.41
[+] 	 NS nsztm2.digi.ninja 5.196.105.10
[*] Removing any duplicate NS server IP Addresses...
[*]  
[*] Trying NS server 81.4.108.41
[+] 81.4.108.41 Has port 53 TCP Open
[+] Zone Transfer was successful!!
[*] 	 SOA nsztm1.digi.ninja 81.4.108.41
[*] 	 NS nsztm1.digi.ninja 81.4.108.41
[*] 	 NS nsztm2.digi.ninja 5.196.105.10
[*] 	 NS intns1.zonetransfer.me 81.4.108.41
[*] 	 NS intns2.zonetransfer.me 5.196.105.10
[*] 	 TXT google-site-verification=tyP28J7JAUHA9fw2sHXMgcCC0I6XBmmoVi04VlMewxA
[*] 	 TXT 6Oa05hbUJ9xSsvYy7pApQvwCUSSGgxvrbdizjePEsZI
[*] 	 TXT ; ls
[*] 	 TXT Remember to call or email Pippa on +44 123 4567890 or pippa@zonetransfer.me when making DNS changes
[*] 	 TXT AbCdEfG
[*] 	 TXT Hi to Josh and all his class
[*] 	 TXT ZoneTransfer.me service provided by Robin Wood - robin@digi.ninja. See http://digi.ninja/projects/zonetransferme.php for more information.
[*] 	 TXT Robin Wood
[*] 	 TXT ' or 1=1 --
[*] 	 TXT () { :]}; echo ShellShocked
[*] 	 TXT '><script>alert('Boo')</script>
[*] 	 PTR www.zonetransfer.me 5.196.105.14
[*] 	 MX @.zonetransfer.me ASPMX.L.GOOGLE.COM 64.233.178.26
[*] 	 MX @.zonetransfer.me ASPMX.L.GOOGLE.COM 2607:f8b0:4020:c07::1a
[*] 	 MX @.zonetransfer.me ALT1.ASPMX.L.GOOGLE.COM 172.253.116.26
[*] 	 MX @.zonetransfer.me ALT1.ASPMX.L.GOOGLE.COM 2a00:1450:400b:c02::1a
[*] 	 MX @.zonetransfer.me ALT2.ASPMX.L.GOOGLE.COM 192.178.223.27
[*] 	 MX @.zonetransfer.me ALT2.ASPMX.L.GOOGLE.COM 2a00:1450:4009:c0f::1a
[*] 	 MX @.zonetransfer.me ASPMX2.GOOGLEMAIL.COM 172.253.116.27
[*] 	 MX @.zonetransfer.me ASPMX2.GOOGLEMAIL.COM 2a00:1450:400b:c02::1a
[*] 	 MX @.zonetransfer.me ASPMX3.GOOGLEMAIL.COM 192.178.223.27
[*] 	 MX @.zonetransfer.me ASPMX3.GOOGLEMAIL.COM 2a00:1450:4009:c0f::1a
[*] 	 MX @.zonetransfer.me ASPMX4.GOOGLEMAIL.COM 173.194.76.26
[*] 	 MX @.zonetransfer.me ASPMX4.GOOGLEMAIL.COM 2a00:1450:400c:c00::1b
[*] 	 MX @.zonetransfer.me ASPMX5.GOOGLEMAIL.COM 142.250.102.26
[*] 	 MX @.zonetransfer.me ASPMX5.GOOGLEMAIL.COM 2a00:1450:4025:402::1a
[*] 	 AAAA deadbeef.zonetransfer.me dead:beaf::
[*] 	 AAAA ipv6actnow.org.zonetransfer.me 2001:67c:2e8:11::c100:1332
[*] 	 A @.zonetransfer.me 5.196.105.14
[*] 	 A asfdbbox.zonetransfer.me 127.0.0.1
[*] 	 A canberra-office.zonetransfer.me 202.14.81.230
[*] 	 A dc-office.zonetransfer.me 143.228.181.132
[*] 	 A email.zonetransfer.me 74.125.206.26
[*] 	 A home.zonetransfer.me 127.0.0.1
[*] 	 A intns1.zonetransfer.me 81.4.108.41
[*] 	 A intns2.zonetransfer.me 5.196.105.10
[*] 	 A office.zonetransfer.me 4.23.39.254
[*] 	 A owa.zonetransfer.me 207.46.197.32
[*] 	 A alltcpportsopen.firewall.test.zonetransfer.me 127.0.0.1
[*] 	 A vpn.zonetransfer.me 174.36.59.154
[*] 	 A www.zonetransfer.me 5.196.105.14
[*] 	 CNAME staging.zonetransfer.me www.sydneyoperahouse.com 13.227.246.59
[*] 	 CNAME staging.zonetransfer.me www.sydneyoperahouse.com 13.227.246.54
[*] 	 CNAME staging.zonetransfer.me www.sydneyoperahouse.com 13.227.246.108
[*] 	 CNAME staging.zonetransfer.me www.sydneyoperahouse.com 13.227.246.55
[*] 	 CNAME staging.zonetransfer.me www.sydneyoperahouse.com 2600:9000:2044:ae00:7:60:4d00:93a1
[*] 	 CNAME staging.zonetransfer.me www.sydneyoperahouse.com 2600:9000:2044:a00:7:60:4d00:93a1
[*] 	 CNAME staging.zonetransfer.me www.sydneyoperahouse.com 2600:9000:2044:9000:7:60:4d00:93a1
[*] 	 CNAME staging.zonetransfer.me www.sydneyoperahouse.com 2600:9000:2044:b400:7:60:4d00:93a1
[*] 	 CNAME staging.zonetransfer.me www.sydneyoperahouse.com 2600:9000:2044:6e00:7:60:4d00:93a1
[*] 	 CNAME staging.zonetransfer.me www.sydneyoperahouse.com 2600:9000:2044:4800:7:60:4d00:93a1
[*] 	 CNAME staging.zonetransfer.me www.sydneyoperahouse.com 2600:9000:2044:5c00:7:60:4d00:93a1
[*] 	 CNAME staging.zonetransfer.me www.sydneyoperahouse.com 2600:9000:2044:a400:7:60:4d00:93a1
[*] 	 SRV _sip._tcp.zonetransfer.me www 5060 0 no_ip
[*] 	 HINFO Casio fx-700G Windows XP
[*] 	 RP robin robinwood
[*] 	 AFSDB 1 asfdbbox
[*] 	 AFSDB 1 asfdbbox
[*] 	 LOC 53 20 56.558 N 1 38 33.526 W 0.00m
[*] 	 NAPTR P 1 1  email.zonetransfer.me E2U+email
[*] 	 NAPTR P 2 3 !^.*$!sip:customer-service@zonetransfer.me! . E2U+sip
[*] 	 CERT PKIX 0 0 MIIDvTCCAqUCFHh5BGzOrlYrXo5h90ip m0aDUEz9MA0GCSqGSIb3DQEBCwUAMIGa MQswCQYDVQQGEwJHQjEYMBYGA1UECAwP U291dGggWW9ya3NoaXJlMRIwEAYDVQQH DAlTaGVmZmllbGQxEjAQBgNVBAoMCURp Z2luaW5qYTEQMA4GA1UECwwHSGFja2lu ZzEYMBYGA1UEAwwPem9uZXRyYW5zZmVy Lm1lMR0wGwYJKoZIhvcNAQkBFg56dG1A ZGlnaS5uaW5qYTAeFw0yNTA3MDIxMzU1 MTNaFw0yNjA3MDIxMzU1MTNaMIGaMQsw CQYDVQQGEwJHQjEYMBYGA1UECAwPU291 dGggWW9ya3NoaXJlMRIwEAYDVQQHDAlT aGVmZmllbGQxEjAQBgNVBAoMCURpZ2lu aW5qYTEQMA4GA1UECwwHSGFja2luZzEY MBYGA1UEAwwPem9uZXRyYW5zZmVyLm1l MR0wGwYJKoZIhvcNAQkBFg56dG1AZGln aS5uaW5qYTCCASIwDQYJKoZIhvcNAQEB BQADggEPADCCAQoCggEBALzYVM9WlBqO KU1lmnKJkKdIEZOhkscHQktEJORXCism SWV3FfbsLw7D3sfCc0h9ecZglsYvFUmE M0I0noYtuHPAlF2+FotVuoFrYuMYrEQo Zs4kuORIEx8pwHMZQUSM6KwVVLIB/FE9 56GfovgxGxWs33QaTKATAVChD9KTLf6w Vh/eC+0GI6mbvGvjqZFmmV/SYmmkdqEB WB7q3+SByfVrUohCA2GO30dwk6vUBtIj +J+i4SzKzLXIvFEfbCirMPQvdflgwPbj wp+cWG7oUBvfQZfZbaTp+9+V8FoBl0f8 fGj/Mae1n0rSV5hnuXot8d3PAoAWQtW3 HJUv1nEboAMCAwEAATANBgkqhkiG9w0B AQsFAAOCAQEAXop6ftpV2/r7tkXqFCsM wub7ZBd12U14nsBon+X7K5Nr6obrVAtn WO+XwD8x2UgvYIQBuRLK9LOX6VYoiWMV rItIN8KRSsin5eJe4tzewsNGrVtkVbbK ULViCeBtDgmImk8rkZeWU1uNOsq0t/wd 3GUZe2CM9DpKVhPFhc9Uq3pYbAsidYlp SApuuj8ka3L+VruzJVwveyKTUkWAsN1i Sv7BGgEF0039WW3IEv1ZP81cAdWFy1fx +tuteM6Iz5xkx1tp0/eLtb39cnKFQnrs 8itDG2j3yBc3CClYmw4NNU2nODN4COt7 uzXBez6iIFSNqQjVyFyomtPn4ae0cYRH Ew==
[*] 	 DNSKEY RSASHA1NSEC3SHA1 256 03010001aa682fe227401631da88b774 8e36e3e7440c5827556d1c38835f098d e32eb9e067106c12eafc07284c390b31 a08398bbfedbddcf2d8f6070950aaeab 165eefd407496efc7caec3f9b3fff00a 2c9c11b92b98095c4be9844b49f71f3f 5f463def55da5bd94b9ad27a878017b2 b6e04ebda0f4a3a23d221bbc33cc5bca 8b31a4244e6f3d86f3dcf8dbb3ace10e 3cd8685ecfe23ccf92433aef4c17ebeb 2d30db23ed61875623590defa3900718 74c6321d9724f440a8a884aab66f26cf 44e24eecadcd94373ca1a399a171f9fd 17cb0d0cf1cd7c98495f1f2f08a651eb a5a3b600d7523f00743b0ec52c16ed92 dafc460b247c9e6c7d8186cde2c06f4c 5da2e5e3 3
[*]  
[*] Trying NS server 5.196.105.10
[+] 5.196.105.10 Has port 53 TCP Open
[+] Zone Transfer was successful!!
[*] 	 SOA nsztm1.digi.ninja 81.4.108.41
[*] 	 NS nsztm1.digi.ninja 81.4.108.41
[*] 	 NS nsztm2.digi.ninja 5.196.105.10
[*] 	 NS intns1.zonetransfer.me 81.4.108.41
[*] 	 NS intns2.zonetransfer.me 5.196.105.10
[*] 	 TXT google-site-verification=tyP28J7JAUHA9fw2sHXMgcCC0I6XBmmoVi04VlMewxA
[*] 	 TXT 6Oa05hbUJ9xSsvYy7pApQvwCUSSGgxvrbdizjePEsZI
[*] 	 TXT ; ls
[*] 	 TXT Remember to call or email Pippa on +44 123 4567890 or pippa@zonetransfer.me when making DNS changes
[*] 	 TXT AbCdEfG
[*] 	 TXT Hi to Josh and all his class
[*] 	 TXT ZoneTransfer.me service provided by Robin Wood - robin@digi.ninja. See http://digi.ninja/projects/zonetransferme.php for more information.
[*] 	 TXT Robin Wood
[*] 	 TXT ' or 1=1 --
[*] 	 TXT () { :]}; echo ShellShocked
[*] 	 TXT '><script>alert('Boo')</script>
[*] 	 PTR www.zonetransfer.me 5.196.105.14
[*] 	 MX @.zonetransfer.me ASPMX.L.GOOGLE.COM 64.233.178.26
[*] 	 MX @.zonetransfer.me ASPMX.L.GOOGLE.COM 2607:f8b0:4020:c07::1a
[*] 	 MX @.zonetransfer.me ALT1.ASPMX.L.GOOGLE.COM 172.253.116.26
[*] 	 MX @.zonetransfer.me ALT1.ASPMX.L.GOOGLE.COM 2a00:1450:400b:c02::1a
[*] 	 MX @.zonetransfer.me ALT2.ASPMX.L.GOOGLE.COM 192.178.223.27
[*] 	 MX @.zonetransfer.me ALT2.ASPMX.L.GOOGLE.COM 2a00:1450:4009:c0f::1a
[*] 	 MX @.zonetransfer.me ASPMX2.GOOGLEMAIL.COM 172.253.116.27
[*] 	 MX @.zonetransfer.me ASPMX2.GOOGLEMAIL.COM 2a00:1450:400b:c02::1a
[*] 	 MX @.zonetransfer.me ASPMX3.GOOGLEMAIL.COM 192.178.223.27
[*] 	 MX @.zonetransfer.me ASPMX3.GOOGLEMAIL.COM 2a00:1450:4009:c0f::1a
[*] 	 MX @.zonetransfer.me ASPMX4.GOOGLEMAIL.COM 173.194.76.26
[*] 	 MX @.zonetransfer.me ASPMX4.GOOGLEMAIL.COM 2a00:1450:400c:c00::1b
[*] 	 MX @.zonetransfer.me ASPMX5.GOOGLEMAIL.COM 142.250.102.26
[*] 	 MX @.zonetransfer.me ASPMX5.GOOGLEMAIL.COM 2a00:1450:4025:402::1a
[*] 	 AAAA deadbeef.zonetransfer.me dead:beaf::
[*] 	 AAAA ipv6actnow.org.zonetransfer.me 2001:67c:2e8:11::c100:1332
[*] 	 A @.zonetransfer.me 5.196.105.14
[*] 	 A asfdbbox.zonetransfer.me 127.0.0.1
[*] 	 A canberra-office.zonetransfer.me 202.14.81.230
[*] 	 A dc-office.zonetransfer.me 143.228.181.132
[*] 	 A email.zonetransfer.me 74.125.206.26
[*] 	 A home.zonetransfer.me 127.0.0.1
[*] 	 A intns1.zonetransfer.me 81.4.108.41
[*] 	 A intns2.zonetransfer.me 5.196.105.10
[*] 	 A office.zonetransfer.me 4.23.39.254
[*] 	 A owa.zonetransfer.me 207.46.197.32
[*] 	 A alltcpportsopen.firewall.test.zonetransfer.me 127.0.0.1
[*] 	 A vpn.zonetransfer.me 174.36.59.154
[*] 	 A www.zonetransfer.me 5.196.105.14
[*] 	 CNAME staging.zonetransfer.me www.sydneyoperahouse.com 13.227.246.54
[*] 	 CNAME staging.zonetransfer.me www.sydneyoperahouse.com 13.227.246.55
[*] 	 CNAME staging.zonetransfer.me www.sydneyoperahouse.com 13.227.246.59
[*] 	 CNAME staging.zonetransfer.me www.sydneyoperahouse.com 13.227.246.108
[*] 	 CNAME staging.zonetransfer.me www.sydneyoperahouse.com 2600:9000:2044:9000:7:60:4d00:93a1
[*] 	 CNAME staging.zonetransfer.me www.sydneyoperahouse.com 2600:9000:2044:a400:7:60:4d00:93a1
[*] 	 CNAME staging.zonetransfer.me www.sydneyoperahouse.com 2600:9000:2044:a00:7:60:4d00:93a1
[*] 	 CNAME staging.zonetransfer.me www.sydneyoperahouse.com 2600:9000:2044:5c00:7:60:4d00:93a1
[*] 	 CNAME staging.zonetransfer.me www.sydneyoperahouse.com 2600:9000:2044:4800:7:60:4d00:93a1
[*] 	 CNAME staging.zonetransfer.me www.sydneyoperahouse.com 2600:9000:2044:ae00:7:60:4d00:93a1
[*] 	 CNAME staging.zonetransfer.me www.sydneyoperahouse.com 2600:9000:2044:b400:7:60:4d00:93a1
[*] 	 CNAME staging.zonetransfer.me www.sydneyoperahouse.com 2600:9000:2044:6e00:7:60:4d00:93a1
[*] 	 SRV _sip._tcp.zonetransfer.me www 5060 0 no_ip
[*] 	 HINFO Casio fx-700G Windows XP
[*] 	 RP robin robinwood
[*] 	 AFSDB 1 asfdbbox
[*] 	 AFSDB 1 asfdbbox
[*] 	 LOC 53 20 56.558 N 1 38 33.526 W 0.00m
[*] 	 NAPTR P 1 1  email.zonetransfer.me E2U+email
[*] 	 NAPTR P 2 3 !^.*$!sip:customer-service@zonetransfer.me! . E2U+sip
[*] 	 CERT PKIX 0 0 MIIDvTCCAqUCFHh5BGzOrlYrXo5h90ip m0aDUEz9MA0GCSqGSIb3DQEBCwUAMIGa MQswCQYDVQQGEwJHQjEYMBYGA1UECAwP U291dGggWW9ya3NoaXJlMRIwEAYDVQQH DAlTaGVmZmllbGQxEjAQBgNVBAoMCURp Z2luaW5qYTEQMA4GA1UECwwHSGFja2lu ZzEYMBYGA1UEAwwPem9uZXRyYW5zZmVy Lm1lMR0wGwYJKoZIhvcNAQkBFg56dG1A ZGlnaS5uaW5qYTAeFw0yNTA3MDIxMzU1 MTNaFw0yNjA3MDIxMzU1MTNaMIGaMQsw CQYDVQQGEwJHQjEYMBYGA1UECAwPU291 dGggWW9ya3NoaXJlMRIwEAYDVQQHDAlT aGVmZmllbGQxEjAQBgNVBAoMCURpZ2lu aW5qYTEQMA4GA1UECwwHSGFja2luZzEY MBYGA1UEAwwPem9uZXRyYW5zZmVyLm1l MR0wGwYJKoZIhvcNAQkBFg56dG1AZGln aS5uaW5qYTCCASIwDQYJKoZIhvcNAQEB BQADggEPADCCAQoCggEBALzYVM9WlBqO KU1lmnKJkKdIEZOhkscHQktEJORXCism SWV3FfbsLw7D3sfCc0h9ecZglsYvFUmE M0I0noYtuHPAlF2+FotVuoFrYuMYrEQo Zs4kuORIEx8pwHMZQUSM6KwVVLIB/FE9 56GfovgxGxWs33QaTKATAVChD9KTLf6w Vh/eC+0GI6mbvGvjqZFmmV/SYmmkdqEB WB7q3+SByfVrUohCA2GO30dwk6vUBtIj +J+i4SzKzLXIvFEfbCirMPQvdflgwPbj wp+cWG7oUBvfQZfZbaTp+9+V8FoBl0f8 fGj/Mae1n0rSV5hnuXot8d3PAoAWQtW3 HJUv1nEboAMCAwEAATANBgkqhkiG9w0B AQsFAAOCAQEAXop6ftpV2/r7tkXqFCsM wub7ZBd12U14nsBon+X7K5Nr6obrVAtn WO+XwD8x2UgvYIQBuRLK9LOX6VYoiWMV rItIN8KRSsin5eJe4tzewsNGrVtkVbbK ULViCeBtDgmImk8rkZeWU1uNOsq0t/wd 3GUZe2CM9DpKVhPFhc9Uq3pYbAsidYlp SApuuj8ka3L+VruzJVwveyKTUkWAsN1i Sv7BGgEF0039WW3IEv1ZP81cAdWFy1fx +tuteM6Iz5xkx1tp0/eLtb39cnKFQnrs 8itDG2j3yBc3CClYmw4NNU2nODN4COt7 uzXBez6iIFSNqQjVyFyomtPn4ae0cYRH Ew==
[*] 	 DNSKEY RSASHA1NSEC3SHA1 256 03010001aa682fe227401631da88b774 8e36e3e7440c5827556d1c38835f098d e32eb9e067106c12eafc07284c390b31 a08398bbfedbddcf2d8f6070950aaeab 165eefd407496efc7caec3f9b3fff00a 2c9c11b92b98095c4be9844b49f71f3f 5f463def55da5bd94b9ad27a878017b2 b6e04ebda0f4a3a23d221bbc33cc5bca 8b31a4244e6f3d86f3dcf8dbb3ace10e 3cd8685ecfe23ccf92433aef4c17ebeb 2d30db23ed61875623590defa3900718 74c6321d9724f440a8a884aab66f26cf 44e24eecadcd94373ca1a399a171f9fd 17cb0d0cf1cd7c98495f1f2f08a651eb a5a3b600d7523f00743b0ec52c16ed92 dafc460b247c9e6c7d8186cde2c06f4c 5da2e5e3 3
```


| LINE | INFORMATION                                                                                                  | 
|------|--------------------------------------------------------------------------------------------------------------|
| [+] Zone Transfer was successful!! | DNS server was misconfigured : we now have the entire DNS database for this record !                         |
| [*] 	 A asfdbbox.zonetransfer.me 127.0.0.1 | There is a localhost IP available ; maybe a staging or testing environment, or some data                     |
| [*] 	 A vpn.zonetransfer.me 174.36.59.154 | The address of the VPN portal it uses                                                                        |
| [*] 	 SRV _sip._tcp.zonetransfer.me www 5060 0 no_ip | There is a VoIP / call service on port 5060                                                                  | 
| [*] 	 CNAME staging.zonetransfer.me www.sydneyoperahouse.com 13.227.246.54 | CNAME means 'alias', therefore, staging.zonetransfer.me is just a pointer to domain www.sydneyoperahouse.com |
| [*] 	 TXT ' or 1=1 --<br>[*] 	 TXT () { :]}; echo ShellShocked<br>[*] 	 TXT '><script>alert('Boo')</script> | Attack payload examples                                                                                      |
| [*] 	 HINFO Casio fx-700G Windows XP | Host information : the host system type and OS                                                               |
| [*] 	 LOC 53 20 56.558 N 1 38 33.526 W 0.00m | The localisation of the server (the exact coordinates !)                                                     |
| [*] 	 CERT PKIX 0 0 MIIDvTCCAqUCFHh5BGzOrlYrXo5h90ip [...] | The full cryptographic certificate, which is basically the public SSL certificate embedded in the DNS        |\
| [*] 	 DNSKEY RSASHA1NSEC3SHA1 256 03010001aa682fe227401631da88b774 [...] | The DNSSEC cryptographic key used to sign the DNS records and prevent DNS spoofing                           |


<br>
<hr>

#### <a id="dnsrecon-subdomains"><span style="color: var(--title)">04.02 BRUTE FORCE SUBDOMAINS</a>

<hr><br>

using ```dnsrecon -d example.com -D wordlist.txt -t brt```, note that wordlist.txt matters since it searches for common workds such as admin, mail, vpn, api, dev, etc.

```
dnsrecon -d zonetransfer.me -D wordlist.txt -t brt
[*] Using the dictionary file: /usr/share/dnsrecon/dnsrecon/data/namelist.txt (provided by tool)
[*] brt: Performing host and subdomain brute force against zonetransfer.me...
[+] 	 A email.zonetransfer.me 74.125.206.26
[+] 	 A home.zonetransfer.me 127.0.0.1
[+] 	 A office.zonetransfer.me 4.23.39.254
[+] 	 A owa.zonetransfer.me 207.46.197.32
[+] 	 CNAME staging.zonetransfer.me www.sydneyoperahouse.com
[+] 	 CNAME www.sydneyoperahouse.com dc8smvz8l4jlg.cloudfront.net
[+] 	 A dc8smvz8l4jlg.cloudfront.net 13.227.246.108
[+] 	 A dc8smvz8l4jlg.cloudfront.net 13.227.246.59
[+] 	 A dc8smvz8l4jlg.cloudfront.net 13.227.246.54
[+] 	 A dc8smvz8l4jlg.cloudfront.net 13.227.246.55
[+] 	 CNAME staging.zonetransfer.me www.sydneyoperahouse.com
[+] 	 CNAME www.sydneyoperahouse.com dc8smvz8l4jlg.cloudfront.net
[+] 	 AAAA dc8smvz8l4jlg.cloudfront.net 2600:9000:2044:d400:7:60:4d00:93a1
[+] 	 AAAA dc8smvz8l4jlg.cloudfront.net 2600:9000:2044:b000:7:60:4d00:93a1
[+] 	 AAAA dc8smvz8l4jlg.cloudfront.net 2600:9000:2044:c600:7:60:4d00:93a1
[+] 	 AAAA dc8smvz8l4jlg.cloudfront.net 2600:9000:2044:4400:7:60:4d00:93a1
[+] 	 AAAA dc8smvz8l4jlg.cloudfront.net 2600:9000:2044:4800:7:60:4d00:93a1
[+] 	 AAAA dc8smvz8l4jlg.cloudfront.net 2600:9000:2044:ee00:7:60:4d00:93a1
[+] 	 AAAA dc8smvz8l4jlg.cloudfront.net 2600:9000:2044:bc00:7:60:4d00:93a1
[+] 	 AAAA dc8smvz8l4jlg.cloudfront.net 2600:9000:2044:400:7:60:4d00:93a1
[+] 	 CNAME testing.zonetransfer.me www.zonetransfer.me
[+] 	 A www.zonetransfer.me 5.196.105.14
[+] 	 A vpn.zonetransfer.me 174.36.59.154
[+] 	 A www.zonetransfer.me 5.196.105.14
[+] 24 Records Found
```


<br> <br>

<hr>

### <a id="theharvester"><span style="color: var(--title)">05.00 THEHARVESTER</a>

<hr><br>

```theHarvester``` is a command which runs a <span style="color: var(--highlight)">large OSINT recon run</span> or the specified ```-d``` domain. In essence, theHarverster will try search engines, leak databases, DNS sources, securit APIs, code repos, and thread intel platforms (though most of them require an API key to work).

+ a databases can be queried directly through through ```theHarvester -d [domain] -b [database]``` (for i.e. : crt.sh with `crtsh` or DNSDumpster with `dnsdumpster`)

```
theHarvester -d zonetransfer.me -b all
Read proxies.yaml from /etc/theHarvester/proxies.yaml
*******************************************************************
*  _   _                                            _             *
* | |_| |__   ___    /\  /\__ _ _ ____   _____  ___| |_ ___ _ __  *
* | __|  _ \ / _ \  / /_/ / _` | '__\ \ / / _ \/ __| __/ _ \ '__| *
* | |_| | | |  __/ / __  / (_| | |   \ V /  __/\__ \ ||  __/ |    *
*  \__|_| |_|\___| \/ /_/ \__,_|_|    \_/ \___||___/\__\___|_|    *
*                                                                 *
* theHarvester 4.10.1                                             *
* Coded by Christian Martorella                                   *
* Edge-Security Research                                          *
* cmartorella@edge-security.com                                   *
*                                                                 *
*******************************************************************

[*] Target: zonetransfer.me 
######### I REMOVED 'MISSING API KEY' ERRORS #################################################
        Searching results.
[*] Searching Certspotter. 
[*] Searching Chaos. 
[*] Searching Baidu. 
[*] Searching Duckduckgo. 
Fofa API error: [-700] 账号无效
Failed to parse Fofa response: 
[!] Missing API key for Fofa API (Invalid credentials). 
[*] Searching Fofa. 
[*] Searching Gitlab. 
[*] Searching Commoncrawl. 
2026-05-27 19:34:25,783 - theHarvester.discovery.hudsonrocksearch - INFO - Starting Hudson Rock processing for: zonetransfer.me
2026-05-27 19:34:25,783 - theHarvester.discovery.hudsonrocksearch - INFO - Starting Hudson Rock search for: zonetransfer.me
[*] Searching Hackertarget. 
Read api-keys.yaml from /etc/theHarvester/api-keys.yaml
[*] Searching Leakix. 
[*] Searching Leaklookup. 
2026-05-27 19:34:32,113 - theHarvester.discovery.hudsonrocksearch - INFO - Domain statistics: 0 total compromised, 0 employees, 0 users
2026-05-27 19:34:33,115 - theHarvester.discovery.hudsonrocksearch - INFO - Hudson Rock search completed. Found 0 hosts, 0 IPs, 0 emails
2026-05-27 19:34:33,116 - theHarvester.discovery.hudsonrocksearch - INFO - Hudson Rock processing completed successfully: 0 hosts, 0 IPs, 0 emails, 0 stealers
[*] Searching Hudsonrock. 
[*] Searching Otx. 
[*] Searching Rapiddns. 
[*] Searching Subdomaincenter. 
[*] Searching Robtex. 
[*] Searching Thc. 
No response from ThreatCrowd API for: zonetransfer.me
[*] Searching Threatcrowd. 
[*] Searching Urlscan. 
[*] Searching Subdomainfinderc99. 
[*] Windvane API key not found. Using limited unauthenticated access.
[*] Searching CRTsh. 
[*] API unavailable, using fallback subdomain pattern search...
[*] Searching Yahoo. 
[*] Found 2 subdomains using DNS fallback
[*] Searching Windvane. 
[*] Searching Waybackarchive. 

[*] ASNS found: 2
--------------------
AS16276
AS16509

[*] Interesting Urls found: 14
--------------------
http://alltcpportsopen.firewall.test.zonetransfer.me/
http://asfdbbox.zonetransfer.me/
http://canberra-office.zonetransfer.me/
http://dc-office.zonetransfer.me/
http://deadbeef.zonetransfer.me/
http://email.zonetransfer.me/
http://home.zonetransfer.me/
http://intns1.zonetransfer.me/
http://intns2.zonetransfer.me/
http://ipv6actnow.org.zonetransfer.me/
http://office.zonetransfer.me/
http://owa.zonetransfer.me/
http://staging.zonetransfer.me/
http://vpn.zonetransfer.me/

[*] No LinkedIn users found.



[*] LinkedIn Links found: 0
---------------------

[*] IPs found: 13
-------------------
13.227.246.59
217.147.177.157
217.147.180.162
2600:9000:206f:4200:7:60:4d00:93a1
2600:9000:206f:ba00:7:60:4d00:93a1
2600:9000:206f:c200:7:60:4d00:93a1
2600:9000:266e:1a00:7:60:4d00:93a1
2600:9000:277c:aa00:7:60:4d00:93a1
5.196.105.14
54.192.51.114
54.192.51.120

[*] Emails found: 2
----------------------
customer-service@zonetransfer.me
pippa@zonetransfer.me

[*] No people found.

[*] Hosts found: 29
---------------------
3dWww.zonetransfer.me
Info.zonetransfer.me
Www.zonetransfer.me
acme-challenge.zonetransfer.me
adonis.zonetransfer.me
alltcpportsopen.firewall.test.zonetransfer.me
asfdbbox.zonetransfer.me
brutus.zonetransfer.me
canberra-office.zonetransfer.me
cmdexec.zonetransfer.me
contact.zonetransfer.me
dc-office.zonetransfer.me
deadbeef.zonetransfer.me
email.zonetransfer.me
helena.zonetransfer.me
home.zonetransfer.me
internal.zonetransfer.me
intns1.zonetransfer.me
intns2.zonetransfer.me
ipv6actnow.org.zonetransfer.me
office.zonetransfer.me
owa.zonetransfer.me
sip.zonetransfer.me
sqli.zonetransfer.me
staging.zonetransfer.me
staging.zonetransfer.me:d3gdbrxsb9xhmf.cloudfront.net
tcp.zonetransfer.me
testing.zonetransfer.me
vpn.zonetransfer.me

[*] Performing SecurityScorecard scan...
Read api-keys.yaml from /etc/theHarvester/api-keys.yaml
An exception has occurred in SecurityScorecard scanning: 
[!] Missing API key for SecurityScorecard. 

[*] Performing BuiltWith scan...
Read api-keys.yaml from /etc/theHarvester/api-keys.yaml

[!] Missing API key for BuiltWith. 
```


| LINE                                                                                                                                                                                                                                                                                                                               | INFORMATION                                                                                                                                                                                                                                                                                                                                                       | 
|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [*] ASNS found: 2 <br>--------------------<br>AS16276<br>AS16509                                                                                                                                                                                                                                                                   | Shows the network ownership / who hosts the infrastructure (hosting providers).                                                                                                                                                                                                                                                                                   |
| [*] Interesting Urls found: 14<br>--------------------<br>http://alltcpportsopen.firewall.test.zonetransfer.me/<br>http://canberra-office.zonetransfer.me/<br>http://dc-office.zonetransfer.me/<br>http://office.zonetransfer.me/<br>http://owa.zonetransfer.me/<br>http://staging.zonetransfer.me/<br>http://vpn.zonetransfer.me/ | <br><br>firewall test<br>shows an office in canberra as well as dc (below)<br>-> also shows the naming convention of the company<br>the office portal (useful to try to get a login access)<br>owa = outlook web access -> another login portal<br>a staging environment (which probably has way less security or a login like admin:admin)<br>A VPN login portal |
| [*] IPs found: 13<br>-------------------<br>217.147.180.162<br>2600:9000:206f:4200:7:60:4d00:93a1                                                                                                                                                                                                                                  | It uses IPv4 and IPv6 and the addresses belong to cloud providers. It reveals the backend structure a bit more                                                                                                                                                                                                                                                    |
| [*] Emails found: 2<br>----------------------<br>customer-service@zonetransfer.me<br>pippa@zonetransfer.me                                                                                                                                                                                                                         | Emails we can contact for a phishing attack, social engineering, or even to guess the naming patterns for email addresses (like firstname@company.com)                                                                                                                                                                                                            |
| [*] Hosts found: 29<br>---------------------<br>cmdexec.zonetransfer.me<br>alltcpportsopen.firewall.test.zonetransfer.me<br>internal.zonetransfer.me<br>sqli.zonetransfer.me<br>vpn.zonetransfer.me                                                                                                                                | <br><br>a command execution host<br>the internal system<br>a firewal testing environment<br>an sql injection testing host<br>the vpn host                                                                                                                                                                                                                         |


<br> <br>

<hr>

### <a id="wafw00f"><span style="color: var(--title)">06.00 WAFW00F</a>

<hr><br>

```wafw00f``` is a <span style="color: var(--highlight)">web application firewall (WAF) fingerprinting toolkit</span> which will basically check for unusual server headers, cookies, custom block pages, and modified responses.

Basically, internally the command will send an HTTP request and then look at the response headers for things like : 

- ```Server : cloudflare``` -> WAF
- ```X-Akamai-Transformed``` -> Akamai infrastructre (for bot migration, DDoS filtering, Web Exploit Filtering, etc.).
- ```__cf_bm=``` -> bot management cookie

Using ```wafw00f -v [domain]``` will return a verbose response while using ```wafw00f -a [domain]``` will force wafw00f to try all the WAF detections tools it has.

```
wafw00f google.com

                ______
               /      \                                                      
              (  W00f! )                                                     
               \  ____/                                                      
               ,,    __            404 Hack Not Found                        
           |`-.__   / /                      __     __                       
           /"  _/  /_/                       \ \   / /                       
          *===*    /                          \ \_/ /  405 Not Allowed       
         /     )__//                           \   /                         
    /|  /     /---`                        403 Forbidden                     
    \\/`   \ |                                 / _ \                         
    `\    /_\\_              502 Bad Gateway  / / \ \  500 Internal Error    
      `_____``-`                             /_/   \_\\                      
                                                                             
                        ~ WAFW00F : v2.3.2 ~                                 
        The Web Application Firewall Fingerprinting Toolkit                  
                                                                             
[*] Checking https://google.com
[+] Generic Detection results:
[-] No WAF detected by the generic detection
[~] Number of requests: 7
```

<br><br>

```
wafw00f cloudfare.com 

                   ______
                  /      \                                                   
                 (  Woof! )                                                  
                  \  ____/                      )                            
                  ,,                           ) (_                          
             .-. -    _______                 ( |__|                         
            ()``; |==|_______)                .)|__|                         
            / ('        /|\                  (  |__|                         
        (  /  )        / | \                  . |__|                         
         \(_)_))      /  |  \                   |__|                         

                    ~ WAFW00F : v2.3.2 ~
    The Web Application Firewall Fingerprinting Toolkit                      
                                                                             
[*] Checking https://cloudfare.com
[+] The site https://cloudfare.com is behind Cloudflare (Cloudflare Inc.) WAF.
[~] Number of requests: 2
```


<br> <br>

<hr>

### <a id="whatweb"><span style="color: var(--title)">07.00 WHATWEB</a>

<hr><br>

```whatweb``` is a <span style="color: var(--highlight)">web technology fingerprinting tool</span>, which basically asls <span style="color: var(--highlight)">what technologies is X website using ?</span> It mainly identifies
- web servers
- CMS platforms
- frameworks
- analytics
- programming languages
- WAFs
- libraries
- CDN usage
- login systems

through
- HTTP headers
- HTML source
- cookies
- JavaScript files
- CMS signatures
- frameworks
- server banners
- metadata
- favicon hashes
- error messages

Note that : 

> whatweb is an in between between active and passive recon command since it pings the infrastructure directly

> whatweb also has agression levels (1-3), with 1 being stealthier and 3 being more requests but more detections.


```
whatweb google.com
http://google.com 
    [301 Moved Permanently] 
    Country[UNITED STATES][US], 
    HTTPServer[gws], 
    IP[142.250.139.101], 
    RedirectLocation[http://www.google.com/], 
        Title[301 Moved], 
    UncommonHeaders[content-security-policy-report-only], 
    X-Frame-Options[SAMEORIGIN], 
    X-XSS-Protection[0]
http://www.google.com/ 
    [200 OK] Cookies[AEC,NID,__Secure-STRP], 
    Country[UNITED STATES][US], 
    HTML5, 
    HTTPServer[gws], 
    HttpOnly[AEC,NID], 
    IP[142.251.152.119], 
    Script, 
    Title[Google], 
    UncommonHeaders[content-security-policy-report-only], 
    X-Frame-Options[SAMEORIGIN], 
    X-XSS-Protection[0]
```

| LINE                                                 | INFORMATION                                                                                                                    | 
|------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------|
| [301 Moved Permanently]                              | The webiste moved to a new address permanently                                                                                 |
| Country[UNITED STATES][US]                           | The infrastructure is located in the US                                                                                        |
| HTTPServer[gws]                                      | The web server used is google web server                                                                                       |
| IP[142.250.139.101]                                  | The old IP address for google.com                                                                                              |
| IP[142.251.152.119]                                  | The new IP address for google.com                                                                                              |
| [200 OK] Cookies[AEC,NID,__Secure-STRP]              | NID = google's tracking cookie, AEC = security-related cookie, __Secure[...] = uses HTTPS only and secure flags                |
| HttpOnly[AEC,NID]                                    | Only HTTP can access the cookies (not javascript), which prevents cookie theft                                                 |
| HTML5 & Script                                       | The website uses HTML5 and javascript                                                                                          |
| UncommonHeaders[content-security-policy-report-only] | google.com is using a special HTTP security header (which is used to monitoring browser activity as well as testing CSP rules) |
| X-Frame-Options[SAMEORIGIN]                          | Other websites cannot embed google.com in an iframe                                                                            |
| X-XSS-Protection[0]                                  | Disables the old XSS filters                                                                                                   |

<br> 

```
whatweb github.com
http://github.com 
    [301 Moved Permanently] 
    Country[UNITED STATES][US], 
    IP[140.82.112.4], 
    RedirectLocation[https://github.com/]
https://github.com/ 
     [200 OK] Content-Language[en-US], 
     Cookies[_gh_sess,_octo,logged_in], 
     Country[UNITED STATES][US], 
     Email[you@domain.com], 
     HTML5, 
     HTTPServer[github.com], 
     HttpOnly[_gh_sess,logged_in], 
     IP[140.82.112.4], 
     Open-Graph-Protocol[object][1401488693436528], 
     OpenSearch[/opensearch.xml], 
     Script[application/javascript,application/json], 
     Strict-Transport-Security[max-age=31536000; includeSubdomains; preload], 
     Title[GitHub · Change is constant. GitHub keeps you ahead. · GitHub], 
     UncommonHeaders[x-content-type-options,referrer-policy,content-security-policy,x-github-request-id], 
     X-Frame-Options[deny], 
     X-XSS-Protection[0]
```

| LINE                                                                                                | INFORMATION                                                                                                                       | 
|-----------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------|
| RedirectLocation[https://github.com/]                                                               | github forces HTTPS and users are automatically redirected to the HTTPS url                                                       |
| Cookies[_gh_sess,_octo,logged_in]                                                                   | github sessino cookies + tracking and analytics cookies (github uses cookie-based sessions, aka, auth is tracked through cookies) |
| Email[you@domain.com]                                                                               | email-like string detected by whatweb                                                                                             |
| HttpOnly[_gh_sess,logged_in]                                                                        | Impossible for JavaScript to steal the cookies (through document.cookie for example)                                              |
| HTTPServer[github.com]                                                                              | github hides what server it uses (apache, nginx, IIS, etc.)                                                                       |
| Open-Graph-Protocol[object][1401488693436528]                                                       | How the links appear when you send them through another platform                                                                  |
| Strict-Transport-Security[max-age=31536000; includeSubdomains; preload]                             | basically github forcing browsers to connect over HTTPS for 1 year (for the subdomains as well)                                   |
| UncommonHeaders[x-content-type-options,referrer-policy,content-security-policy,x-github-request-id] | nosniff = prevents MIME sniffing attacks, filters scripts, styles, frames, images, etc. to approved sources only                  |



```
whatweb demo.testfire.net
http://demo.testfire.net 
    [200 OK] Apache, 
    Cookies[JSESSIONID], 
    Country[UNITED STATES][US], 
    HTTPServer[Apache-Coyote/1.1], 
    HttpOnly[JSESSIONID], 
    IP[65.61.137.117], 
    Java, Title[Altoro Mutual]
```

| LINE                          | INFORMATION                                                                                                                                 | 
|-------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------|
| [200 OK] Apache               | running Apache ecosystem                                                                                                                    |
| Cookies[JSESSIONID]           | The server tracks users using server-side sessions                                                                                          |
| HTTPServer[Apache-Coyote/1.1] | the server is not Apache itself, but rather using the HTTP connector to connect to Apache Tomcat, which means the application is using Java |
| Java, Title[Altoro Mutual]    | The website is Java-based                                                                                                                   |

Therefore, we can infer that : 

Application: Altoro Mutual
<br>Backend: Java
<br>Container: Tomcat
<br>HTTP Connector: Apache-Coyote
<br>Session Framework: JSESSIONID
<br>Cookie Security: HttpOnly enabled
<br>Location: US
<br>IP: 65.61.137.117

```
whatweb -a 3 juice-shop.herokuapp.com
http://juice-shop.herokuapp.com/ 
    [200 OK] Country[UNITED STATES][US], 
    HTML5, HTTPServer[Heroku], 
    IP[54.220.192.176], 
    Script[module], 
    Title[OWASP Juice Shop], 
    UncommonHeaders[access-control-allow-origin,feature-policy,nel,report-to,reporting-endpoints,x-content-type-options,x-recruiting], 
    Via-Proxy[1.1 heroku-router], 
    X-Frame-Options[SAMEORIGIN]
```

| LINE                                                                                                                              | INFORMATION                                                                                                                                                                                                                                                                                             | 
|-----------------------------------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| HTML5, HTTPServer[Heroku]                                                                                                         | Heroku is a Platform-as-a-Service, which acts as a reverse-proxy, which will therefore hide the backend                                                                                                                                                                                                 |
| Via-Proxy[1.1 heroku-router]                                                                                                      | It confirms that the path is browser -> Heroku router -> Application container, which means Heroku will handle all the traffic before the application                                                                                                                                                   |
| Script[module]                                                                                                                    | Modern Javascript application (such as React.js)                                                                                                                                                                                                                                                        |
| IP[54.220.192.176]                                                                                                                | IP belongs to AWS, which means Heroku is running on AWS infrastructure                                                                                                                                                                                                                                  |
| UncommonHeaders[access-control-allow-origin,feature-policy,nel,report-to,reporting-endpoints,x-content-type-options,x-recruiting] | feature policy (also named permission-policy) = the site disables browser features such as camera or microphone<br>NEL (Network Error Loggin) = browser will send a report for any failures regarding the site<br>report-to = where the browser will report the errors<br>X-recruiting = dev easter egg |

<br><br>

### <a id="previous"><span style="color: var(--link)">[<- Previous](../README.md)</a></span> <a id="next"><span style="color: var(--link)">[Next ->](./01_BASE_COMPONENTS.md)</a></span>

<hr><br></span>
