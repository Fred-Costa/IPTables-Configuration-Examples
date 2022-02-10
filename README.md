# IPTables-Configuration-Examples


## Block Everything
iptables –P INPUT DROP
iptables –P OUTPUT DROP
iptables –P FORWARD DROP

## Para conseguir ter acesso a si mesmo 
iptables –A INPUT –i lo –j ACCEPT
iptables –A OUTPUT –o lo –j ACCEPT

## Allow ICMP
iptables -A INPUT -p icmp  -j ACCEPT
iptables -A OUTPUT -p icmp -j ACCEPT
iptables -A FORWARD -p icmp -j ACCEPT

## Allow Individual Rule
iptables –A INPUT -i * –p tcp –-dport * -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT

iptables –A OUTPUT -o * –p tcp –-sport * –m state --state RELATED,ESTABLISHED –j ACCEPT

iptables –A FORWARD -i * –p tcp –-dport * -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT

iptables –A FORWARD -o * –p tcp –-sport * –m state --state RELATED,ESTABLISHED –j ACCEPT

iptables –A FORWARD -i * –p tcp –-dport * -m state --state RELATED,ESTABLISHED -j ACCEPT

iptables –A FORWARD -o * –p tcp –-sport * –m state --state NEW,RELATED,ESTABLISHED –j ACCEPT


## Allow Rules of Multiple Ports
iptables -A INPUT -p tcp/udp -i interface -m  multiport --dport X,X,X,X,X  -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT

iptables -A OUTPUT -p tcp/udp -o interface -m  multiport --sport X,X,X,X,X -m state --state RELATED,ESTABLISHED -j ACCEPT

iptables -A FORWARD -p tcp/udp -i interface -m  multiport --dport X,X,X,X,X  -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT

iptables -A FORWARD -p tcp/udp -o interface -m  multiport --sport X,X,X,X,X -m state --state RELATED,ESTABLISHED -j ACCEPT

iptables -A FORWARD -p tcp/udp -i interface -m  multiport --dport X,X,X,X,X  -m state --state RELATED,ESTABLISHED -j ACCEPT

iptables -A FORWARD -p tcp/udp -o interface -m  multiport --sport X,X,X,X,X -m state --state NEW,RELATED,ESTABLISHED -j ACCEPT

## NAT/PAT - OUTSIDE
iptables -t nat -A POSTROUTING -o interface -j MASCARADE

## NAT - INSIDE - PORT FORWADING - Multiple Ports
iptables -t nat -A PREROUTING -i interface -p tcp/udp -m multiport --dport X,X -j DNAT --to-destination X.X.X.X  

## NAT - INSIDE - PORT FORWADING - Individual Ports
iptables -t nat -A PREROUTING -i interface -p tcp/udp --dport X -j DNAT --to-destination X.X.X.X

## NAT - INSIDE - PORT FORWADING - Forward all traffic
iptables -t nat -A PREROUTING -i interface -j DNAT --to-destination X.X.X.X


**********
netfilter-persistent flush - clear memory and put all tables [filter, net, raw, mangle, security] em ACCEPT
netfilter-persistent reload - Copy of /etc/iptables/rules.v4 for memory 
netfilter-persistent save  - Use this command for saving in /etc/iptables/rules.v4 the FW rules who are in memory
**********
