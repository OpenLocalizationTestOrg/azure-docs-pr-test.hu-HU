Egy címkét adhat hozzá egy erőforráscsoportba **azure védelmicsoport-készlet**. Ha az erőforráscsoport nem rendelkezik meglévő címkék, továbbítja a kódban.

```azurecli
azure group set -n tag-demo-group -t Dept=Finance
```

Címkék egész frissülnek. Ha szeretne egy címke hozzáadása egy meglévő címkék rendelkező erőforrás csoporthoz, adja át a címkék. 

```azurecli
azure group set -n tag-demo-group -t Dept=Finance;Environment=Production;Project=Upgrade
```

Címkék nem örökli erőforrás egy erőforráscsoportban. Egy erőforrást egy címkét adhat hozzá, **azure erőforráskészlethez**. Az erőforrástípus hozzáadni a címkéhez való API verziószáma át. Ha beolvasása az API-verzió van szüksége, a következő paranccsal az erőforrás-szolgáltató állítja a típushoz:

```azurecli
azure provider show -n Microsoft.Storage --json
```

Az eredményeket keresse meg a kívánt erőforrás-típus.

```azurecli
"resourceTypes": [
{
  "resourceType": "storageAccounts",
  ...
  "apiVersions": [
    "2016-01-01",
    "2015-06-15",
    "2015-05-01-preview"
  ]
}
...
```

Most, adja meg az adott API-verzió, az erőforráscsoport neve, erőforrás nevét, erőforrástípus és címke paraméterekként.

```azurecli
azure resource set -g tag-demo-group -n storagetagdemo -r Microsoft.Storage/storageAccounts -t Dept=Finance -o 2016-01-01
```

Címkék közvetlenül erőforrások és csoportok léteznek. A meglévő címkék megtekintéséhez beolvasása erőforráscsoport és az erőforrások **azure-csoportok megjelenítése**.

```azurecli
azure group show -n tag-demo-group --json
```

Ami az erőforráscsoportot, többek között az alkalmazott címkéket kapcsolatos metaadatokat adja vissza.

```azurecli
{
  "id": "/subscriptions/4705409c-9372-42f0-914c-64a504530837/resourceGroups/tag-demo-group",
  "name": "tag-demo-group",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "location": "southcentralus",
  "tags": {
    "Dept": "Finance",
    "Environment": "Production",
    "Project": "Upgrade"
  },
  ...
}
```

Megtekintheti egy adott erőforráshoz címkék használatával **azure-erőforrás megjelenítése**.

```azurecli
azure resource show -g tag-demo-group -n storagetagdemo -r Microsoft.Storage/storageAccounts -o 2016-01-01 --json
```

Egy címke az összes erőforrást lekéréséhez használja:

```azurecli
azure resource list -t Dept=Finance --json
```

A címkeérték erőforráscsoportok lekéréséhez használja:

```azurecli
azure group list -t Dept=Finance
```

A meglévő címkéket az előfizetés a következő paranccsal tekintheti meg:

```azurecli
azure tag list
```
