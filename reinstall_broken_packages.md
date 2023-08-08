# Reinstall broken debian packages

```sh
debsums -a > debian_check 2> debian_ko
grep 'debsums: missing file' debian_ko | awk -F'from ' '{print $2}' | awk -F' package' '{print $1}' | uniq > paquets.txt
while IFS= read -r paquet; do
    apt install --reinstall "$paquet"
done < paquets.txt
```
