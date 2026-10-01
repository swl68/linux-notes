# libvirt 
Для прохождения multicast трафика в гостевую систему при использовании macvtap с mode='passthrough' требуется установить ключ trustGuestRxFilters='yes' иначе работа протоколов lldp, lacp, ospf, ldp, ipv6 будет нарушена.
```
<interface type='direct' trustGuestRxFilters='yes'>
  <source dev='bond1' mode='passthrough'/>
  <model type='virtio'/>
</interface>
```

### https://libvirt.org/formatdomain.html#virtual-network
