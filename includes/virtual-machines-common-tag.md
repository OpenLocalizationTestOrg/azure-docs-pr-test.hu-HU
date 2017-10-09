


## <a name="tagging-a-virtual-machine-through-templates"></a>A sablonok használatával virtuálisgép-címkézés
Első lépésként nézzük címkézés sablonok keresztül. [Ez a sablon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-tags) címkék helyez el a következő erőforrások hello: számítási (virtuális gép), a tároló (Tárfiók) és a hálózaton (a nyilvános IP-cím, a virtuális hálózati és a hálózati adapter). Ez a sablon a Windows virtuális gépek azonban Linux virtuális gépek módosítani kell.

Kattintson a hello **tooAzure telepítése** hello a gomb [Sablonhivatkozás](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-tags). Ez jelenik meg a toohello [Azure-portálon](https://portal.azure.com/) sablon telepíthető.

![Egyszerű telepítés címkékkel](./media/virtual-machines-common-tag/deploy-to-azure-tags.png)

Ez a sablon tartalmazza a következő címkék hello: *részleg*, *alkalmazás*, és *létrehozta*. Akkor is felvehet és szerkeszthet ezekkel a címkékkel hello sablon közvetlenül a Ha azt szeretné, hogy különböző címkenevek.

![Egy Azure címkék](./media/virtual-machines-common-tag/azure-tags-in-a-template.png)

Ahogy látja, hello címkék is meg van adva kulcs/érték párok, egymástól kettősponttal (:). hello címkék a következő formátumban kell megadni:

        “tags”: {
            “Key1” : ”Value1”,
            “Key2” : “Value2”
        }

Hello sablonfájl mentési, az Ön által választott hello címkékkel a Szerkesztés befejezése után.

Ezután hello **paraméterek szerkesztése** szakaszban kitöltheti hello értékek a címkék.

![Címkék szerkesztése az Azure-portálon](./media/virtual-machines-common-tag/edit-tags-in-azure-portal.png)

Kattintson a **létrehozása** toodeploy Ez a sablon a címke értékekkel.

## <a name="tagging-through-hello-portal"></a>Címkézés hello portálon keresztül
Miután létrehozta az erőforrások címkékkel, megtekintheti, vegye fel, és hello portálon címkék törlése.

Jelölje be hello címkéket ikon tooview a címkéket:

![Azure-portálon címkék ikon](./media/virtual-machines-common-tag/azure-portal-tags-icon.png)

Hello portálon keresztül új címke hozzáadása a saját kulcs/érték pár meghatározásával, és mentse.

![Új címke hozzáadása az Azure-portálon](./media/virtual-machines-common-tag/azure-portal-add-new-tag.png)

Az új címke ekkor meg kell jelennie az erőforrás címkék hello listáját.

![Új címke az Azure portálon mentése](./media/virtual-machines-common-tag/azure-portal-saved-new-tag.png)

