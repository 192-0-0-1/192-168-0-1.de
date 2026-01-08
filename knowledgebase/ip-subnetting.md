# IP-Adressen & Subnetting

Ein umfassender Leitfaden zum Verständnis von IP-Adressen und Subnetting.

---

## Inhaltsverzeichnis

1. [Was ist eine IP-Adresse?](#was-ist-eine-ip-adresse)
2. [IPv4 vs. IPv6](#ipv4-vs-ipv6)
3. [Aufbau einer IPv4-Adresse](#aufbau-einer-ipv4-adresse)
4. [Netzklassen (Classful Addressing)](#netzklassen-classful-addressing)
5. [Private vs. Öffentliche IP-Adressen](#private-vs-öffentliche-ip-adressen)
6. [Subnetting Grundlagen](#subnetting-grundlagen)
7. [Subnetzmaske](#subnetzmaske)
8. [CIDR-Notation](#cidr-notation)
9. [Praktische Beispiele](#praktische-beispiele)
10. [Übungsaufgaben](#übungsaufgaben)

---

## Was ist eine IP-Adresse?

Eine IP-Adresse (Internet Protocol Address) ist eine eindeutige numerische Kennung, die jedem Gerät in einem Netzwerk zugewiesen wird. Sie ermöglicht die Kommunikation zwischen Geräten im Internet oder in lokalen Netzwerken.

**Funktion:**
- Identifizierung von Geräten im Netzwerk
- Routing von Datenpaketen zum richtigen Ziel
- Adressierung auf der Netzwerkschicht (Layer 3 im OSI-Modell)

---

## IPv4 vs. IPv6

### IPv4 (Internet Protocol Version 4)
- **Format:** 32-Bit-Adresse
- **Darstellung:** Vier Zahlen von 0-255, getrennt durch Punkte
- **Beispiel:** `192.168.0.1`
- **Adressraum:** ~4,3 Milliarden Adressen
- **Problem:** Adressknappheit durch weltweite Internetnutzung

### IPv6 (Internet Protocol Version 6)
- **Format:** 128-Bit-Adresse
- **Darstellung:** Acht Gruppen von vier Hexadezimalzahlen, getrennt durch Doppelpunkte
- **Beispiel:** `2001:0db8:85a3:0000:0000:8a2e:0370:7334`
- **Adressraum:** ~340 Sextillionen Adressen
- **Vorteil:** Praktisch unbegrenzte Adressen

**In diesem Guide konzentrieren wir uns auf IPv4, da es noch am weitesten verbreitet ist.**

---

## Aufbau einer IPv4-Adresse

Eine IPv4-Adresse besteht aus 32 Bits, die in vier 8-Bit-Segmente (Oktette) unterteilt sind.

### Dezimal-Darstellung
```
192.168.0.1
```

### Binär-Darstellung
```
11000000.10101000.00000000.00000001
```

### Aufschlüsselung
- Jedes Oktett kann Werte von 0 bis 255 annehmen
- Die Adresse besteht aus zwei Teilen:
  - **Netzwerk-Anteil:** Identifiziert das Netzwerk
  - **Host-Anteil:** Identifiziert das Gerät im Netzwerk

---

## Netzklassen (Classful Addressing)

Früher wurden IP-Adressen in Klassen eingeteilt. Diese Einteilung ist heute durch CIDR ersetzt, ist aber zum Verständnis wichtig.

### Klasse A
- **Bereich:** `1.0.0.0` bis `126.255.255.255`
- **Erstes Bit:** 0
- **Standard-Subnetzmaske:** `255.0.0.0` oder `/8`
- **Netzwerke:** 126 Netzwerke
- **Hosts pro Netzwerk:** ~16,7 Millionen
- **Verwendung:** Große Organisationen

### Klasse B
- **Bereich:** `128.0.0.0` bis `191.255.255.255`
- **Erste zwei Bits:** 10
- **Standard-Subnetzmaske:** `255.255.0.0` oder `/16`
- **Netzwerke:** ~16.000 Netzwerke
- **Hosts pro Netzwerk:** ~65.000
- **Verwendung:** Mittlere Organisationen

### Klasse C
- **Bereich:** `192.0.0.0` bis `223.255.255.255`
- **Erste drei Bits:** 110
- **Standard-Subnetzmaske:** `255.255.255.0` oder `/24`
- **Netzwerke:** ~2 Millionen Netzwerke
- **Hosts pro Netzwerk:** 254
- **Verwendung:** Kleine Netzwerke

### Klasse D (Multicast)
- **Bereich:** `224.0.0.0` bis `239.255.255.255`
- **Verwendung:** Multicast-Gruppen

### Klasse E (Reserviert)
- **Bereich:** `240.0.0.0` bis `255.255.255.255`
- **Verwendung:** Experimentell, nicht für normale Nutzung

---

## Private vs. Öffentliche IP-Adressen

### Öffentliche IP-Adressen
- Im Internet eindeutig und routbar
- Werden von Internet Service Providern (ISPs) zugewiesen
- Werden von der IANA (Internet Assigned Numbers Authority) verwaltet

### Private IP-Adressen
Private IP-Adressen sind für die Nutzung in lokalen Netzwerken reserviert und werden nicht im Internet geroutet.

**Reservierte Bereiche:**
- **Klasse A:** `10.0.0.0` bis `10.255.255.255` (`10.0.0.0/8`)
- **Klasse B:** `172.16.0.0` bis `172.31.255.255` (`172.16.0.0/12`)
- **Klasse C:** `192.168.0.0` bis `192.168.255.255` (`192.168.0.0/16`)

**Sonderadressen:**
- **Loopback:** `127.0.0.0/8` (meist `127.0.0.1` - "localhost")
- **Link-Local:** `169.254.0.0/16` (automatische Adresszuweisung wenn DHCP fehlschlägt)

---

## Subnetting Grundlagen

Subnetting ist der Prozess, ein großes Netzwerk in kleinere Teilnetzwerke (Subnetze) aufzuteilen.

### Warum Subnetting?
- **Effizienz:** Bessere Nutzung des verfügbaren Adressraums
- **Sicherheit:** Trennung von Netzwerksegmenten
- **Performance:** Reduzierung von Broadcast-Domains
- **Organisation:** Logische Strukturierung des Netzwerks

### Wichtige Begriffe
- **Netzwerkadresse:** Erste Adresse im Subnetz (alle Host-Bits auf 0)
- **Broadcast-Adresse:** Letzte Adresse im Subnetz (alle Host-Bits auf 1)
- **Nutzbare Hosts:** Alle Adressen zwischen Netzwerk- und Broadcast-Adresse
- **Anzahl nutzbarer Hosts:** 2^n - 2 (wobei n = Anzahl der Host-Bits)

---

## Subnetzmaske

Die Subnetzmaske bestimmt, welcher Teil der IP-Adresse das Netzwerk und welcher Teil den Host identifiziert.

### Funktionsweise
- Binär gesehen: Zusammenhängende 1en für Netzwerk-Teil, 0en für Host-Teil
- Wird mit der IP-Adresse per bitweisem AND verknüpft, um die Netzwerkadresse zu ermitteln

### Beispiel
```
IP-Adresse:      192.168.1.100
Subnetzmaske:    255.255.255.0

Binär:
IP:              11000000.10101000.00000001.01100100
Maske:           11111111.11111111.11111111.00000000
                 ─────────────────────────── ────────
Netzwerkadresse: 11000000.10101000.00000001.00000000
                 = 192.168.1.0
```

### Standard-Subnetzmasken
| CIDR | Subnetzmaske    | Anzahl Hosts |
|------|-----------------|--------------|
| /8   | 255.0.0.0       | 16,777,214   |
| /16  | 255.255.0.0     | 65,534       |
| /24  | 255.255.255.0   | 254          |
| /25  | 255.255.255.128 | 126          |
| /26  | 255.255.255.192 | 62           |
| /27  | 255.255.255.224 | 30           |
| /28  | 255.255.255.240 | 14           |
| /29  | 255.255.255.248 | 6            |
| /30  | 255.255.255.252 | 2            |

---

## CIDR-Notation

CIDR (Classless Inter-Domain Routing) ist die moderne Methode zur IP-Adressierung.

### Format
```
IP-Adresse/Präfixlänge
Beispiel: 192.168.1.0/24
```

Die Zahl nach dem Schrägstrich gibt an, wie viele Bits für das Netzwerk verwendet werden.

### Umrechnung
- `/24` bedeutet: Die ersten 24 Bits sind für das Netzwerk, die letzten 8 für Hosts
- Entspricht der Subnetzmaske `255.255.255.0`

### Berechnung der Anzahl der Hosts
```
Formel: 2^(32 - Präfixlänge) - 2

Beispiel /24:
2^(32-24) - 2 = 2^8 - 2 = 256 - 2 = 254 nutzbare Hosts
```

---

## Praktische Beispiele

### Beispiel 1: Einfaches Heimnetzwerk

**Gegeben:**
- IP-Adresse: `192.168.0.1/24`

**Analyse:**
```
Netzwerkadresse:    192.168.0.0
Erste nutzbare IP:  192.168.0.1
Letzte nutzbare IP: 192.168.0.254
Broadcast-Adresse:  192.168.0.255
Anzahl Hosts:       254
Subnetzmaske:       255.255.255.0
```

### Beispiel 2: Subnetting eines Netzwerks

**Aufgabe:** Teile das Netzwerk `192.168.1.0/24` in 4 gleich große Subnetze.

**Lösung:**
- Wir brauchen 2 zusätzliche Bits für 4 Subnetze (2^2 = 4)
- Neue Präfixlänge: /24 + 2 = /26
- Jedes Subnetz hat 2^6 - 2 = 62 nutzbare Hosts

**Subnetze:**
1. `192.168.1.0/26` - Hosts: 192.168.1.1 bis 192.168.1.62
2. `192.168.1.64/26` - Hosts: 192.168.1.65 bis 192.168.1.126
3. `192.168.1.128/26` - Hosts: 192.168.1.129 bis 192.168.1.190
4. `192.168.1.192/26` - Hosts: 192.168.1.193 bis 192.168.1.254

### Beispiel 3: VLSM (Variable Length Subnet Mask)

**Aufgabe:** Du hast das Netzwerk `10.0.0.0/8` und benötigst:
- 1 Subnetz mit 1000 Hosts (Büro)
- 1 Subnetz mit 500 Hosts (Produktion)
- 1 Subnetz mit 50 Hosts (Verwaltung)
- 2 Punkt-zu-Punkt-Verbindungen (je 2 Hosts)

**Lösung:**
1. **Büro (1000 Hosts):** Benötigt mindestens 1024 Adressen → /22 (2^10 = 1024)
   - Subnetz: `10.0.0.0/22`
   
2. **Produktion (500 Hosts):** Benötigt mindestens 512 Adressen → /23 (2^9 = 512)
   - Subnetz: `10.0.4.0/23`
   
3. **Verwaltung (50 Hosts):** Benötigt mindestens 64 Adressen → /26 (2^6 = 64)
   - Subnetz: `10.0.6.0/26`
   
4. **P2P-Links:** Benötigen je 2 Adressen → /30 (2^2 = 4)
   - Subnetz 1: `10.0.6.64/30`
   - Subnetz 2: `10.0.6.68/30`

---

## Übungsaufgaben

### Aufgabe 1: Grundlagen
Gegeben ist die IP-Adresse `172.16.45.123/20`.

**Fragen:**
1. Was ist die Netzwerkadresse?
2. Was ist die Broadcast-Adresse?
3. Wie viele nutzbare Hosts gibt es?
4. Was ist die Subnetzmaske in Dezimalschreibweise?

### Aufgabe 2: Subnetting
Teile das Netzwerk `192.168.100.0/24` in 8 gleich große Subnetze.

**Fragen:**
1. Welche Präfixlänge haben die neuen Subnetze?
2. Liste alle Netzwerkadressen auf.
3. Wie viele Hosts passen in jedes Subnetz?

### Aufgabe 3: VLSM
Du hast das Netzwerk `172.20.0.0/16` und benötigst:
- Subnetz A: 8000 Hosts
- Subnetz B: 4000 Hosts
- Subnetz C: 2000 Hosts
- Subnetz D: 200 Hosts

**Aufgabe:** Plane die Subnetze effizient (möglichst wenig Verschwendung).

### Aufgabe 4: Analyse
Bestimme, ob folgende IP-Adressen im gleichen Subnetz liegen:
1. `10.5.67.89/22` und `10.5.65.200/22`
2. `192.168.1.50/26` und `192.168.1.70/26`
3. `172.16.128.45/18` und `172.16.192.100/18`

---

## Lösungen

### Lösung Aufgabe 1
1. Netzwerkadresse: `172.16.32.0`
2. Broadcast-Adresse: `172.16.47.255`
3. Nutzbare Hosts: 4094 (2^12 - 2)
4. Subnetzmaske: `255.255.240.0`

### Lösung Aufgabe 2
1. Präfixlänge: /27 (24 + 3 = 27, da 2^3 = 8)
2. Netzwerkadressen:
   - 192.168.100.0/27
   - 192.168.100.32/27
   - 192.168.100.64/27
   - 192.168.100.96/27
   - 192.168.100.128/27
   - 192.168.100.160/27
   - 192.168.100.192/27
   - 192.168.100.224/27
3. Hosts pro Subnetz: 30 (2^5 - 2)

### Lösung Aufgabe 3
- Subnetz A: `172.20.0.0/19` (8190 Hosts)
- Subnetz B: `172.20.32.0/20` (4094 Hosts)
- Subnetz C: `172.20.48.0/21` (2046 Hosts)
- Subnetz D: `172.20.56.0/24` (254 Hosts)

### Lösung Aufgabe 4
1. **Ja** - Beide liegen in `10.5.64.0/22`
2. **Nein** - 10.5.67.89 liegt in `10.5.64.0/26`, 10.5.65.200 liegt in `10.5.64.64/26`
3. **Nein** - 172.16.128.45 liegt in `172.16.128.0/18`, 172.16.192.100 liegt in `172.16.192.0/18`

---

## Weiterführende Ressourcen

- [RFC 791 - Internet Protocol](https://www.rfc-editor.org/rfc/rfc791)
- [RFC 1918 - Private Address Space](https://www.rfc-editor.org/rfc/rfc1918)
- [RFC 4632 - CIDR](https://www.rfc-editor.org/rfc/rfc4632)

---

**Letzte Aktualisierung:** Januar 2026  
**Lizenz:** Apache v2.0 [LICENSE](https://github.com/192-0-0-1/192-168-0-1.de/blob/main/LICENSE)
**Beiträge willkommen!**