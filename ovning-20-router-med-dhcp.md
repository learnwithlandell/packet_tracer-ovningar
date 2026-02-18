# DHCP i ett lokalt LAN – Konfiguration och verifiering

---

# Syfte

I denna övning ska du:

* Analysera en befintlig routerkonfiguration
* Verifiera att DHCP är korrekt konfigurerat
* Konfigurera en klient att erhålla IP via DHCP
* Validera att adressutdelningen fungerar
* Bekräfta nätverkskommunikation i LAN

Notera: Routern fungerar som DHCP-server.

---

# Del 1 - Skapa följande topologi

* 1 Router (1941)
* 1 Switch (2960)
* 1 PC


![Topologi](./img/topologi-router-med-dhcp.png)

---

# Del 2 – Analysera routerns konfiguration

Öppna CLI på routern och kör:

```
show running-config
```

Verifiera att följande finns:

* Interface G0/1 med IP-adress 192.168.10.1/24
* Ett DHCP pool-konfigurationsblock
* Exkluderade adresser

I den aktuella konfigurationen finns:

* DHCP pool: `MinPool`
* Nät: 192.168.10.0 /24
* Default-router: 192.168.10.1
* DNS-server: 8.8.8.8
* Excluded range: 192.168.10.1 – 192.168.10.20

Se routerkonfigurationen: 

---

# Del 3 – Kontrollera att korrekt interface är aktivt

Verifiera att G0/1 är up:

```
show ip interface brief
```

Interface G0/1 ska:

* Vara up/up
* Ha IP 192.168.10.1

Observera att G0/0 är shutdown och saknar IP. Detta är korrekt enligt konfigurationen.

---

# Del 4 – Konfigurera PC att använda DHCP

På PC:

Config → FastEthernet0
Välj:

* IPv4: DHCP

Vänta några sekunder.

Alternativt via Desktop → IP Configuration → DHCP.

---

# Del 5 – Verifiera DHCP-tilldelning

På PC, kontrollera:

* IP-adress
* Subnet mask
* Default gateway
* DNS-server

IP-adressen ska:

* Tillhöra 192.168.10.0/24
* Inte ligga mellan 192.168.10.1–20
* Gateway ska vara 192.168.10.1
* DNS ska vara 8.8.8.8

---

# Del 6 – Verifiera DHCP från routern

På routern:

```
show ip dhcp binding
```

Du ska se:

* En dynamiskt tilldelad IP
* Associerad MAC-adress

Kontrollera även:

```
show ip dhcp pool
```

Verifiera:

* Antal leased addresses
* Available addresses

---

# Del 7 – Testa kommunikation

Från PC:

```
ping 192.168.10.1
```

Ping ska lyckas.

Valfritt:

```
arp -a
```

Verifiera att gatewayns MAC-adress har lärts in.

---

# Verifiering – Hur du vet att uppgiften är korrekt utförd

Du har lyckats när:

1. PC har fått korrekt IP-adress inom tillåtet intervall.
2. `show ip dhcp binding` visar en aktiv lease.
3. Ping till 192.168.10.1 fungerar.
4. ARP-tabellen på PC innehåller gatewayns MAC.
5. DHCP-poolen visar minskat antal tillgängliga adresser.

Tips: Switchkonfigurationen kräver ingen ändring i denna övning och har inga särskilda VLAN-konfigurationer. Se switchens running-config: 

---

# Bonus – Analysfrågor

1. Varför är 192.168.10.1–20 exkluderade?
2. Vad händer om poolen tar slut?
3. Vad händer om default-router inte anges i poolen?
4. Kan DHCP fungera om G0/1 är shutdown?

---
