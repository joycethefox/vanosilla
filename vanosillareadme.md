# Vanosilla - server
 
## Disclamier
 
If you're here just to get the server running without knowing anything - read the entire [Getting Started](#Getting-Started) section (`Mid-type PC` point below is still valid).
 
**Before doing any actions, you should have**:
- some knowledge about: 
  - object-oriented programming in C#, 
  - asynchronous programming (Task, async, await),
  - LINQ,
  - Depedency Injection
- some knowledge about NosTale packet structure
- some knowledge about what SQL, Lua and YAML are
- Mid-type PC:
  - Processor:
    - Intel Core i5 or higher
    - AMD Ryzen 5 or higher
  - 8GB of RAM or more
  - ~35GB of available disk space
 
___
 
## About
 
WingsEmu (Nos`WingsEmu`lator) is an emulator for the game NosTale. The source is based on NosWings code, but without any NosWings-specific changes. 
 
The source code is from July 17th 2021.
 
Authors:
- `Blowa` - responsible for the structure of the project, the use of appropriate technologies and tools.
- `Quarry` - responsible for most of the gameplay part - from the Battle System, Algorithms, Relation System, Mail & Note System to Time-Spaces, Rainbow Battle, AI and more.
- `Adanlink` - responsible for NosBazaar, Instant Combat, Act4 Dungeon, Family systems, Mini-games, Database Server.
- `Yoshi` - responsible for Quest System, in-game logs.
- `Roxeez` - responsible for Lua handling, Session and cross-channel rework.
- `Tuskk` - responsible for in-game logs.
- `Allan` - responsible for Logs system.
___
 
## Technologies
 
- **PostgreSQL** - player database
- **Redis** - player sessions and player "daily" data caching
- **MongoDB** - player in-game logs
- **gRPC** - connecting RPCs between services
- **EMQX** - service Bus transportation layer broker, MQTT protocol
 
___
 
## Getting Started
 
Install or have:
 
- `IDE` (just one of those is enough):
  - [JetBrains Rider](https://www.jetbrains.com/rider)
  - [Visual Studio 2022](https://visualstudio.microsoft.com)
- [.NET 5 SDK](https://dotnet.microsoft.com/en-us/download/dotnet/5.0)
- [Docker](https://download.docker.com/win/stable/Docker%20for%20Windows%20Installer.exe)
 
___
 
**Additional information for Visual Studio**:
 
Extract `properties_for_visual_studio.zip` file and paste each `Properties` directory to each given project.
 
I recommend using:
- [JetBrains .NET Resharper](https://www.jetbrains.com/resharper) for better code quality
- [SwitchStartupProject](https://marketplace.visualstudio.com/items?itemName=vs-publisher-141975.SwitchStartupProject) extension to create your own projects startup configs. It will be useful to start more projects at once and later more game channels.
 
Download it for your Visual Studio version, install it and restart Visual Studio.
 
___
 
 
If you want to exceute `.ps1` script files via PowerShell, you have to set execution policy. To do that:
- Run PowerShell as Administrator
- Type `Set-ExecutionPolicy RemoteSigned` and press the [ENTER] key
 
You can find more information about it by clicking [here](https://stackoverflow.com/a/4038991).
 
___
 
### Docker Installation
 
First we need to install [Docker](https://opensource.com/resources/what-docker). Run `Docker for Windows Installer.exe` as Administrator and after some seconds, you should see this window:
 
![](https://i.imgur.com/jv2SdvR.png)
 
If you want, you can uncheck `Add shortcut to desktop`. Then click the `Ok` button:
 
![](https://i.imgur.com/Put6Oza.png)
 
After successfully installing Docker, the program will ask you to restart your computer (PC restarting in 2022, yikes).
 
![](https://i.imgur.com/aubgD0A.png)
 
Once you've restarted your computer, run Docker. Before doing anything, you have to accept Docker's terms - click `I accept the terms` checkbox and click `Accept` button:
 
![](https://i.imgur.com/YjvEvmT.png)
 
Okay, it's almost over. Docker needs [WSL 2](https://docs.microsoft.com/en-us/windows/wsl/about) for Linux virtualization. 
 
![](https://i.imgur.com/Xhbddfu.png)
 
Go to the [aka.ms/wsl2kernel](https://docs.microsoft.com/en-us/windows/wsl/install-manual#step-4---download-the-linux-kernel-update-package) website and download the installer by clicking `WSL2 Linux kernel update package for x64 machines` link:
 
![](https://i.imgur.com/eVlbP78.png)
 
Installation is very simple - just run the `wsl_update_x64.msi` installer, click `Next` button and wait for the end and close the installer's window.
 
After installation, restart Docker and wait for everything to load. After a short while you should see `Getting Started with Docker` window:
 
![](https://i.imgur.com/Xk4KRyF.png)
 
Let's skip it using `Skip tutorial` button and... that's it - Congratulations!
 
![](https://i.imgur.com/AUY0n7c.png)
 
### Running Docker
 
To run PostgreSQL, Redis, MongoDB and MQTT Broker for our server, we have to create [Docker containers](https://www.docker.com/resources/what-container). 
 
- **PowerShell**
  - Go to the `.server/scripts/Docker` directory
  - For each file in the directory, click right mouse button on the file and choose `Run with PowerShell`
    - ![](https://i.imgur.com/1ar8Nkd.png)
- **Terminal**
  - Go to the `.server/scripts/Docker` directory
  - Open each `.ps1` file, select the entire script (starts with `docker run`) and copy it into Terminal (you can use this script anywhere) and then press [`ENTER`] key:
    - ![](https://i.imgur.com/auCrU6q.png)
 
After successfully using the commands, you should see 4 new containers in your Docker Hub.
 
![](https://i.imgur.com/Lz5O8PL.png)
 
___
 
### Running the server
 
Finally, we can run the server. First, let's setup multiple startup projects:
 
 
<details>
    <summary><b> --- JSON for Visual Studio --- </b></summary>
 
```json
{
  "Version": 3,
  "ListAllProjects": false,
  "MultiProjectConfigurations": 
  {
    "Server": 
    {
      "Projects": 
      {
        "LoginServer": 
        {
          "ProfileName": "LoginServer",
          "StartProject": true
        },
        "Master": {
          "ProfileName": "Master",
          "StartProject": true
        },
        "DatabaseServer": {
          "ProfileName": "DatabaseServer",
          "StartProject": true
        },
        "TranslationsServer": {
          "ProfileName": "TranslationsServer",
          "StartProject": true
        },
        "FamilyServer": {
          "ProfileName": "FamilyServer",
          "StartProject": true
        },
        "BazaarServer": {
          "ProfileName": "BazaarServer",
          "StartProject": true
        },
        "LogsServer": {
          "ProfileName": "LogsServer",
          "StartProject": true
        },
        "MailServer": {
          "ProfileName": "MailServer",
          "StartProject": true
        },
        "RelationServer": {
          "ProfileName": "RelationServer",
          "StartProject": true
        },
        "Scheduler": {
          "ProfileName": "Scheduler",
          "StartProject": true
        },
        "GameChannel": {
          "ProfileName": "GameChannel",
          "StartProject": true
        }
      }
    }
  }
}
```
</details>
 
 
 
- **Visual Studio 2022**:
  - Click on empty label, expand it and click `Configure...` option:
    - ![](https://i.imgur.com/wEhW37W.png)
  - Remove generated code and copy all content hidden in the **JSON for Visual Studio** section and paste it to the file:
    - ![](https://i.imgur.com/0viABLk.png)
  - Save the file using `CTRL + S` keys and you should see new config when you expand the label again:
    - ![](https://i.imgur.com/hc306lu.png)
  - Now we need to set `Working directory` for each project. To do that, click on small arrow and click `<project-name> Debug Properties`:
    - ![](https://i.imgur.com/7SUxX3A.png)
  - Now set the path to the `dist/<project-name>`, example:
    - `..\server\dist\bazaar-server`
    - ![](https://i.imgur.com/SD93YDK.png)
  - Close the window and repeat for every executable project.
- **JetBrains Rider**:
  - Click `Run` button from the toolbar and choose `Edit Configurations` button:
    - ![](https://i.imgur.com/oCXVptO.png)
  - Click `+` button, scroll down and choose `Compound` option:
    - ![](https://i.imgur.com/WJG7qGG.png)
  - Name it whatever you want
  - Add these projects by clicking `+` button:
    - BazaarServer
    - DatabaseServer
    - FamilyServer
    - GameChannel
    - LoginServer
    - LogsServer
    - MailServer
    - Master
    - RelationServer
    - Scheduler
    - TranslationsServer
    - ![](https://i.imgur.com/Tw7ZY9W.png)
    - ![](https://i.imgur.com/IV2n667.png)
  - For each added project, change `Working directory` path to `dist/<project-name>`, example:
    - `.../server/dist/bazaar-server`
    - ![](https://i.imgur.com/lWbvgcY.png)
 
**Before starting the server, we need to copy resources for our game-server**:
- `./server-files`
- `./server-translations`
- `./client-files`
 
Follow given instruction:
 
- Open PowerShell in `./server` directory
- Type `.\scripts\update-server-files.ps1` and press [`ENTER`] key
 
Next, let's create default accounts to be able to log in:
 
- **PowerShell**:
  - Build `Toolkit` project
  - Go to the `./server` directory
  - Type `.\scripts\Database\default-accounts.ps1` and press [`ENTER`] key
 
- **Terminal**:
  - Build `Toolkit` project
  - Go to the `./server/dist/toolkit` directory
  - Type `Toolkit.exe create-accounts` and press [`ENTER`]
 
Default accounts are:
- Login: `admin` with password: `test`
- Login: `test` with password: `test`
 
After that, click magic button `Run` in your IDE. Wait for each project to build up and run.
 
If all went well, congratulations - you did it!
If not, check the message from the exception.
 
___
 
## Environment Variables
 
[Environment Variables](https://docs.microsoft.com/en-us/dotnet/api/system.environment.getenvironmentvariable) are used to changed some data in diffrent environments - for example, we can have diffrent connection to database while being on localhost and diffrent in the production mode - that's why we can just set environment variables without changing anything in source code.
 
Example of Environment Variables for the database connection:
 
```csharp
Environment.GetEnvironmentVariable("DATABASE_IP") ?? "localhost";
```
 
The `GetEnvironmentVariable` method will return the string - if `DATABASE_IP` key will be present in env. variables (let's give for example `127.0.0.1`), it will return `"127.0.0.1"` string - if not, the method will return [nullable](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/builtin-types/nullable-reference-types) string. To set default value, we're gonna use [`??`](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/null-coalescing-operator) operator to set `"localhost"` string if that happend.
 
We will be using Environment Variables later in this documentation.
 
___
 
## IBattleEntity
 
`IBattleEntity` is an interface that identifies an entity that can move and attack. There are 4 entities that inherit this interface:
 
- `IPlayerEntity` - interface that represents player
- `IMateEntity` - interface that represents player's NosMate
- `IMonsterEntity`- interface that represents monster
- `INpcEntity` - interface that represents NPC
 
Each entity contains the same properties:
- Id
- Type
- Position
- Level
- Speed
- Hp - current health
- Maximum Health
- Mp - current mana
- Maximum Mana
- Faction
- Resistances
  - Fire
  - Water
  - Light
  - Shadow
- Element
- Element Rate
- Size
...and much more.
 
Each `IBattleEntity` also has its own loop system:
- `ICharacterSystem`
- `IMateSystem`
- `IMonsterSystem`
- `INpcSystem`
 
Each `IMapInstance` map contains all 4 systems - more information about systems on this below.
 
## Entity Component System
 
WingsEmu is using [ECS](https://www.guru99.com/entity-component-system.html) for entity.
 
Each `IMapInstance` contains list of entity systems. You can find all information about the number of a given entities (number of players, monsters on the map), but also the method responsible for ticking the map.
`ProcessTick` method refreshes all entities on the map every x ms - removing old entities (e.g. when the player has changed the map or when the monster has been killed and will never respawn), but also the AI of monsters / NPCs - finding opponents, attacking, moving etc.
 
### Component
 
Component helps in keeping order for each entity. In short, instead creating a lot of properties and methods inside entity class, the best solution is creating component to hold some data. 
 
A list of some most-used player's components:
- `IMateComponent`
- `IBCardComponent`
- `IBuffComponent`
- `IEquipmentOptionContainer`
 
___
 
### Creating your own Component
 
Let's say you want to save the number of attempts to upgrade your Specialist Cards and equipment. We will store:
- **Specialist Card**:
  - Successful attempts
  - Failed attempts
  - Burnt Souls
- **Equipment**:
  - Successful attempts
  - Failed attempts
  - Level fixed
 
First, let's create our new interface and store it in `WingsAPI.Game` project inside `EntityStatistics` directory. My component will have a name `IUpgradeStatisticsComponent`:
 
```csharp
public interface IUpgradeStatisticsComponent
{
}
```
 
Okay and now let's add the data we are interested in to this interface:
 
```csharp
public interface IUpgradeStatisticsComponent
{
    ushort SpecialistSuccess { get; set; }
    ushort SpecialistFail { get; set; }
    ushort SpecialistBurntSouls { get; set; }
 
    ushort EqupimentSuccess { get; set; }
    ushort EqupimentFail { get; set; }
    ushort EqupimentLevelFixed { get; set; }
}
```
 
Now it's time to implement this interface into some class, so let's create a new one and name it `UpgradeStatisticsComponent`:
 
```csharp
public class UpgradeStatisticsComponent
{
}
```
 
Now inherit the class with the interface:
 
```csharp
public class UpgradeStatisticsComponent : IUpgradeStatisticsComponent
{
}
```
 
After implementing the methods, the final class should look like this:
 
```csharp
public class UpgradeStatisticsComponent : IUpgradeStatisticsComponent
{
    public ushort SpecialistSuccess { get; set; }
    public ushort SpecialistFail { get; set; }
    public ushort SpecialistBurntSouls { get; set; }
 
    public ushort EqupimentSuccess { get; set; }
    public ushort EqupimentFail { get; set; }
    public ushort EqupimentLevelFixed { get; set; }
}
```
 
Great! Now, it's time to add our created component to the `IPlayerEntity` (it's located in `WingsAPI.Data` project under `Characters` directory).
 
Go to the end of the file and add our new component:
 
```csharp
IUpgradeStatisticsComponent UpgradeStatisticsComponent { get; }
```
 
Now let's move to `PlayerEntity.Stats` class (more information about it below) and implement our component to the class (somewhere at the beginning of the file where all components are stored):
 
```csharp
public IUpgradeStatisticsComponent UpgradeStatisticsComponent { get; }
```
 
After that, go to the `PlayerEntity.cs` file and inside the constructor add component:
 
```csharp
UpgradeStatisticsComponent = new UpgradeStatisticsComponent();
```
 
That's it! Great, it's time to use our properties. For demonstration purposes I will only take care of Specialist Card upgrade.
 
Go to the `SpUpgradeEventHandler.cs` file and then find `SpUpgrade` method and find `upgradeResult` variable.
 
```csharp
SpUpgradeResult upgradeResult = randomBag.GetRandom();
```
 
There should be a switch underneath it and 4 cases:
 
```csharp
switch (upgradeResult)
{
    case SpUpgradeResult.Break when isProtected:
    case SpUpgradeResult.Break:
    case SpUpgradeResult.Succeed:
    case SpUpgradeResult.Fail:
}
```
 
For both `SpUpgradeResult.Break` cases let's increase `SpecialistBurntSouls`:
 
```csharp
session.PlayerEntity.UpgradeStatisticsComponent.SpecialistBurntSouls++;
```
 
And for `SpUpgradeResult.Succeed` and `SpUpgradeResult.Fail` appropriate properties:
 
**Succeed**:
```csharp
session.PlayerEntity.UpgradeStatisticsComponent.SpecialistSuccess++;
```
 
**Fail**:
```csharp
session.PlayerEntity.UpgradeStatisticsComponent.SpecialistFail++;
```
 
Congratulations, that's it! Everytime when the player will upgrade his Specialist Card, he will be able to track his amount of attempts.
 
## Player
 
### IClientSession
 
When player connects to the server, the `IClientSession` is created. It handles all packets sent and received from player, holds data about TcpSession and `IPlayerEntity` itself.
 
While writing various methods, events and commands, you will surely come across `IClientSession`.
 
___
 
### IPlayerEntity
 
`PlayerEntity` class holds all information about player - the `PlayerEntity` class is separated into 5 [partial](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/partial-classes-and-methods) classes:
- `PlayerEntity` - holds main data about the player
- `PlayerEntity.Family` - holds data about the player's family
- `PlayerEntity.Revival` - holds data about player's revival
- `PlayerEntity.Skills` - holds data about player's skills, cooldowns, skill upgrades etc.
- `PlayerEntity.Stats` - holds generic data like player's statistics, quests, mail, notes, equipment etc.
 
___
 
## Commands
 
WingsEmu is using [Qmmands](https://github.com/Quahu/Qmmands) to process player's commands. Commands prefixes are `$` and `%`. If you want to add or remove prefixes, go to the `IClientSession.cs` file and modify this line:
 
```csharp
private static readonly char[] COMMAND_PREFIX = { '$', '%' };
```
 
First, let's look at the main base of commands - the module
 
```csharp
public class SaltyModuleBase : ModuleBase<WingsEmuIngameCommandContext>
{
}
```
 
`SaltyModuleBase` module will help us to identify and store our commands in different places in the solution.
 
I recommend using the name syntax of adding suffix `Module` to created module.
Let's create our module and inherit it with `SaltyModuleBase`. Most of the commands are in `WingsEmu.Plugins.Essentials` project, so we will create our module there as well:
 
```csharp
public class MyCommandsModule : SaltyModuleBase
{
}
```
 
Now when loading a module, all commands inside it will be loaded. Before we even add new commands, let's give this module a name, description and the required authority to execute the commands:
 
```csharp
[Name("My commands")]
[Description("This module is related to new commands.")]
[RequireAuthority(AuthorityType.User)]
public class MyCommandsModule : SaltyModuleBase
{
}
```
 
We can change the authority type so that only Game Master and higher ranks can use these commands:
 
```csharp
[RequireAuthority(AuthorityType.GameMaster)]
````
 
Now it's time to create the command. To do that, create a method inside your module and add `Command` attribute with the name of the command:
 
```csharp
[Name("My commands")]
[Description("This module is related to new commands.")]
[RequireAuthority(AuthorityType.User)]
public class MyCommandsModule : SaltyModuleBase
{
    [Command("ping")]
    public void Ping()
    {
    }
}
```
 
We can also add a detailed description of what this command is for:
 
```csharp
[Name("My commands")]
[Description("This module is related to new commands.")]
[RequireAuthority(AuthorityType.User)]
public class MyCommandsModule : SaltyModuleBase
{
    [Command("ping")]
    [Description("This commands send you a pong message.")]
    public void Ping()
    {
    }
}
```
 
Before we turn on the server, we need to load our module to the server. Go to the `EssentialsPlugin.cs` file in `WingsEmu.Plugins.Essentials` namespace and inside `OnLoad` method add this line:
 
```csharp
_commands.AddModule<MyCommandsModule>();
```
 
Great, now server knows that there is a command inside this module. When starting the server you should notice that your module and your command have loaded:
 
![](https://i.imgur.com/hoBYm4T.png)
 
 
To use a given command in the game, use any of the command prefixes (`$` or `%`) and the name of the command in the chat. In my case it will look like this:
 
```
$ping
```
 
OK, nothing happened... because `Ping()` doesn't do anything yet. If you put a breakpoint inside the command and type the command in the chat, you will see that it worked and that the breakpoint was caught.
 
![](https://i.imgur.com/2Y3TpgG.png)
 
Now, it's time to send a message as the command description says. To get the player who executed the command, you can just take it from `SaltyModuleBase` that inherits `WingsEmuIngameCommandContext` and it inherits `CommandContext` - here is all the information about the command.
 
Okay, lets pull it out finally. We will use `Context.Player` for this and add it to a variable:
 
```csharp
[Command("ping")]
[Description("This command sends you a pong message.")]
public void Ping()
{
    IClientSession session = Context.Player;
}
```
 
Now, let's send him a `Pong` message using green color:
 
```csharp
[Command("ping")]
[Description("This command sends you a pong message.")]
public void Ping()
{
    IClientSession session = Context.Player;
    session.SendChatMessage("Pong", ChatMessageColorType.Green);
}
```
 
Let's run the server and check the results:
 
![](https://i.imgur.com/e5H8cx4.png)
 
Congratulations! Now it's time to play with the parameters a bit - let's add our first parameter to the command.
 
Let's say the command sends the message x times - let's add parameter of type `byte` and name it `times`.
 
```csharp
[Command("ping")]
[Description("This command send you a pong message x times.")]
public void Ping(byte times)
{
}
```
 
Next let's create a `for` loop that send x times our `Pong` message:
 
```csharp
[Command("ping")]
[Description("This command send you a pong message x times.")]
public void Ping(byte times)
{
    IClientSession session = Context.Player;
 
    for (int i = 0; i < times; i++)
    {
        session.SendChatMessage($"Pong", ChatMessageColorType.Green);
    }
}
```
 
Now it's time to use the command - let's say I want get 5 times the `Pong` message - this time I will use command but with the new parameter:
 
```
$ping 5
```
 
Let's run the server again and check the results:
 
![](https://i.imgur.com/2MUxvQo.png)
 
Well done! 
 
Now it's time to use `SaltyCommandResult` class. Let's suppose something bad happened while executing a command or we were expecting different parameters - the player knows nothing about what went wrong.
 
The `SaltyCommandResult` has two parameters in the constructor:
```csharp
public SaltyCommandResult(bool isSuccessful, string message = null)
{
    IsSuccessful = isSuccessful;
    Message = message;
}
```
 
- If given command has been executed successfully
- (optional) The final message when the command is successful or not
 
Now let's change our method from returning nothing to `SaltyCommandResult` class:
 
```csharp
[Command("ping")]
public SaltyCommandResult Ping()
{
}
```
 
Now we have to always return the `SaltyCommandResult` class - let's return successful result:
 
```csharp
[Command("ping")]
public SaltyCommandResult Ping()
{
    return new SaltyCommandResult(true, "Command has been executed successfully! Pong.");
}
```
 
Now, let's return failed executed command:
 
```csharp
[Command("ping")]
public SaltyCommandResult Ping()
{
    return new SaltyCommandResult(false, "Oops, something went wrong...");
}
```
 
I recommend always using `SaltyCommandResult` class as return, because we always know if something bad happened.
 
___
 
### Custom Type Parser
 
Let's say you want to create your own parameter for your command, because you are tired of constantly checking if a certain monster exists.
 
Let's create our own Type Parser sealed class with inherited `TypeParser<>` in `WingsAPI.Commands` in `TypeParsers` directory and named it `MonsterDataTypeParser`:
 
```csharp
public sealed class MonsterDataTypeParser : TypeParser<IMonsterData>
{
}
```
 
The next step is to implement the `ParseAsync` method:
 
```csharp
public sealed class MonsterDataTypeParser : TypeParser<IMonsterData>
{
    public override ValueTask<TypeParserResult<IMonsterData>> ParseAsync(Parameter parameter, string value, CommandContext context) => throw new NotImplementedException();
}
```
 
and finally, let's find if monster exists:
 
 
```csharp
public sealed class MonsterDataTypeParser : TypeParser<IMonsterData>
{
    private readonly INpcMonsterManager _npcMonsterManager;
 
    public MonsterDataTypeParser(INpcMonsterManager npcMonsterManager)
    {
        _npcMonsterManager = npcMonsterManager;
    }
 
    public override ValueTask<TypeParserResult<IMonsterData>> ParseAsync(Parameter param, string value, CommandContext context)
    {
        if (!int.TryParse(value, out int monsterVnum))
        {
            return new ValueTask<TypeParserResult<IMonsterData>>(new TypeParserResult<IMonsterData>($"Couldn't parse value: {value}."));
        }
 
        IMonsterData monsterData = _npcMonsterManager.GetNpc(monsterVnum);
 
        return monsterData is null
            ? new ValueTask<TypeParserResult<IMonsterData>>(new TypeParserResult<IMonsterData>($"Monster with given vnum {value} doesn't exist."))
            : new ValueTask<TypeParserResult<IMonsterData>>(new TypeParserResult<IMonsterData>(monsterData));
    }
}
```
 
All that's left is to add a new TypeParser to the `EssentialsPlugin.cs` file in `OnLoad` method:
 
```csharp
_commands.AddTypeParser(new MonsterDataTypeParser(_npcMonsterManager));
```
 
Now, we can check what's speed have a Fox (monster vnum: 1):
 
```csharp
// Using $monster 1
[Command("monster-speed")]
public SaltyCommandResult CheckMonsterSpeed(IMonsterData monsterData)
{
    return new SaltyCommandResult(true, $"Monster vnum: {monsterData.MonsterVNum} have {monsterData.BaseSpeed} speed.");
}
```
 
___
 
### Remainder
 
`Remainder` class is an attribute that allows us to set the last parameter of the method.
 
Let's say I want to send a message to my friend by using a command:
 
```csharp
[Command("message")]
public SaltyCommandResult FriendMessage(IClientSession friend, string message)
{
    IClientSession session = Context.Player;
 
    friend.SendChatMessage($"{session.PlayerEntity.Name} sent you a message: {message}");
 
    return new SaltyCommandResult(true);
}
```
 
Now when I will use command with some message that contains spaces after the `friend` parameter, let's say:
 
```
$message Jacob Hey, thanks for having me!
```
 
It won't execute. Why? It's because command executor is looking for a command `message` with a certain number of parameters. To let him know, just set ``[Remainder]`` attribute before `string message` parameter:
 
```csharp
[Command("message")]
public SaltyCommandResult FriendMessage(IClientSession friend, [Remainder] string message)
{
    IClientSession session = Context.Player;
 
    friend.SendChatMessage($"{session.PlayerEntity.Name} sent you a message: {message}");
 
    return new SaltyCommandResult(true);
}
```
 
This time the command executor will know that the `message` parameter is the last one and it will all strings after first parameter.
 
___
 
## Events and Event Handlers
 
WingsEmu is based on `Event Driven Architecture` - that means everything is based on events and event handlers. In a nutshell, these are global asynchronous methods available for the entire project(s).
 
The base of the event is `IAsyncEvent`:
 
```csharp
public interface IAsyncEvent
{
}
```
 
Every `IAsyncEvent` has its own handler or even multiple handlers. 
 
To create your own event, you have to inherit `IAsyncEvent` in your class. I recommend using the name syntax of adding suffix `Event` to your class:
 
```csharp
public class GiveItemsEvent : IAsyncEvent
{
}
```
 
Of course each event can have its own properties - data that can be later used in an event handler:
 
```csharp
public class GiveItemsEvent : IAsyncEvent
{
    public List<int> ItemVnums { get; set; }
    public IClientSession Receiver { get; set; }
}
```
 
OK, we have our own event - it's time to create the event handler.
 
`IAsyncEventProcessor<T>` is responsible for event handling, where the `T` is the `IAsyncEvent` like our event class.
 
Let's create our handler by making a new class `GiveItemsEventHandler` - and again, I recommend using the name syntax of adding suffix `EventHandler` to your class:
 
```csharp
public class GiveItemsEventHandler : IAsyncEventProcessor<GiveItemsEvent>
{
}
```
 
`IAsyncEventProcessor` interface contains `Task HandleAsync()` method which we need to implement:
 
```csharp
public class GiveItemsEventHandler : IAsyncEventProcessor<GiveItemsEvent>
{
    public async Task HandleAsync(GiveItemsEvent e, CancellationToken cancellation)
    {
    }
}
```
 
When the event happens, it will go to the `HandleAsync` method of the event handler with the `e` paramether - the event's data that we sent earlier:
 
```csharp
public class GiveItemsEventHandler : IAsyncEventProcessor<GiveItemsEvent>
{
    public async Task HandleAsync(GiveItemsEvent e, CancellationToken cancellation)
    {
        List<int> itemVnums = e.ItemVnums;
        IClientSession receiver = e.Receiver;
    }
}
```
 
Now you're probably asking:
- `Okay, everything is ready... but how do you execute this event?`
 
The answer is... `IAsyncEventPipeline` - with its help you can trigger an event.
 
Let's say I have a command that gives a list of items to some player (for more information about commands, check [Commands](#Commands) section):
 
```csharp
[Name("Items Module")]
[RequireAuthority(AuthorityType.SuperGameMaster)]
public class ItemModule : SaltyModuleBase
{
    // Event executor
    private readonly IAsyncEventPipeline _asyncEventPipeline;
 
    public ItemModule(IAsyncEventPipeline asyncEventPipeline)
    {
        _asyncEventPipeline = asyncEventPipeline;
    }
 
    [Command("give")]
    public async Task<SaltyCommandResult> GiveAsync(IClientSession receiver, string itemVnums)
    {
        if (string.IsNullOrWhiteSpace(itemVnums))
        {
            return new SaltyCommandResult(false, "You must specify an items to give.");
        }
 
        // We will use string for itemVnums to seperate numbers using ; as seperator -> 1;42;50 etc.
        string[] itemVnumsArray = itemVnums.Split(';');
 
        if (itemVnumsArray.Length == 0)
        {
            return new SaltyCommandResult(false, "You must specify an items to give.");
        }
 
        var itemVnumsList = new List<int>();
 
        foreach (string itemVnum in itemVnumsArray)
        {
            if (!int.TryParse(itemVnum, out int itemVnumParsed))
            {
                continue;
            }
 
            itemVnumsList.Add(itemVnumParsed);
        }
 
        // Create new event
        var giveItemsEvent = new GiveItemsEvent()
        {
            ItemVnums = itemVnumsList,
            Receiver = recevier
        };
 
        // Execute event
        await _asyncEventPipeline.ProcessEventAsync(giveItemsEvent);
 
        return new SaltyCommandResult(true, "Items has been sent!");
    }
}
```
 
Now let's move to the our event handler and let's change it to give items to the receiver:
 
```csharp
public class GiveItemsEventHandler : IAsyncEventProcessor<GiveItemsEvent>
{
    private readonly IGameItemInstanceFactory _gameItemInstanceFactory;
 
    public GiveItemsEventHandler(IGameItemInstanceFactory gameItemInstanceFactory)
    {
        _gameItemInstanceFactory = gameItemInstanceFactory;
    }
 
    public async Task HandleAsync(GiveItemsEvent e, CancellationToken cancellation)
    {
        List<int> itemVnums = e.ItemVnums;
        IClientSession receiver = e.Receiver;
 
        if (receiver is null)
        {
            return;
        }
 
        if (itemVnums is null || itemVnums.Count < 1)
        {
            return;
        }
 
        foreach (int itemVnum in itemVnums)
        {
            GameItemInstance newItem = _gameItemInstanceFactory.CreateItem(itemVnum);
            if (newItem is null)
            {
                // The item couldn't be created because it doesn't exist
                continue;
            }
 
            await receiver.AddNewItemToInventory(newItem);
        }
    }
}
```
 
Congratulations - that's it! Now each time this event is executed, the `receiver` will receive the 
items that were added to the list.
 
___
 
### PlayerEvent
 
`PlayerEvent` is a a base class to create an event for `IClientSession`. Instead of constantly initializing `IAsyncEventPipeline`, we can use `EmitEventAsync` method inside `IClientSession`.
 
First, let's check what does `PlayerEvent` contain:
 
```csharp
public class PlayerEvent : IAsyncEvent
{
    public IClientSession Sender { get; set; }
}
```
 
As you can see, each time an event is executed, we will have a player who performed that event - and as before, we can add our own data to our own event:
 
 
```csharp
public class ReportPlayerEvent : PlayerEvent
{
    public string TargetPlayerName { get; set; }
    public string Reason { get; set; }
}
```
 
Now, if we want to perform an event, just use `EmitEventAsync` from `IClientSession`:
 
```csharp
// Let's say this is me as IClientSession
IClientSession player = me;
 
var reportPlayerEvent = new ReportPlayerEvent()
{
    TargetPlayerName = "Jacob",
    Reason = "Saying bad words to the Game Master"
};
 
await player.EmitEventAsync(reportPlayerEvent);
```
 
You can even reduce amount of code executing event by doing that:
 
```csharp
// Let's say this is me as IClientSession
IClientSession player = me;
 
await player.EmitEventAsync(new ReportPlayerEvent()
{
    TargetPlayerName = "Jacob",
    Reason = "Saying bad words to the Game Master"
});
```
 
Of course, the handler for this event will look like this:
 
```csharp
public class ReportPlayerEventHandler : IAsyncEventProcessor<ReportPlayerEvent>
{
    private readonly ISessionManager _sessionManager;
 
    public ReportPlayerEventHandler(ISessionManager sessionManager)
    {
        _sessionManager = sessionManager;
    }
 
    public async Task HandleAsync(ReportPlayerEvent e, CancellationToken cancellation)
    {
        IClientSession sender = e.Sender; // Player who executed this event
        string targetPlayerName = e.TargetPlayerName;
        string reason = e.Reason;
 
        if (string.IsNullOrEmpty(targetPlayerName))
        {
            return;
        }
 
        if (string.IsNullOrEmpty(reason))
        {
            return;
        }
 
        // Find player's session in current channel
        IClientSession target = _sessionManager.GetSessionByCharacterName(targetPlayerName);
 
        // If player is offline
        if (target is null)
        {
            return;
        }
 
        // Create final reason to the Game Master
        string finalReason = $"{sender.PlayerEntity.Name} reported {target.PlayerEntity.Name}, reason: {reason}";
 
        // This method will send a chat message to the Game Masters
        await target.NotifyStrangeBehavior(StrangeBehaviorSeverity.NORMAL, finalReason);
    }
}
```
 
When you will be creating new events, you will almost always use `PlayerEvent` instead of` IAsyncEvent` class for the player.
 
___
 
### IBattleEntityEvent
 
`IBattleEntityEvent` is a a base class to create an event for `IBattleEntity`.
 
```csharp
public interface IBattleEntityEvent : IAsyncEvent
{
    IBattleEntity Entity { get; }
}
```
 
Examples of using this version of the event are e.g. death of a entity, attacking etc.
 
___
 
## New Game Channels
 
To run additional channel(s) for our server, we need to create an executable profile. Before do that, let's check what [Environment Variables](#Environment-Variables) are available - we can find them in `WorldServerSingleton` class:
 
- `GAME_SERVER_IP` - channel IP
- `GAME_SERVER_PORT` - channel port
- `GAME_SERVER_GROUP` - channel server group
- `GAME_SERVER_SESSION_LIMIT` - channel session limit
- `GAME_SERVER_CHANNEL_ID` - channel ID
- `GAME_SERVER_CHANNEL_TYPE` - channel type (PVE_NORMAL or ACT_4)
- `GAME_SERVER_AUTHORITY` - channel required authority to join the channel
 
To create basic channel, we need to change:
- `GAME_SERVER_PORT`,
- `GAME_SERVER_CHANNEL_ID`
 
and one more thing for Kestrel port:
 
- `HTTP_LISTEN_PORT`
 
In example I will create second channel, so my env. my variables will look like this:
 
- `GAME_SERVER_PORT` = 8001,
- `GAME_SERVER_CHANNEL_ID` = 2,
- `HTTP_LISTEN_PORT` = 17501
 
 
 
- **Visual Studio 2022**:
  - Select `GameChannel` project, click small arrow and choose `GameChannel Debug Properties` option:
    - ![](https://i.imgur.com/9VQxuXU.png) 
  - Click first button named `Create a new profile` and choose second option `Executable`:
    - ![](https://i.imgur.com/3QA3uaW.png)
  - Select your `Exectuable` and `Working directory` path:
    - ![](https://i.imgur.com/AeTzXuk.png)
  - Scroll down and find `Environment variables` part - now we need to create own env. variables. As description says, we need to seperating each variable using comma `,`. My variables will look like this: 
    - `GAME_SERVER_PORT=8001,GAME_SERVER_CHANNEL_ID=2,HTTP_LISTEN_PORT=17501`
    - ![](https://i.imgur.com/V00WLXA.png)
  - Let's change name of this profile to know what channel it is. Click last button in the menu and change it whatever you want (I will name it `Channel 2`):
    - ![](https://i.imgur.com/al45dNa.png)
  - Now we have to add our created channel to the startup projects. Expand our config label and click `Configure...` option:
    - ![](https://i.imgur.com/lu2393h.png)
  - Select given part of the code, copy and paste it below:
    - ![](https://i.imgur.com/LhOsYoV.png)
    - ![](https://i.imgur.com/pXzWmoO.png)
  - Rename it as your profile name in both places and save the file:
    - ![](https://i.imgur.com/zAKfttC.png)
- **JetBrains Rider**:
  - Click `Run` button from the toolbar and choose `Edit Configurations` button:
    - ![](https://i.imgur.com/oCXVptO.png)
  - Click `+` button and choose `.NET Project` option:
    - ![](https://i.imgur.com/rbYFHFJ.png)
  - Let's change the name and choose `GameChannel` in `Project` section:
    - ![](https://i.imgur.com/l9rEP8h.png)
  - Change `Working directory` path to `dist/game-server`:
    - ![](https://i.imgur.com/1cNJNNN.png)
  - Now it's time to set env. variables. The seperator between variables is `;`, so my variables will look like this: 
    - `GAME_SERVER_PORT=8001;GAME_SERVER_CHANNEL_ID=2;HTTP_LISTEN_PORT=17501`
    - ![](https://i.imgur.com/vhZWjYp.png)
  - Apply changes. To add created `.NET Project` to your compound, just click `+` button and choose the project:
    - ![](https://i.imgur.com/rYr9SPf.png)
 
### Act 4 Channel
 
To run Act 4 channel, follow steps above but set these env. variables:
 
- `GAME_SERVER_PORT` = 8051,
- `GAME_SERVER_CHANNEL_ID` = 51,
- `GAME_SERVER_CHANNEL_TYPE` = ACT_4,
- `HTTP_LISTEN_PORT` = 17551
 
Environment Variables strings:
 
- **Visual Studio 2022** - `GAME_SERVER_PORT=8051,GAME_SERVER_CHANNEL_ID=51,GAME_SERVER_CHANNEL_TYPE=ACT_4,HTTP_LISTEN_PORT=17551`
- **JetBrains Rider** - `GAME_SERVER_PORT=8051;GAME_SERVER_CHANNEL_ID=51;GAME_SERVER_CHANNEL_TYPE=ACT_4;HTTP_LISTEN_PORT=17551`