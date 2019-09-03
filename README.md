# esx_kamuhizmeti

Oyunculara ceza oalrak çöp toplama tarzı kamu hizmeti cezaları vermeyi sağlar


### Gerekenler
* ESX
* ESX skinchanger
  * [skinchanger](https://github.com/ESX-Org/skinchanger)


### Using Git
```
cd resources
git clone https://github.com/apoiat/esx_communityservice [esx]/esx_communityservice
```

### Manually
- Download https://github.com/apoiat/esx_communityservice/archive/master.zip
- Put it in the `[esx]` directory


## kurulum
- Veritabanınıza `esx_kamuhizmeti.sql` dosyasını aktarın
- Sunucunuzun server.cfg dosyasına `esx_kamuhizmeti` yazın

## How to apply community service.

- Use the `esx_communityservice:sendToCommunityService(target, service_count)` server trigger.
- Use the `/comserv player_id service_count` command (only admins).
- Use the `/endcomserv player_id` to finish a player's community service (only admins).



# policejob menüsüne ekleme yolu

 `esx_policejob: client/main.lua`:

```lua
-- aşağıdaki satırları ctrl+f ile bulup {label = "Community Service",	value = 'communityservice'} satırını ekleyin
{label = _U('fine'),			value = 'fine'},
{label = _U('unpaid_bills'),	value = 'unpaid_bills'},
-- son satırdan önce "," koymayı unutmayın
{label = "Community Service",	value = 'communityservice'}


-- aşağıdaki kodu bulun
elseif action == 'unpaid_bills' then
	OpenUnpaidBillsMenu(closestPlayer)
-- aşağıdaki kodu ekleyin
elseif action == 'communityservice' then
	SendToCommunityService(GetPlayerServerId(closestPlayer))
end


-- ADDITION [3]
-- dosyanın en sonuna bu function'ı ekelyin
function SendToCommunityService(player)
	ESX.UI.Menu.Open('dialog', GetCurrentResourceName(), 'Community Service Menu', {
		title = "Community Service Menu",
	}, function (data2, menu)
		local community_services_count = tonumber(data2.value)
		
		if community_services_count == nil then
			ESX.ShowNotification('Invalid services count.')
		else
			TriggerServerEvent("esx_communityservice:sendToCommunityService", player, community_services_count)
			menu.close()
		end
	end, function (data2, menu)
		menu.close()
	end)
end