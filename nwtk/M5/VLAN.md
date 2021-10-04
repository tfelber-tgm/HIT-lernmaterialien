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

Hierzu öffnet man das CLI (Command Line Interface). Um in den privilegierten EXEC Modus zu gelangen gibt man `enable` oder einfach  `en` ein. Mittels `show vlan brief` wird eine Übersicht der VLANs ausgegeben.[3]

### Portbasierte VLANs

Um ein VLAN zu erstellen muss man in den Konfigurationsmodus mittels `configure terminal` gehen, ob man sich bereits in ihm befindet sieht man an der #. Mittels `vlan (vlan_id)` erstellt man das VLAN. Die vlan_id darf noch nicht verwendet sein. Einen Namen weist man dem VLAN durch `name vlan_name` zu. [3]

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
S1(config-if)# switchport access vlan 20
S1(config-if)# end
```

Durch `exit` springt man wieder eine Stufe höher, um z.B. noch ein VLAN zuzuordnen. `end` hingegen springt wieder ganz zurück zum Anfang.

### Trunks

Um Trunks zu erstellen, navigiert man wieder in den Konfigurationsmodus und wählt den Port aus auf dem der Trunk arbeiten soll. Um diesen Port als Trunk festzulegen gibt man `switchport mode trunk` ein. Danach wählt man das native VLAN aus `switchport trunk native vlan (vlan_id)`. Zusätzlich kann man auch die erlaubte VLANS auf dem Trunk festlegen `switchport trunk allowed vlan (vlan_id), (vlan_id), ...`. [3]

Bsp:

```
S1# configure terminal
S1(config)# interface G0/1
S1(config-if)# switchport mode trunk
S1(config-if)# switchport trunk native vlan 99
S1(config-if)# end
```

Um die Trunks zu vervollständigen müssen auch die Trunk Ports auf allen anderen Switches festgelegt werden. Man wird aber merken, dass es auch davor schon funktioniert, da das DTP (Dynamic Trunking Protocol) automatisch die nebenliegenden Ports konfiguriert. Es wird jedoch periodisch eine Fehlermeldung im CLI ausgegeben, dass die Ports nicht vollständig konfiguriert sind. 

Um die Konfiguration zu überprüfen gibt man `show interfaces (interface_id) switchport` ein. [3]

Quellen:

[1] https://www.ionos.de/digitalguide/server/knowhow/vlan-grundlagen/

[2] https://networkdirection.net/articles/network-theory/taggeduntaggedandnativevlans/

[3] Cisco Netacad "Routing & Switching"
