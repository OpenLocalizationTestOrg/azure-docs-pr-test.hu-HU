egy címke tooa erőforráscsoportot tooadd használja **azure védelmicsoport-készlet**. Ha hello erőforráscsoport nem rendelkezik meglévő címkék, adjon át hello címke.

```azurecli
azure group set -n tag-demo-group -t Dept=Finance
```

Címkék egész frissülnek. Ha azt szeretné, hogy a címke tooa erőforráscsoport, amelynek létező címkék tooadd, adja át az összes hello címkét. 

```azurecli
azure group set -n tag-demo-group -t Dept=Finance;Environment=Production;Project=Upgrade
```

Címkék nem örökli erőforrás egy erőforráscsoportban. egy címke tooa erőforrás tooadd használja **azure erőforráskészlethez**. Hello hello erőforrástípus hello címkét felvenni kívánt API verziószámát adja át. Tooretrieve hello API verziójára van szükség, ha használja a következő parancs az erőforrás-szolgáltató hello hello típus állítja hello:

```azurecli
azure provider show -n Microsoft.Storage --json
```

Keresse meg szeretné hello erőforrástípus hello eredmény elérése érdekében.

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

Címkék közvetlenül erőforrások és csoportok léteznek. toosee hello meglévő címkéket, az erőforráscsoportot és az erőforrások lekérése **azure-csoportok megjelenítése**.

```azurecli
azure group show -n tag-demo-group --json
```

Amely hello erőforráscsoport, beleértve bármilyen alkalmazott címkék tooit kapcsolatos metaadatokat adja vissza.

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

Megtekintheti egy adott erőforráshoz hello címkék használatával **azure-erőforrás megjelenítése**.

```azurecli
azure resource show -g tag-demo-group -n storagetagdemo -r Microsoft.Storage/storageAccounts -o 2016-01-01 --json
```

tooretrieve címke értékű, minden hello erőforrás használata:

```azurecli
azure resource list -t Dept=Finance --json
```

tooretrieve címke értékű, minden hello erőforráscsoportok használja:

```azurecli
azure group list -t Dept=Finance
```

A parancs a következő hello hello meglévő címkék az előfizetésében tekintheti meg:

```azurecli
azure tag list
```
