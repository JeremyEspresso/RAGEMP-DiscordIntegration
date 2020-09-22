# RAGEMP-DiscordIntegration
This wrapper allows you easily create an instance of a discord bot within your RAGE:MP server.

Features:
1. Send messages to discord from your RAGE:MP Server.
2. Send messages to your RAGE:MP Server from your Discord server.
3. Register specific channel for the bot to listen. (Can be changed during runtime).
3. Remove specific channel for the bot to STOP listening. (Can be changed during runtime).
4. Update bot status on setup and/or during runtime

# Review(s)
![itsok](https://i.imgur.com/1hz3CU5.png) ![jamesdoesntgiveafuck](https://i.imgur.com/fEQbxIN.png)

# How to set up
1. Create a new application on [Discord Developers](https://discord.com/developers/applications)
2. Create a bot.
3. Invite bot to discord server.
4. Use the token from your bot to initialize the bot as shown in the example below.
5. Register/Remove channels from where your bot sends to all players.

# Example script:
```cs
using GTANetworkAPI;
using System;
using System.Collections.Generic;
using System.Text;

public class Yes : Script
{
    public Yes()
    {
        NAPI.Util.ConsoleOutput("Loaded: yes");
    }

    [ServerEvent(Event.ResourceStart)]
    public void OnResourceStart()
    {
        Integration.DiscordIntegration.SetUpBotInstance("TOKEN_HERE", "RAGE:MP", Discord.ActivityType.Playing, Discord.UserStatus.DoNotDisturb);
    }

    [ServerEvent(Event.ChatMessage)]
    public async void OnChatMessage(Player player, string strMessage)
    {
        string strFormatted = $"[RAGE:MP] {player.Name}: {strMessage}";
        await Integration.DiscordIntegration.SendMessage(3897429387492374, strFormatted, true).ConfigureAwait(true);
    }

    [Command("registerchannel")]
    public void RegisterDiscord(Player player, ulong discordChannelID)
    {
        bool bSuccess = Integration.DiscordIntegration.RegisterChannelForListenting(discordChannelID);

        player.SendChatMessage(bSuccess ? "Success" : "No Success");
    }

    [Command("removechannel")]
    public void RemoveDiscordChannel(Player player, ulong discordChannelID)
    {
        bool bSuccess = Integration.DiscordIntegration.RemoveChannelFromListening(discordChannelID);

        player.SendChatMessage(bSuccess ? "Success" : "No Success");
    }

    [Command("botstatus")]
    public async void UpdateBotStatusCommand(Player player, string gameName, Discord.ActivityType eActivityType, Discord.UserStatus eUserStatus)
    {
        await Integration.DiscordIntegration.UpdateStatus(gameName, eActivityType, eUserStatus).ConfigureAwait(true);
    }

}

```
