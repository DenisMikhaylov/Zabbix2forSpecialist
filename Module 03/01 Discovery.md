

Настроить автоматичекое добавление
```
Discovery
New Discovery rules
  Name: Local network new
  IP range: 192.168.1.1-254
  Checks: 
    Check type: SNMPv2 agent 
    SNMP community: public
    SNMP OID .1.3.6.1.2.1.1.5.0
    Add
  Update interval: 3m



Alert->Actions-> Discovery action
  Create action
    Name: Action add snmp device to zabbix
    Conditions: 
      Discovery status: equals Up                 
      Add
    Operations:
      Add host
      Add to host groups: Discovered hosts            
      Link to templates:
              1. Windows by SNMP
      Set host inventory mode: Automatic
    Add
```

