# Redeemer — Hack The Box Write-Up
Overview

Redeemer is a Very Easy Linux machine on Hack The Box designed to introduce users to the concept of exploiting an exposed Redis database.

The machine demonstrates how a misconfigured Redis service that is accessible from the network can lead to sensitive data exposure.

Key learning points include:

- Service enumeration
- Identifying Redis services
- Connecting to Redis without authentication
- Extracting sensitive data from a key-value database

# Reconnaissance

The first step in any penetration test is identifying open ports and services running on the target system. This can be done using Nmap.

nmap -sC -sV <TARGET_IP>

Example:

nmap -sC -sV 10.129.16.95

Example output:

6379/tcp open  redis  Redis key-value store

Port 6379 is the default port used by Redis, which is an in-memory key-value database.

The presence of this service indicates that the target system is running a Redis instance.

# Service Enumeration

Once Redis is identified, the next step is to attempt a connection using the Redis command-line interface.

redis-cli -h <TARGET_IP>

Example:

redis-cli -h 10.129.16.95

If authentication is not configured, the connection will succeed and display the Redis prompt:

10.129.16.95:6379>

This indicates that the database is accessible without credentials.

# Enumerating the Redis Database

Redis stores data using a key-value structure.

To list all available keys in the database:

KEYS *

Example output:

1) "flag"

This reveals a key named flag stored in the database.

# Retrieving the Flag

After identifying the key, the stored value can be retrieved using the GET command.

GET flag

Example output:
{xxxxxxxxxxxxxxxx}

This value is the flag required to complete the machine.

# Security Implications

Exposing Redis to external networks without authentication is a serious security misconfiguration.

Possible risks include:

- Data Exposure
- Attackers can read all stored data inside the Redis database.
- Session Hijacking (Many web applications store session tokens in Redis. An attacker could steal session tokens and impersonate legitimate users.)
- Data Manipulation (Attackers can modify database values, potentially escalating privileges or altering application behavior.)
- Remote Code Execution (In some configurations, Redis can be abused to write files to the system and even add malicious SSH keys.)

# Lessons Learned

The Redeemer machine demonstrates several important penetration testing concepts:

- Identifying services based on open ports
- Understanding common service misconfigurations
- Enumerating accessible services manually
- Extracting sensitive data from exposed databases

Many real-world compromises occur due to simple misconfigurations like this.

Conclusion

Redeemer is an introductory machine that highlights the risks of exposing internal services to external networks.

By performing basic reconnaissance and interacting with an exposed Redis service, it is possible to retrieve sensitive data without exploiting complex vulnerabilities.

This machine reinforces the importance of proper service configuration and network segmentation.
