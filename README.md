# eastron_sdm_hc3
 Eastron Smartmeter SDM 230 und SDM630 mit Fibaro HC3
# QuickApp zur Abfrage der EASTRON SDM630 V3 und SDM230 V2 Modbus Smart Meter für HC3

Diese QuickApp dient der Abfrage der meisten Parameter der Eastron SDM630 / SDM 230 Modbus-Geräte. Sie liest direkt vom Modbus-Server, beispielsweise einem Modbus-Gerät wie dem Waveshare RS485 zu RJ45 Ethernet Industrial Serial Server Modbus Gateway.

## Wichtige Informationen:

- Nach der Installation wird nur das übergeordnete Gerät erstellt.
- Du kannst nun eigene Icons für übergeordnete und untergeordnete Geräte verwenden; diese müssen als Variable mit den Icon-IDs eingetragen werden. Wenn mehrere Geräte-IDs verwendet werden, musst du für jedes Gerät ein Icon bereitstellen, durch Komma getrennt. Sobald das Gerät aktiviert ist, werden diese Icons verwendet, wenn die ChildDevices erstellt werden.
  - Hinweis: Du kannst http://<hc3-ip-address>/api/icons verwenden, um eine Liste deiner Icons und Icon-IDs zu erhalten.
- Die Abfrage und Erstellung von ChildDevices bleibt inaktiv, bis die Variable SDM_ACTIVE auf true gesetzt wird. Dadurch kannst du die anderen Parameter, die ebenfalls auf dem Tab "Variables" gesetzt werden, nach Bedarf anpassen. Nachdem SDM_ACTIVE auf true gesetzt wurde, werden die ChildDevices automatisch erstellt.
- Weitere ChildDevices können hinzugefügt oder entfernt werden, indem der Parameter "enabled" im LUA-Code unter "ChildDevicesDetails" auf true oder false gesetzt wird.
- Wenn du ein V2-Gerät hast, lösche einfach die letzten beiden Zeilen in "ChildDevices Details", diese sind nur für V3.
  - Tipp: Wenn du Änderungen vornimmst und die QA-Datei speicherst, stelle sicher, dass du alle Variablen auf dem Tab "Variables" vorher löscht. Andernfalls werden die Variableneinstellungen deiner gespeicherten QA-Datei verwendet und der Eintrag "SDM_ACTIVE = false" wird wahrscheinlich nicht vorhanden sein, und die QA wird sofort versuchen, ChildDevices zu erstellen und Werte einzulesen.

## Variablen:

- `MODBUS_IP`: Enthält die IP-Adresse des Modbus-Adapters, z.B. von Waveshare
- `MODBUS_PORT`: Standardport des Adapters, 502
- `UNIT_ID`: ID des Eastron-Smartmeters
- `INTERVALL`: Definiert die Abfrageintervalle in Sekunden
- `ICON_PARENT`: Setzt die ID des zu verwendenden Icons für das übergeordnete Gerät
- `ICON_CHILDS`: Setzt die IDs der zu verwendenden Icons für die untergeordneten Geräte. Du kannst unterschiedliche Icons für verschiedene Geräte (IDs) verwenden; durch Komma getrennte Einträge
- `DEVICE_TYPE`: Setzt das Gerät, nur SDM230 oder SDM630 werden unterstützt
- `UNIT_IDS`: Komma-getrennte IDs der Smart Meter. Diese müssen den auf dem Smart Meter gesetzten IDs entsprechen
- `SDM_ACTIVE`: Stoppt alle Prozesse, wenn false, und setzt sie fort, wenn true

## Wichtig:

- Wenn ChildDevices mit "enabled=false" gelöscht werden, werden auch die bis zu diesem Zeitpunkt für dieses ChildDevice gespeicherten Daten gelöscht! Wenn dies nicht gewünscht ist, sollte das Gerät über die Schnittstelle verwaltet werden.
- Ich würde nicht empfehlen, alle Parameter auszulesen, da die QuickApp sonst in meinem Fall zu viel Last auf den HC3-Speicher gelegt hat.

## Haftungsausschluss:

Verwendung auf eigenes Risiko, ich habe das Gerät für meine eigenen Zwecke geschrieben und der Code kann ohne jegliche Garantie auf Vollständigkeit und Richtigkeit verwendet werden. Wenn du kein Risiko eingehen willst, lass es einfach. Der Code kann sich noch ändern.

Viel Spaß!

```lua
-- Beispiel für eine Konfiguration der Variablen in LUA
MODBUS_IP = "192.168.1.100"
MODBUS_PORT = 502
UNIT_ID = 1
INTERVALL = 30
ICON_PARENT = 1234
ICON_CHILDS = "1234,5678"
DEVICE_TYPE = "SDM630"
UNIT_IDS = "1,2,3"
SDM_ACTIVE = false
```

```lua
-- Beispiel für ChildDevicesDetails
ChildDevicesDetails = {
    { name = "Voltage L1", enabled = true, unit = "V", deviceType = "com.fibaro.binarySensor", id = 1 },
    { name = "Voltage L2", enabled = true, unit = "V", deviceType = "com.fibaro.binarySensor", id = 2 },
    { name = "Voltage L3", enabled = true, unit = "V", deviceType = "com.fibaro.binarySensor", id = 3 },
    -- Für V2-Geräte letzte zwei Zeilen löschen
    { name = "Power L1", enabled = true, unit = "W", deviceType = "com.fibaro.binarySensor", id = 4 },
    { name = "Power L2", enabled = true, unit = "W", deviceType = "com.fibaro.binarySensor", id = 5 }
}
```

Du kannst diesen Text als README.md auf GitHub einstellen, um die Installation und Konfiguration der QuickApp zu dokumentieren.