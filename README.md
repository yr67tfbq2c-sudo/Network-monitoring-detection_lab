# Network-monitoring-detection (Home cybersecurity lab)
this is network monitoring lab, using  Ubuntu servers,to stimulate a Hydra attack  in a safe environment.

## Architecture
- **Victim VM**: Ubuntu Server 24.04 ARM64, Apache2
- **Attacker VM**: Ubuntu Server 24.04 ARM64, Hydra, Nmap
- **Sensor VM**: Ubuntu Server 24.04 ARM64, Suricata, tcpdump

## Hardware/Software
- Host: Apple M3 Mac
- Virtualization: UTM
- Network: 192.168.64.0/24 (Shared Network)

## Tools Used
- Suricata (IDS/IPS)
- Hydra (Password cracker)
- Nmap (Network scanner)
- Apache2 (Web server)

# Issues 
- suricata roadblock
- - the second connection.
- the first issue was "password not found/ 0 password match" in my password.txt list.
- ssh connection refused when using Hydra to attack the victim.

# Troubleshooting steps taken.
changed Suricata interface to allow it to run after the first failure, changed the HOME_NET ip address to the internal network to reduce false positives, changed my interface from eth0 to esp01 to ensure Suricata is running and doesn’t run into a problem — Yess it works!!!

The Hydra roadblock:

1- I ran into a problem while running the hydra command, “ hydra -l [ username ] -p password.txt ssh://192.168.64.x —-> the issue was that the letter wasn’t capitalized, so hydra typed in the [ username of my server] and password.txt as the literal password..simply because I didn’t capitalized the “P” it shout have been “-P’  not ”-p”

2- Maybe the password list was too long, and I should reduce it to at least 5-10 —> it worked, but the connection was refused.
      - Let’s try pinging the victim address and see if it works —>  pinging does work
      - Maybe the ssh connection is broken between the two ( check the system)? ——> [cmd: “sudo systemctl status ssh] ——> ssh.service (active)…
      - Let’s check if ssh is listening [ cmd: “sudo ss -tlnp | grep :22 ]
              - “sudo ss -tlnp” = this is the equivalent of checking which port is listening 
              - “ | grep :22 ” ( it’s not “i”, it’s the vertical bar “|”) =  this is for port 22 to check ssh

      - Check the Firewall [ cmd: sudo ufw status ] ——>  “ status inactive.” (good, I want the attacker to have full control to demolishthe victim, it’s not blocking ssh). 

 3-  allow the attacker to establish the connection with the victim ——>  connection established…let’s try again…connection refused again!


The nmap scan shows what I expected it to show. 

4 - it’s not working again!!!
5-Nevermind, the password to my victim server was not in the password.txt list I created. That was dumb of me…..STILL DOESN’T WORK. 

## Current Status
Working on SSH brute force detection. Encountering intermittent "connection refused" - investigating SSH rate limiting.

# Author
linkedin.com/in/yvan-obama. 
Email: yvan.obama@outlook.com


# License
This project is open source and available for educational purposes.

Built this for better understanding of Linux, pen testing and advancement in cybersecuirty
