# Hostile networks

### Classifying Attackers

- 25% are known hostile networks (nothing good comes from them)
- 25% are bots
- 25% come from known cloud providers
- 25% are "researchers"

Source: https://www.hackerfactor.com/blog/index.php?/archives/775-Scans-and-Attacks.html

Bot Directory by Distil Networks: https://www.distilnetworks.com/bot-directory/

See also Access Watch database: https://access.watch/database

BotoPedia by Incapsula http://www.botopedia.org/

### Usage

Run `myattackers-ipsets-install.sh`

Update ipset files with embedded update script: `sed -n -e 's/^#\$ //p' example.ipset | bash`

### Usage on systems without ipset

```bash
grep -h '^add' *.ipset | cut -d " " -f 3 | sortip \
    | xargs -L 1 echo iptables -I myattackers -j REJECT -s
```

### Usage in htaccess files

```bash
echo "<RequireAll>"
echo "Require all granted"
grep -h '^add' *.ipset | cut -d " " -f 3 | sortip \
    | xargs -L 1 echo Require not ip
echo "</RequireAll>"
```

### Usage on Mikrotik routers

```bash
grep -h '^add' *.ipset | cut -d " " -f 3 | sortip \
    | xargs -I % echo "/ip firewall address-list add list=myattackers-ipset address=%" \
    > mikrotik-myattackers-ipset.rsc
```

Usage on router: `/import file=mikrotik-myattackers-ipset.rsc`
