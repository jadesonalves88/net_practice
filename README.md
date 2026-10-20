*This project has been created as part of the 42 curriculum by jaalves-.*

# NetPractice

## Description

NetPractice is an introductory networking project from the 42 curriculum. Its goal is to teach the basics of TCP/IP networking through practice: configuring IP addresses, subnet masks, routing tables and default gateways so that simulated networks function correctly.

The project provides a local training web interface with 10 levels (10 exercises). Each level shows a non-functioning network diagram (hosts, switches, routers and an Internet node) with one or more communication goals. The student must edit the unshaded fields (IP addresses, masks, routes and gateways) until every goal reports `OK`, using the on-page logs to understand errors such as invalid IP, duplicate IP, missing gateway, loop, or unreachable destination.

This repository contains the 10 solved configuration files for login `jaalves-`, one per level, exported with the `Get my config` button.

## Instructions

No compilation or installation is required. Only a web browser and Python 3 are needed.

### 1. Run the training interface

From the folder where the NetPractice files were extracted:

```sh
./run.sh
```

This script starts a local web server and opens the training page in your browser.

If `run.sh` does not work, run manually:

```sh
python3 -m http.server 49242
```

Then open `http://localhost:49242` in your browser (you may change the port number).

### 2. Solve a level

1. Enter your intranet login (here `jaalves-`) in the `Training` tab and click `Start!`. The interface generates your personal addressing plan from your login. It is very important to enter your login, otherwise the configuration is fully random and cannot be evaluated.
2. Open Level 1 to Level 10 in order.
3. For each level, read the goals at the top of the page.
4. Modify the editable (unshaded) IP, Mask, Route and Gateway fields.
5. Click `Check again` to simulate the network and read the logs at the bottom of the page.
6. When all goals are `OK`, a `Next level` button appears.

During peer evaluation, three random levels from 6 to 10 must be solved in 15 minutes in the `Evaluation` tab, without external tools (a simple calculator such as `bc` is tolerated).

### 3. Export configurations

Before leaving each successful level, click `Get my config`. This downloads a `levelN.json` file containing only the fields you were allowed to edit.

### 4. Submission requirements

Submission details: 10 exported configuration files (one per level) must be placed at the repository root:

```text
./README.md
./level1.json
./level2.json
./level3.json
./level4.json
./level5.json
./level6.json
./level7.json
./level8.json
./level9.json
./level10.json
```

Only the work inside the Git repository is evaluated. Double-check file names.

## Levels overview

- Level 1: two pairs of directly connected hosts. Fix invalid host octets so A/B share the same /24 and C/D share the same /16.
- Level 2: fix an invalid mask and loopback `127.x.x.x` addresses. Put A/B in the same /27 and C/D in the same /30 with private `10.0.0.1/30` and `10.0.0.2/30`.
- Level 3: three hosts on one switch. Align all masks to /25 and all IPs to the same `104.198.x.0/25` network.
- Level 4: two hosts and a router on one switch. Align A, B and R1 to the same /28 network.
- Level 5: two hosts separated by a router. Configure A/R1 in the same /25, B/R2 in the same /18, A default route via R1 and B gateway via R2.
- Level 6: host behind a router to the Internet. Configure A mask /25, R1 IP, A default via R1, R default via Internet, and Internet return route `x.x.x.128/25` via R2.
- Level 7: two routers in series (A-R1-R2-C). Build three /30 subnets (`x.x.14.0/30`, `x.x.14.252/30`, `x.x.15.0/30`) and static routes on R1/R2 plus default gateways on A/C.
- Level 8: two routers plus Internet with three goals (C-D, C-Internet, D-Internet). Build `/27`, `/28` and `/30` subnets under a `/26` supernet, default routes on C/D/R2 and a return route on Internet via R1.
- Level 9: switch LAN plus two routers plus Internet with six goals. Build LAN `x.198.x.0/25`, link `x.x.18.252/30`, C-net `11.0.0.0/24`, D-net `x.x.64.0/18`, with matching static routes and Internet return routes.
- Level 10: switch LAN plus two routers plus Internet with seven goals. Build LAN `x.x.150.0/25`, link `x.x.150.252/30`, H3-net `x.x.151.0/24`, H4-net `x.x.150.128/26`, with H3 gateway, R1 route to H3, and Internet supernet return `x.x.150.0/23`.

## Resources

Networking concepts studied in this project:

- TCP/IP addressing: IPv4 address structure, network vs host bits, public vs private ranges (`10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16`), loopback `127.0.0.0/8` rejected by the simulator.
- Subnet masks: dotted notation (`255.255.255.0`) and CIDR (`/24`, `/25`, `/27`, `/28`, `/30`), network and broadcast addresses (host bits all 0 or all 1 are invalid), subnetting to fit hosts, switches and point-to-point router links.
- Default gateways and routing tables: `default` / `0.0.0.0/0` routes, longest-prefix / first-match behavior, gateway must share a subnet with one local interface, static routes on routers for remote networks.
- Routers and switches: switches flood to all ports (same broadcast domain, same subnet required), routers forward between subnets via routing tables, Internet node does not route private addresses.
- OSI layers: L1/L2 connectivity simulated by links, L3 IPv4 addressing and routing as the focus of the exercises.

Classic references:

- 42 NetPractice training interface (`run.sh`, `index.html`, `js/sim.js`) and `en.subject-Netpractice.txt`.
- IETF RFC 791 (Internet Protocol), RFC 950 (Subnetting), RFC 1918 (Private Address Space).
- Cisco: "IP Addressing and Subnetting for New Users".
- LearnSubit / Practical Networking tutorials on subnetting and gateway behavior.

### AI usage

AI was used only as a productivity aid, and every output was manually reviewed and tested:

- Drafting this `README.md` structure to match the subject `Readme Requirements` (Description, Instructions, Resources, submission details).
- Generating a local Node.js reimplementation of `js/show.js` (`hash_login`, `my_random`, `random_repl`) and `js/sim.js` (`ip_to_int`, `mask_to_int`, `rec_route`) to recompute the per-login base addresses for `jaalves-` and to validate the 10 exported `level*.json` files until all goals reported `OK`.
- No level solution was blindly copied: each subnet, mask and route was reasoned from the level topology and then verified with the simulator. Final exported JSONs contain only the editable fields, in the same format as the `Get my config` button.
