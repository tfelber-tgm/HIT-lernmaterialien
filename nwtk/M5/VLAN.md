# VLAN (Virtual Local Area Network)

## Was ist ein VLAN?

> Ein Virtual Local Area Network (VLAN) ist **ein kleineres logisches Segment** innerhalb eines großen physikalischen, kabelgebundenen Netzwerks. [1]

Jedes VLAN hat eine eigene Broadcast-Domain, d.h. Kommunikation ist nur innerhalb eines VLANs möglich. Sie partitionieren das Netzwerk auf Layer 2. Inter VLAN Kommunikation ist nur mit einem Router möglich. In einem physikalischen Netzwerk können mehrere logische Netzwerke erzeugt werden. 


## Vorteile von VLAN

* Sicherheit
* Kostenreduktion
* Performance
* Broadcastdomäne einschränken
  * Broadcast Frames werden nur innerhalb des VLANs weitergeleitet
  * Zur Kontrolle der Reichweite von Broadcasts und ihr Einfluss am Netz
* Simples Projekt und Applikationsmanagement

[3]

## Portbasiertes (untagged) VLAN

Hier werden VLANs den Ports eines Switches zugewiesen. Das bedeutet das alle Netze im selben VLAN müssen miteinander verbunden sein. [2]

## Tagged VLAN

Hier enthalten die Frames zusätzlich noch einen Tag, der besagt in welchem VLAN sie gesendet werden. Das bedeutet, dass nicht alle Teilnehmer eines VLANs direkt mit einander Verbunden sein müssen. Der Switch entscheidet wie er Datei verschicken muss, um zu den richtigen Teilnehmer zu kommen. Das wird auch als ***Trunking*** bezeichnet.[2]

> * Ein VLAN Trunk trägt mehr als ein VLAN
> * Ein VLAN Trunk wird meist zwischen Switches erstellt, sodass VLAN Geräte miteinander Kommunizieren können, auch ohne physischen Verbindung

## VLANs erstellen und verwalten

Mittels `show vlan brief` wird eine Übersicht der VLANs ausgegeben.

Um ein VLAN zu erstellen muss man in den Konfigurationsmodus mittels `configure terminal` gehen. Mittels `vlan vlan_id` erstellt man das VLAN mit einer vlan_id, die noch nicht verwendet wird. Einen Namen kann man dem VLAN durch `name vlan_name` zuweisen. [3]

Bsp:

```
S1# configure terminal
S1(config)# vlan 20
S1(config)# name IT-Abteilung
S1(config)# end
```

Um Ports einem VLAN zuzuweisen geht man wieder in den Konfigurationmodus `configure terminal`. Danach wird der Port mit `interface (interface_id)` ausgewählt, der hinzugefügt werden soll und setzt den Port in Access Mode `switchport mode access`. Mittels `switchport access vlan (vlan_id)` wird das VLAN ausgewählt. [3]

Bsp:

```
S1# configure terminal
S1(config)# interface F0/18
S1(config-if)# switchport mode access
S1(config-if)# switchport mode access vlan 20
S1(config-if)# end
```



Quellen:

[1] https://www.ionos.de/digitalguide/server/knowhow/vlan-grundlagen/

[2] https://networkdirection.net/articles/network-theory/taggeduntaggedandnativevlans/

[3] Cisco Netacad "Routing & Switching"
