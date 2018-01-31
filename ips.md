---

copyright:
  years: 2017, 2018
lastupdated: "2018-01-16"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# IBM Cloud IP Ranges

A frequently asked question is **What IP ranges do I allow through the firewall?**. The following list contains the full range of IPs to use with the Virtual Router Appliance.

## Frontend (public) network

|Datacenter|City|State|Country|IP Range|
|---|---|---|---|---|
|ams01|Amsterdam|-|NLD|159.253.158.0/23|
|ams03|Amsterdam|-|NLD|159.8.198.0/23|
|che01|Chennai|-|IND|169.38.118.0/23|
|dal01|Dallas|Texas|USA|66.228.118.0/23|
|dal05|Dallas|Texas|USA|173.192.118.0/23|
|dal06|Dallas|Texas|USA|184.172.118.0/23|
|dal07|Dallas|Texas|USA|50.22.118.0/23|
|dal08|Dallas|Texas|USA|192.255.18.0/24|
|dal09|Dallas|Texas|USA|198.23.118.0/23|
|dal10|Dallas|Texas|USA|169.46.118.0/23|
|dal12|Dallas|Texas|USA|169.47.118.0/23|
|dal13|Dallas|Texas|USA|169.48.118.0/24|
|fra02|Frankfurt|-|DEU|159.122.118.0/23|
|hkg02|Hong Kong|-|CHN|119.81.138.0/23|
|hou02|Houston|Texas|USA|173.193.118.0/23|
|lon02|London|-|ENG|5.10.118.0/23|
|lon04|London|-|ENG|169.62.118.0/24|
|lon06|London|-|ENG|158.176.118.0/23|
|mel01|Melbourne|-|AUS|168.1.118.0/23|
|mex01|Mexico City|-|MEX|169.57.118.0/23|
|mil01|Milan|-|ITA|159.122.138.0/23|
|mon01|Montreal|-|CAN|169.54.118.0/23|
|par01|Paris|-|FRA|159.8.118.0/23|
|osl01|Oslo|-|NOR|169.51.118.0/24|
|sao01|São Paulo|-|BRA|169.57.138.0/23|
|sea01|Seattle|Washington|USA|67.228.118.0/23|
|seo01|Seoul|-|KOR|169.56.118.0/24|
|sjc01|San Jose|California|USA|50.23.118.0/23|
|sjc03|San Jose|California|USA|169.45.118.0/23|
|sjc04|San Jose|California|USA|169.62.118.0/24|
|sng01|Jurong East|-|SGP|174.133.118.0/23|
|syd01|Sydney|-|AUS|168.1.18.0/23|
|syd04|Sydney|-|AUS|130.198.118.0/23|
|tok02|Tokyo|-|JPN|161.202.118.0/23|
|tor01|Toronto|-|CAN|158.85.118.0/23|
|wdc01|Washington D.C.|-|USA|208.43.118.0/23|
|wdc03|Washington D.C.|-|USA|192.255.38.0/24|
|wdc04|Washington D.C.|-|USA|169.55.118.0/23|
|wdc06|Washington D.C.|-|USA|169.60.118.0/23|
|wdc07|Washington D.C.|-|USA|169.61.118.0/23|

Ports to allow:<br>
All TCP/UDP ports<br>
ICMP – ping (for support troubleshooting and monitoring)

## Load Balancer IPs

|Datacenter|City|State|Country|IP Range|
|---|---|---|---|---|
|ams01|Amsterdam|-|NLD|159.253.157.0/24|
|ams03|Amsterdam|-|NLD|159.8.197.0./24|
|che01|Chennai|-|IND|169.38.117.0/24|
|dal01|Dallas|Texas|USA|67.228.66.0/24, 75.126.76.0/24, 174.35.17.0/24, 208.43.15.0/24|
|dal05|Dallas|Texas|USA|50.23.203.0/24, 108.168.157.0/24 173.192.117.0/24, 192.155.205.0/24|
|dal06|Dallas|Texas|USA|184.172.117.0/24|
|dal07|Dallas|Texas|USA|50.22.117.0/24|
|dal09|Dallas|Texas|USA|169.46.187.0/24, 198.23.117.0/24|
|dal10|Dallas|Texas|USA|169.46.117.0/24|
|dal12|Dallas|Texas|USA|169.47.117.0/24|
|dal13|Dallas|Texas|USA|169.48.117.0/24|
|fra02|Frankfurt|-|DEU|159.122.117.0/24|
|hkg02|Hong Kong|-|CHN|119.81.137.0/24|
|hou02|Houston|Texas|USA|173.193.118.0/23|
|lon02|London|-|ENG|5.10.117.0/24|
|lon04|London|-|ENG|158.175.117.0/24|
|lon06|London|-|ENG|158.176.117.0/24|
|mel01|Melbourne|-|AUS|168.1.117.0/24|
|mex01|Mexico City|-|MEX|169.57.117.0/24|
|mil01|Milan|-|ITA|159.122.137.0/24|
|mon01|Montreal|-|CAN|169.54.117.0/24|
|par01|Paris|-|FRA|159.8.117.0/24|
|osl01|Oslo|-|NOR|169.51.117.0/24|
|sao01|São Paulo|-|BRA|169.57.137.0/24|
|sea01|Seattle|Washington|USA|67.228.117.0/24|
|seo01|Seoul|-|KOR|169.56.117.0/24|
|sjc01|San Jose|California|USA|50.23.117.0/24|
|sjc03|San Jose|California|USA|169.45.117.0/24|
|sng01|Jurong East|-|SGP|174.133.117.0/24|
|syd01|Sydney|-|AUS|168.1.17.0/24|
|syd04|Sydney|-|AUS|130.198.117.0/24|
|tok02|Tokyo|-|JPN|161.202.117.0/24|
|tor01|Toronto|-|CAN|158.85.117.0/24|
|wdc01|Washington D.C.|-|USA|50.22.248.0/25, 169.54.27.0/24, 198.11.250.0/24, 208.43.117.0/24|
|wdc04|Washington D.C.|-|USA|169.55.117.0/24|
|wdc06|Washington D.C.|-|USA|169.60.117.0/24|
|wdc07|Washington D.C.|-|USA|169.61.117.0/24|

## DOS Mitigation Systems

|Datacenter|City|State|Country|IP Range|
|---|---|---|---|---|
|AMS|Amsterdam|-|NLD|159.253.156.0/24, 159.8.196.0/24|
|CHE|Chennai|-|IND|169.38.116.0/24|
|DAL|Dallas|Texas|USA|75.126.61.0/24|
|FRA|Frankfurt|-|DEU|159.122.116.0/24|
|HKG|Hong Kong|-|CHN|119.81.136.0/24|
|HOU|Houston|Texas|USA|173.193.116.0/24|
|KOR|Seoul|-|South Korea|169.56.116.0/24|
|LON|London|-|ENG|5.10.116.0/24|
|MEL|Melbourne|-|AUS|168.1.116.0/24|
|MEX|Mexico City|-|MEX|169.57.116.0/24|
|MIL|Milan|-|ITA|159.122.136.0/24|
|MON|Montreal|-|CAN|169.54.116.0/24|
|NOR|Oslo|-|Norway|169.56.116.0/24|
|PAR|Paris|-|FRA|159.8.116.0/24|
|SAO|São Paulo|-|BRA|169.57.136.0/24|
|SEA|Seattle|Washington|USA|50.23.167.0/24|
|SJC|San Jose|California|USA|50.23.116.0/24|
|SNG|Jurong East|-|SGP|174.133.116.0/24|
|SYD|Sydney|-|AUS|168.1.16.0/24|
|TOK|Tokyo|-|JPN|161.202.116.0/24|
|TOR|Toronto|-|CAN|158.85.116.0/24|
|WDC|Washington D.C.|-|USA|50.22.255.0/24|

Ports to allow: <br>
All TCP/UDP ports

## Vulnerability Scans
To ensure successful completion of a vulnerability scan, please permit access for the following IP address: **173.192.255.232** and **172.17.19.38**.

## Backend (private) Network

IP block: your private IP block for server to server communications (10.X.X.X/X)<br>
Ports to allow:<br>
ICMP – ping (for support troubleshooting)<br>
All TCP/UDP ports<br>
For EVault port-specific information, [click here](https://console.bluemix.net/docs/infrastructure/Backup/evault-port-information.html#evault-port-information).

## Service Network (on backend/private network)
Be sure to add rules for both DAL01 and the location of your server. If your server is in AMS01, you'll need to add rules allowing traffic from both DAL01 and AMS01.

For Flex Image Provisions (both image creation and server provisions), it is also necessary to allow DAL05 through as well.

|Datacenter|City|State|Country|IP Range|
|---|---|---|---|---|
|All|-|-|-|161.26.0.0/16
|ams01|Amsterdam|-|NLD|10.2.64.0/20|
|ams03|Amsterdam|-|NLD|10.3.128.0/20|
|che01|Chennai|-|IND|10.200.16.0/20|
|dal01|Dallas|Texas|USA|10.0.64.0/19|
|dal05|Dallas|Texas|USA|10.1.128.0/19|
|dal06|Dallas|Texas|USA|10.2.128.0/20|
|dal07|Dallas|Texas|USA|10.1.176.0/20|
|dal08|Dallas|Texas|USA|100.100.0.0/20|
|dal09|Dallas|Texas|USA|10.2.112.0/20 and 10.3.192.0/24|
|dal10|Dallas|Texas|USA|10.200.80.0/20|
|dal12|Dallas|Texas|USA|10.200.112.0/20|
|dal13|Dallas|Texas|USA|10.200.128.0/20|
|fra02|Frankfurt|-|DEU|10.3.80.0/20|
|hkg02|Hong Kong|-|CHN|10.2.160.0/20|
|hou02|Houston|Texas|USA|10.1.160.0/20|
|lon02|London|-|ENG|10.1.208.0/20|
|lon04|London|-|ENG|10.201.32.0/20|
|lon06|London|-|ENG|10.201.64.0/20|
|mel01|Melbourne|-|AUS|10.2.80.0/20|
|mex01|Mexico City|-|MEX|10.2.176.0/20|
|mil01|Milan|-|ITA|10.3.144.0/20|
|mon01|Montreal|-|CAN|10.3.112.0/20|
|par01|Paris|-|FRA|10.2.144.0/20|
|osl01|Oslo|-|NOR|10.200.96.0/20|
|sao01|São Paulo|-|BRA|10.200.0.0/20|
|sea01|Seattle|Washington|USA|10.1.64.0/19|
|seo01|Seoul|-|KOR|10.200.64.0/20|
|sjc01|San Jose|California|USA|10.1.192.0/20|
|sjc03|San Jose|California|USA|10.3.176.0/20|
|sjc04|San Jose|California|USA|10.201.80.0/20|
|sng01|Jurong East|-|SGP|10.2.32.0/20|
|syd01|Sydney|-|AUS|10.3.96.0/20|
|syd04|Sydney|-|AUS|10.201.16.0/20|
|tok02|Tokyo|-|JPN|10.3.64.0/20|
|tor01|Toronto|-|CAN|10.2.48.0/20|
|wdc01|Washington D.C.|-|USA|10.1.96.0/19|
|wdc03|Washington D.C.|-|USA|100.100.32.0/20|
|wdc04|Washington D.C.|-|USA|10.3.160.0/20 and 10.201.0.0/20|
|wdc06|Washington D.C.|-|USA|10.200.160.0/20|
|wdc07|Washington D.C.|-|USA|10.200.176.0/20|

## SSL VPN network (on backend/private network)
ICMP – ping (for support troubleshooting) <br>
All TCP/UDP ports (for access from your local workstation)

## SSL VPN Datacenters

|Datacenter|City|State|Country|IP Range|
|---|---|---|---|---|
|ams01|Amsterdam|-|NLD|10.2.200.0/23|
|ams03|Amsterdam|-|NLD|10.3.220.0/24|
|che01|Chennai|-|IND|10.200.232.0/24|
|dal01|Dallas|Texas|USA|10.1.0.0/23|
|dal05|Dallas|Texas|USA|10.1.24.0/23|
|dal06|Dallas|Texas|USA|10.2.208.0/23|
|dal07|Dallas|Texas|USA|10.1.236.0/24|
|dal09|Dallas|Texas|USA|10.2.232.0/24|
|dal10|Dallas|Texas|USA|10.200.228.0/24|
|dal12|Dallas|Texas|USA|10.200.216.0/22|
|dal13|Dallas|Texas|USA|10.200.212.0/22|
|fra02|Frankfurt|-|DEU|10.2.236.0/24|
|hkg02|Hong Kong|-|CHN|10.2.216.0/24|
|hou02|Houston|Texas|USA|10.1.56.0/23|
|lon02|London|-|ENG|10.2.220.0/24|
|lon04|London|-|ENG|10.200.196.0/24|
|lon06|London|-|ENG|10.3.200.0/24|
|mel01|Melbourne|-|AUS|10.2.228.0/24|
|mex01|Mexico City|-|MEX|10.3.232.0/24|
|mil01|Milan|-|ITA|10.3.216.0/24|
|mon01|Montreal|-|CAN|10.3.224.0/24|
|par01|Paris|-|FRA|10.3.236.0/24|
|osl01|Oslo|-|NOR|10.200.220.0/22|
|sao01|São Paulo|-|BRA|10.200.236.0/24|
|sea01|Seattle|Washington|USA|10.1.8.0/23|
|seo01|Seoul|-|KOR|10.200.224.0/22|
|sjc01|San Jose|California|USA|10.1.224.0/23|
|sjc03|San Jose|California|USA|10.3.204.0/24|
|sjc04|San Jose|California|USA|10.200.192.0/24|
|sng01|Jurong East|-|SGP|10.2.192.0/23|
|syd01|Sydney|-|AUS|10.3.228.0/24|
|syd04|Sydney|-|AUS|10.200.200.0/24|
|tok02|Tokyo|-|JPN|10.2.224.0/24|
|tor01|Toronto|-|CAN|10.1.232.0/24|
|wdc01|Washington D.C.|-|USA|10.1.16.0/23|
|wdc04|Washington D.C.|-|USA|10.3.212.0/24|
|wdc06|Washington D.C.|-|USA|10.200.208.0/24|
|wdc07|Washington D.C.|-|USA|10.200.204.0/24|

## SSL VPN POPs

|POP|City|State|Country|IP Range|
|---|---|---|---|---|
|atl01|Atlanta|Georgia|USA|10.1.41.0/24|
|chi01|Chicago|Illinois|USA|10.1.49.0/24|
|den01|Denver|Colorado|USA|10.1.53.0/24|
|lax01|Los Angeles|California|USA|10.1.33.0/24|
|mia01|Miami|Florida|USA|10.1.37.0/24|
|nyc01|New York|New York|USA|10.1.45.0/24|

## PPTP VPN Datacenters

|Datacenter|City|State|Country|IP Range|
|---|---|---|---|---|
|ams01|Amsterdam|-|NLD|10.2.203.0/24|
|ams03|Amsterdam|-|NLD|10.3.221.0/24|
|che01|Chennai|-|IND|10.200.233.0/24|
|dal01|Dallas|Texas|USA|10.1.3.0/24|
|dal05|Dallas|Texas|USA|10.1.27.0/24|
|dal06|Dallas|Texas|USA|10.2.211.0/24|
|dal07|Dallas|Texas|USA|10.1.238.0/24|
|dal09|Dallas|Texas|USA|10.2.233.0/24|
|dal10|Dallas|Texas|USA|10.200.229.0/24|
|dal12|Dallas|Texas|USA|10.200.217.0/24|
|fra02|Frankfurt|-|DEU|10.2.237.0/24|
|hkg02|Hong Kong|-|CHN|10.2.217.0/24|
|hou02|Houston|Texas|USA|10.1.59.0/24|
|lon02|London|-|ENG|10.2.221.0/24|
|lon04|London|-|ENG|10.200.197.0/24|
|lon06|London|-|ENG|10.3.201.0/24|
|mel01|Melbourne|-|AUS|10.2.229.0/24|
|mil01|Milan|-|ITA|10.3.217.0/24|
|mon01|Montreal|-|CAN|10.3.255.0/24|
|mex01|Mexico City|-|MEX|10.3.233.0/24|
|par01|Paris|-|FRA|10.3.237.0/24|
|sao01|São Paulo|-|BRA|10.200.237.0/24|
|sea01|Seattle|Washington|USA|10.1.11.0/24|
|seo01|-|South Korea|KOR|10.200.225.0/24|
|sjc01|San Jose|California|USA|10.1.227.0/24|
|sjc03|San Jose|California|USA|10.3.205.0/24|
|sng01|Jurong East|-|SGP|10.2.195.0/24|
|syd01|Sydney|-|AUS|10.3.229.0/24|
|syd04|Sydney|-|AUS|10.200.201.0/24|
|tok02|Tokyo|-|JPN|10.2.225.0/24|
|tor01|Toronto|-|CAN|10.1.233.0/24|
|wdc01|Washington D.C.|-|USA|10.1.19.0/24|
|wdc04|Washington D.C.|-|USA|10.3.213.0/24|
|wdc06|Washington D.C.|-|USA|10.200.209.0/24|
|wdc07|Washington D.C.|-|USA|10.200.205.0/24|

## PPTP VPN POPs

|POP|City|State|Country|IP Range|
|---|---|---|---|---|
|atl01|Atlanta|Georgia|USA|10.1.42.0/24|
|chi01|Chicago|Illinois|USA|10.1.40.0/24|
|den01|Denver|Colorado|USA|10.1.54.0/24|
|lax01|Los Angeles|California|USA|10.1.34.0/24|
|mia01|Miami|Florida|USA|10.1.38.0/24|
|nyc01|New York|New York|USA|10.1.46.0/24|

## Legacy Networks

|IP Range|
|---|
|12.96.160.0/24|
|66.98.240.192/26|
|67.18.139.0/24|
|67.19.0.0/24|
|70.84.160.0/24|
|70.85.125.0/24|
|75.125.126.8|
|209.85.4.0/26|
|216.12.193.9|
|216.40.193.0/24|
|216.234.234.0/24|

If your server uses a **Red Hat Enterprise Linux (RHEL)** license provided by SoftLayer, you will additionally need to allow access to the service network as follows, otherwise updates and licensing will not function properly:

|Server Location|Allow Service Network for this datacenter|
|---|---|
|Amsterdam (AMS01, AMS03)|LON02|
|Chennai (CHE01)|TOK02 and SYD01|
|Dallas (DAL01, DAL05, DAL07, DAL09)|DAL09|
|Dallas (DAL06, DAL10)|DAL06|
|Houston (HOU02)|DAL09|
|Frankfurt (FRA02)|LON02|
|Hong Kong (HKG02)|TOK02 and SYD01|
|London (LON02)|LON02|
|Melbourne (MEL01)|SYD01|
|Mexico (MEX01)|DAL06|
|Milan (MIL01)|LON02|
|Montreal (MON01)|MON01|
|Paris (PAR01)|LON02|
|San Jose (SJC01, SJC03)|SJC03 and DAL06|
|Sao Paulo (SAO01)|SAO01 and DAL09|
|Singapore (SNG01)|TOK02 and SYD01|
|Seattle (SEA01)|SJC03 and DAL06|
|Sydney (SYD01, SYD04)|SYD01|
|Toronto (TOR01)|TOR01|
|Washington DC (WDC01, WDC04, WDC06, WDC07)|MON01|
|Any DC Not Listed Above|DAL09|
