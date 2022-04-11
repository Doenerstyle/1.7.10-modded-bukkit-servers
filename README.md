# Modded servers in Minecraft 1.7.10 in 2022
It's [been some time](https://howoldisminecraft1710.today) since the release of Minecraft 1.7.10. It is, however, still one of the Minecraft versions with the broadest mod support and many modpacks have been and are still being created in 1.7.10.

While very most active modders have moved on to newer versions, some efforts of the community still go into support for 1.7.10 servers, mods, plugins, ...

As getting an overview of such an old version can be quite exhausting and confusing, this guide aims to provide you with information, links, and tips regarding Minecraft 1.7.10 modding and server hosting. If you find anything inaccurate or know something that seems like it belongs here, you're welcome to open an issue or create a pull request!

# Finding the best server software to use

**This guide assumes that you have concrete reasons to stay on Minecraft 1.7.10, such as specific mods not available for newer versions, and are aware of potential risks of using *very* outdated versions, as well as the benefits of using newer versions.**

There are major differences between Forge servers with and without support of Bukkit plugins. A good overview of them can be found [here](https://madelinemiller.dev/blog/minecraft-hybrid-servers).

## Forge servers WITHOUT plugin support
Setting up a Forge server which does not need to support Bukkit plugins, is a [fairly easy progress](https://minecraft.fandom.com/wiki/Tutorials/Setting_up_a_Minecraft_Forge_server#1.6_to_1.16.5). This guide focuses on Forge servers that also support Bukkit plugins.

## Forge servers WITH plugin support (Thermos-like)
Initially, [md-5](https://github.com/md-5) created the Spigot based MCPC/MCPC+ [around back in 2013](https://www.minecraftforum.net/forums/support/server-support-and/1883281-what-is-mcpc), which first allowed to combine plugin and mod support.

There have been way too many forks and forks of forks (and so on) of MCPC+ to name them all, but (in regards to 1.7.10) the arguably most important steps were:

MCPC/MCPC+ -> [Cauldron](https://sourceforge.net/projects/cauldron-unofficial/files/1.7.10) -> [KCauldron](https://sourceforge.net/projects/kcauldron/files) -> [Thermos](https://github.com/CyberdyneCC/Thermos)

If any of these, Thermos should be used. There is, however, a promising newer project/fork still receiving updates to the day of writing: [**Crucible**](https://github.com/CrucibleMC/Crucible)

There is also the fork "Mohist", but there appear to be major reasons against using that ([Source 1](https://github.com/kennytv/list-of-shame), [Source 2](https://essentialsx.net/do-not-use-mohist.html)). In Short: Mohist is accused of breaking the Bukkit API and is de facto replacing the files of the very popular plugin EssentialsX with their own ones, albeit only after the user confirms to do so.

Apart from these two, there are probably hundreds of other forks of Thermos. While some of them might be genuinely improved versions, it is generally not recommended to use any of them, as potential "optimizations" are a good way to break things, especially when combining mod and plugin support.

### SECURITY RISK: Log4J/Log4Shell
It is generally a bad idea to use any pre-1.18.1 server software that has not been patched against the [CVE-2021-44228](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-44228) vulnerability.
**Any server software released prior to 9/12/2021 is generally vulnerable to this remote control exploit.**
If you insist on staying on such a version, follow [Mojang's guide to patching the vulnerability manually](https://www.minecraft.net/nl-nl/article/important-message--security-vulnerability-java-edition).

Crucible has been patched to fix the vulnerability within only 4 days.

## Personal experiences with Thermos and Crucible
After hosting several Thermos servers (usually between 80 and 160 mods, about 10-15 partly self-coded plugins) for me and my friends (up to 10 people at a time) to play on, I'd say that Thermos is indeed a stable software, at least for my use case. I've sticked to the second last version, [57](https://github.com/CyberdyneCC/Thermos/releases/tag/57), however, due to a reason I sadly don't remember.

Bugs we've encountered are an annoyance, but not too major:
* IndustrialCraft² Forge Hammers and Cable Cutters can be used infinitely
* For some weird reason, random dirt blocks would spawn in the air near players. This occured maybe every few hours and really is more funny than disturbing. Our record was around 20-25 dirt blocks, which can easily be removed within 1 minute or 2.
* Likely some more smaller ones, that I've forgotten by now.

Performance has never been an issue apart from generating new chunks (likely due to the rather weak hardware), but we've never had too many machines/chunkloaders/things eating up performance, anyway.

Regarding **Crucible**, I've just started testing it. So far, I can only confirm that it does run seemingly stable (which, considering the ongoing support, should be fairly expectable) and that the IC² bug has been fixed, which is a great sign to me.

I've also used Cauldron and MCPC+ before, and both of them were generally working fine - that was some years ago, however.

**In conclusion, I'd recommend using Crucible. Or, if that is not possible, opening an issue on Crucible's GitHub and continuing with a Log4Shell patched Thermos for the moment.**

# Optimizing the server
## General
Overall, it is recommended to always keep your software up to date: If possible, you should always be using the newest 64 bit version of [Java 8](https://java.com/de/download/manual.jsp) and regularly check for updates of Crucible. All mods and plugins used should preferably be the latest version available for 1.7.10 as well. Also, for the sake of completeness: Make sure you're allocating enough RAM for the server (using the -Xmx JVM flag, as described in the guide linked below) while still leaving some memory for the rest of the system.

**A good introduction to modern performance tuning and measuring of Minecraft servers can be found [here](https://github.com/YouHaveTrouble/minecraft-optimization).** However, that guide focuses on the new(est) versions of Minecraft. Paying close attention to server.properties, bukkit.yml and spigot.yml options should be helpful, nonetheless. The guide also explains some red flags of plugins to be aware of.

Crucible offers a wide variety of different config files and options, as it is built on top of Thermos and thus Spigot and offers its own configuration file, too.

Generally, you should consider to **pre-generate the map(s)** and to **disable any dimensions that you strictly do not intend to use**.
It might also be helpful for your server as well as players with a weak computer/connection to reduce the **view-distance**, which can be set **between 4 and 10** in Minecraft 1.7.10.

If you're *that* perfectionistic, the spigot.yml also allows you to change the messages shown to players when joining without success or entering unknown commands.

## Crucible benefit: Grimoire
Another way to potentially fix issues and optimize the server is using [**Grimoire**](https://github.com/CrucibleMC/Grimoire-legacy).
Grimoire is a coremod that allows you to add so-called "mixins" (based on [Sponge mixins](https://github.com/SpongePowered/Mixin)) to be loaded. Each of these mixins is made for a specific mod, mostly even for a specific item/object/part of the mod. Mixins help the server handling plugins and mods working together and often also **fix specific bugs or unwanted behaviors, such as console spam, crashes, player hacks or undoubtedly overpowered items**.

**It is highly recommended to at least use the NEI mixin, which fixes an exploit were players could force themselves to become a server operator.**

All mixins can be found [here](https://github.com/CrucibleMC/Grimoire-Mixins-1.7.10).

# Useful links

* **Interested in using mods or creating modpacks?** Check out [**my tips and resources on 1.7.10 modding in 2022**](https://github.com/Doenerstyle/1.7.10-mods-in-2022).

* [**kennytv's "The List Of Shame"**](https://github.com/kennytv/list-of-shame) - A collection of Minecraft-related things to stay away from (although I personally do not agree to *all* of them, it does make fair points)
* [**Incendo's "Awesome Minecraft"**](https://github.com/Incendo/awesome-minecraft) - "A curated list of awesome (free) open-source frameworks, libraries and software for Minecraft." (linked on The List Of Shame)

##
Long live Minecraft 1.7.10.
