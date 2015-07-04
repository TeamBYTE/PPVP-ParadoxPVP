#Main
---
````
package me.MineQuestAgent.ParadoxPVP;

import org.bukkit.Bukkit;
import org.bukkit.ChatColor;
import org.bukkit.Location;
import org.bukkit.World;
import org.bukkit.command.Command;
import org.bukkit.command.CommandSender;
import org.bukkit.entity.Player;
import org.bukkit.plugin.java.JavaPlugin;

public class Main extends JavaPlugin {
	
	public boolean onCommand(CommandSender sender, Command cmd, String commandLabel, String[] args) {
		
		if (!(sender instanceof Player)) {
			sender.sendMessage(ChatColor.DARK_RED + "Sorry, but this plugin is for players only. It cannot be used from console.");
		}
		
		Player p = (Player) sender;
		
			if (cmd.getName().equalsIgnoreCase("tp")) {
				if (args.length == 0) {
					p.sendMessage(ChatColor.DARK_PURPLE + "TELEPORT" + ChatColor.DARK_GRAY + "// " + ChatColor.GRAY + "Error. Please specify the player that you would like to teleport to.");
					return true;
				}
				Player target = Bukkit.getServer().getPlayer(args[0]);
				if (target == null) {
					p.sendMessage(ChatColor.DARK_PURPLE + "TELEPORT" + ChatColor.DARK_GRAY + "// " + ChatColor.GRAY + "Error. We couldn't find " + ChatColor.DARK_PURPLE + args[0] + ChatColor.GRAY + ". Please make sure they are online.");
					return true;
					
				}
				p.teleport(target.getLocation());
				p.sendMessage(ChatColor.DARK_PURPLE + "TELEPORT" + ChatColor.DARK_GRAY + "// " + ChatColor.GRAY + "You were succesfully teleported to " + ChatColor.DARK_PURPLE + args[0] + ChatColor.GRAY + ".");
				return true;
			}
			if(cmd.getName().equalsIgnoreCase("sethub")) {
				getConfig().set("spawn.world", p.getLocation().getWorld());
				getConfig().set("spawn.x", p.getLocation().getX());
				getConfig().set("spawn.y", p.getLocation().getY());
				getConfig().set("spawn.z", p.getLocation().getZ());
				saveConfig();
				p.sendMessage(ChatColor.DARK_PURPLE + "HUB" + ChatColor.DARK_GRAY + "// " + ChatColor.GRAY + "You have successfully set the hub spawn.");
				return true;
			}
			
			if (cmd.getName().equalsIgnoreCase("hub")) {
				if (getConfig().getConfigurationSection("spawn") == null) {
					p.sendMessage(ChatColor.DARK_PURPLE + "HUB" + ChatColor.DARK_GRAY + "// " + ChatColor.GRAY + "Error. The hub location has not been set.");
					return true;
				}
				World w = Bukkit.getServer().getWorld(getConfig().getString("spawn.world"));
				double x = getConfig().getDouble("spawn.x");
				double y = getConfig().getDouble("spawn.y");
				double z = getConfig().getDouble("spawn.z");
				p.teleport(new Location(w, x, y, z));
				p.sendMessage(ChatColor.DARK_PURPLE + "HUB" + ChatColor.DARK_GRAY + "// " + ChatColor.GRAY + "You were successfully teleported to the ParadoxPVP hub!");
			}
		return true;	
	}
}

````
#plugin.yml
---
````
name: ParadoxPVP
main: me.MineQuestAgent.ParadoxPVP.Main
author: MineQuestAgent
version: 1.0
description: The main plugin for the ParadoxPVP network.

commands:

  tp:
    usage: /<command>
    aliases: [teleport]
    description: Use this command to teleport to other players(MOD+).
  sethub:
    usage: /<command>
    aliases: [shub]
    description: Use this command to set the hub of the server(OWNER).
  hub:
    usage: /<command>
    aliases: [spawn, lobby]
    description: Teleport the ParadoxPVP network hub(ALL).
    ````
    
    
    Copyright 2015 ParadoxPVP Networks and MineQuestAgent. All rights reserved. For use on the ParadoxPVP Network servers only.
