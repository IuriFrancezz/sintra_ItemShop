# sintra_itemShop #
## FiveM - Simple Shop for gangs / Police / Ambulance to buy Items ##


# Language: English [ENG] #

## CONFIG ##
> 1º Step: Go to your Job and open config.lua 

> 2º Step: Insert into your code, above the authorizedWeapons 

> Yes, you can add more, you only need copy the line 11 and paste bellow adding an "," above of the code (at the end)

```
Config.AuthorizedMoldes = {
	{ name = 'item_id',     price = 2500, label = 'What you want to see on menu' }
}
```



## CLIENT SIDE ##
> 3º Step: Go to your Job and open client/main.lua 

> 4º Step: Insert into your code, bellow the skin changer code that:

```
function OpenBuyItemMenu()


    local elements = {}

    for i=1, #Config.AuthorizedItems, 1 do

      local Item = Config.AuthorizedItems[i]

      table.insert(elements, {label = Item.label .. ' - €' .. Item.price, value = Item.name, price = Item.price})

    end

    ESX.UI.Menu.Open(
      'default', GetCurrentResourceName(), 'armory_buy_Items',
      {
        title    = "Loja de Items",
        align    = 'top-right',
        elements = elements,
      },
      function(data, menu)
			  local mname = data.current.value
			  local mprice = data.current.price

              TriggerServerEvent('esx_tequila:Item', mname, mprice)

      end,
      function(data, menu)
        menu.close()
      end
    )
end
```

> 5º Step: Search by "OpenArmoryMenu" [ShowCase](https://prnt.sc/vpfeah)

> 6º Step: Insert above the "end" function that:

```
table.insert(elements, {label = 'Comprar Equipamentos', value = 'buy_Item'})
```

> 7º Step: Scrooling Down you can see 1 IF functions and somes elseif

> 8º Step: Insert into your elseifs functions that: (you can add the function where you want, exceptionally above the IF)

```
elseif data.current.value == 'buy_Item' then
	OpenBuyItemMenu()`
```



## SERVER SIDE ##
> 9º Step: Go to your Job and open server/main.lua 
> 10º Step: Insert into your code, bellow the putStockItems (you can search by that) you paste it:
```
RegisterServerEvent('sintra_itemShop:Item')
AddEventHandler('sintra_itemShop:Item', function(mname, mprice)
	local xPlayer = ESX.GetPlayerFromId(source)


	if xPlayer.getMoney() >= mprice then
		xPlayer.removeMoney(mprice)
		xPlayer.addInventoryItem(mname, 1)
	else
		TriggerClientEvent('esx:showNotification', xPlayer.source, "Você não tem dinheiro suficiente")	
	end
end)
```





# Author #
- [Iuri Rodrigues](discord.gg/PgEe8Yg)
