# pg_ha
Ziel: Ein Leader, genau ein Sync Standby, mindestens ein weiterer Replica. Alle State = running/streaming, TL identisch.
``` sh
# Alle drei Knoten abfragen
for i in j{1..3}; do
  echo "=== $i ==="
  ssh "$i" sudo patronictl -c /etc/patroni/patroni.yml list
done
```
Erwartung (Beispiel):
	• genau ein Role: Leader
	• ein Role: Sync Standby (oder sync_standby)
	• restliche Knoten Replica
	• State beim Leader running, bei Replikas streaming
	• gleiche TL (Timeline) auf allen

``` sh
# Host auswählen (oder per Schleife alle drei prüfen)                                                                        main 86649df4 +137 -9  4  6
H=j1

# Lauscher lokal auf dem Host
ssh "$H" 'sudo ss -lntp | egrep ":(5000|5001|8404)\s" || echo "no listeners"'

# Von deinem Dev-Host aus erreichbar?
nc -zv "$H" 5000
nc -zv "$H" 5001
nc -zv "$H" 8404
```
