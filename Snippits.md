#These are just some Sensi code snipits to run in the Cesc console. 

## How to View Current keybinds:

```c#
public static void Run()
{
    foreach (GameOptions.GameOption current in GameOptions.Options)
    {
        if (current.OptionType == GameOptions.GameOptionType.KeybindData)
    	{
    		Log(current.ToString());
    	}
    }
}
```

Example result:
```
Index: 772, CommandName: kSSlt3, KeyCommandId: SkillSlot3 OptionType: KeybindData, Value: 
Primary: Key: D3, Modifier0: None, Modifier1: None, Modifier2: None
Secondary: Key: None, Modifier0: None, Modifier1: None, Modifier2: None
Joystick: Key: F19, Modifier0: None, Modifier1: None, Modifier2: None
```
Primary indicates the default keybinding.
Secondary indicates the secondary.
Joystick for joystick.

The Enum value you care about is on the first line. `KeyCommandId:`

#Distance to Target
```c#
public static void Run()
{
	Log(GameManager.LocalPlayer.CurrentTarget.Position.Distance(GameManager.LocalPlayer.Position));
	Log(GameManager.LocalPlayer.CurrentTarget.Alias);
}
```

#Dump Inventory
```c#
public static void Run()
{
	Log("Hello World");
	foreach(var item in GameManager.LocalPlayer.InBagItems)
	{
		Log($"{item.ItemId} - {item.Alias}");
	}
}
```
#Dump near by npcs
```c#
public static void Run()
{
	var actors = GameManager.ActorsOfType<Npc>().Where(i => i.Distance <= 3*50f);
	foreach(var a in actors){
		Log($"{a.Alias} -- {a.Name} -- {a.CreatureId}");
	}
}
```

#Dump Skills
```c#
public static void Run()
{
	foreach(var skill in GameManager.LocalPlayer.CurrentSkills)
	{
		Log($"{skill.Name} - {skill.Alias}");
	}
}
```

#Interact With Target
This is part of the QuestHub tool. Note that at time of writing 7/7/16 CurrentTarget does not work for friendlies :'(
```c#
public static void Run()
{
	var Ntarget = GameManager.LocalPlayer.CurrentTarget;
	if(Ntarget != null)
		Log($"<Interact QuestId=\"\" QuestStep=\"\" With=\"{Ntarget.Alias}\" X=\"{Ntarget.Position.X}\" Y=\"{Ntarget.Position.Y}\" Z=\"{Ntarget.Position.Z}\" MapId=\"{GameManager.CurrentMap}\" />");
	else
		Log("NO TARGET");
}
```

#Interact alt
dumps the end of the interact string (zzi's Quest Hub format) 
Offical tags use NpcRecordId instead.

```c#
public static void Run()
{
	var alias = "Ctzn_JinF_CheongRyeongCtzn_RyuBekEon_001";
	var target = GameManager.Actors.FirstOrDefault(i => i.Alias?.ToLower() == alias.ToLower());
	if(target != null)
	{
		Log($"With=\"{target.Alias}\" X=\"{target.Position.X}\" Y=\"{target.Position.Y}\" Z=\"{target.Position.Z}\" MapId=\"{GameManager.CurrentMap}\" />");
		Log($"{target.Name}");
		Log($"CreatureId=\"{target.CreatureId}\"");
	}
	else
		Log($"Failed to find target {alias}");
}
```

#item dump
dumps information about an item from its alias. useful when looking at quest.xml files.

```c#
public static void Run()
{
	var alias = "Quest_Weapon_Dagger_1010";
	var item = GameManager.LocalPlayer.InBagItems.First(i => i.Alias.ToLower() == alias.ToLower());
	if(item != null)
		Log($"{item.ItemId} - {item.Alias}");
	else
		Log("Could not find Item");
}
```

#Move To
```c#
public static void Run()
{
	var target = GameManager.LocalPlayer;
	Log($"<MoveTo QuestId="" Quest X=\"{target.Position.X}\" Y=\"{target.Position.Y}\" Z=\"{target.Position.Z}\" MapId=\"{GameManager.CurrentMap}\" />");
}
```
