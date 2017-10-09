Ellenőrizheti, hogy sikerült-e a kapcsolat hello segítségével [az hálózati vpn-kapcsolat megjelenítése](/cli/azure/network/vpn-connection#show) parancsot. Hello példában "--name", amelyet az tootest hello kapcsolat toohello nevére hivatkozik. Hello kapcsolat esetén hello során jött létre a kapcsolat állapota "Csatlakozás" jeleníti meg. Hello kapcsolat létrejötte után hello állapota too'Connected ".

```azurecli
az network vpn-connection show --name VNet1toSite2 --resource-group TestRG1
```

