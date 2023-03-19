# DNS Authority Sink

DNS Authority Sink is a simple authoritative DNS server that utilizes the dns/meig library written in Go. It serves as a DNS sinkhole by responding to both recursive and authoritative queries with a default answer, which can be overridden with records stored in a SQLite database. Additionally, all DNS queries are logged to the SQLite database.

## Features

- Utilizes the dns/meig library for DNS functionality.
- Responds to both recursive and authoritative queries with a default answer.
- Allows overriding the default answer with records in a SQLite database.
- Logs all DNS queries to the SQLite database, including source IP addresses, DNS query names, and request timestamps.
- Creates and applies the SQLite database schema if the database does not exist.

## Prerequisites

- Go 1.16 or later

## Installation

1. Clone the repository:
```
git clone https://github.com/yourusername/dns-authority-sink.git
cd dns-authority-sink
go build -o dns-authority-sink
```

## Usage

1. Run the dns-authority-sink executable (you will need to run this with elevated privledges to bind to port 53):
```
sudo ./dns-authority-sink
```
2. The server will start listening on port 53 (UDP) for incoming DNS requests.
3. To add custom records to the SQLite database, use an SQLite client to insert records into the dns_records table:
```
echo "INSERT INTO dns_records (qname, answer) VALUES ('www.domain.com', '10.1.1.1');" | sqlite3 dns.db
echo "INSERT INTO dns_records (qname, answer) VALUES ('www.domain2.com', '10.1.2.3');" | sqlite3 dns.db
```
4. Query the server using a DNS client, such as dig:
```
dig @localhost www.somedomain.com
```