## Способ решения проблемы с отображением lldp соседств на устройстве Mikrotik

Создаем лист, в который помещаем интерфейсы, которые требуется отображать в lldp:

```
/interface list member
add interface=eth1 list=lldp
add interface=wlan60-1 list=lld
```

Применяем, созданный лист в настройках обнаружения и отключаем другие протоколы:

```
/ip neighbor discovery-settings
set discover-interface-list=lldp protocol=lldp
```

Запрещаем пересылку кадра lldp от устройства к устройству:

```
/interface bridge filter
add action=drop chain=forward dst-mac-address=01:80:C2:00:00:0E/FF:FF:FF:FF:FF:FF
```
