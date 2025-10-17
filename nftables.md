## NFTABLES NOTES
Список правил с номером для изменения:\\
```sudo nft -a list ruleset```

Добавление правила сверху:\\
```sudo nft insert rule filter INPUT ip saddr 5.165.112.213 tcp dport 22 counter accept```

Добавление правила снизу:\\
```sudo nft add rule filter INPUT ip saddr 5.165.112.213 tcp dport 22 counter accept```

Добавить правило после правила под номером 8:\\
```nft add rule filter INPUT position 8 ip daddr 5.165.112.211 drop```

Удаление правила под номером 10:\\
```sudo nft delete rule filter INPUT handle 10```

Замена существующего правила на новое:\\
```sudo nft replace rule ip cgnat postrouting handle НОМЕР "НОВОЕ ПРАВИЛО"```

## NFTABLES Пример
```
sudo tee /etc/nftables.conf << EOF

table ip FILTER {
  chain INPUT {
    type filter hook input priority filter; policy accept;
    iifname "lo" counter accept
    ip saddr 10.50.100.0/24 tcp dport { 22, 80, 5000, 5001 } counter accept
    ip saddr 192.168.194.0/24 tcp dport { 22 } counter accept
    icmp type echo-request limit rate 5/second accept
    ct state established,related counter accept
    counter drop
  }
}
EOF
```
Проверить синтаксис файла с правилами.
```sudo nft -c -f /etc/nftables.conf```

Перезагрузить службу для применения изменений:
```sudo systemctl enable nftables.service```
