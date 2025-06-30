# Court Smith

## Contact
- 318-880-7463
- CourtSmith1101@gmail.com
- Game and Web Development
- Since 2009

# Table of Contents
| Section      | Description |
| ----------- | ----------- |
| Experience      | Company Projects - No Source      |
| Projects   | Personal and Contract + Source Available      |

# Experience
Game Design and Production Instructor
- Creating course projects to demonstrate the use of C++, Blueprint, AI, Weapons, and Narrative

Looty Games - Avatar Air Bender Fighting Game
- Missions System
- Emote System
- Tutorial
- Leaderboard
- Sound System
- Round System

### Video
[![](https://img.youtube.com/vi/PenfqJHtU6w/0.jpg)](https://www.youtube.com/watch?v=PenfqJHtU6w)

GAME: https://www.roblox.com/games/101874364639492/Avatar-Legends-Bending-Grounds-BETA

Twin Atlas - MTV, Teen Wolf, West Elm
- House Editor
- Voting System
- Obstacle Course
- Round System
- Shop System

### Video
[![](https://img.youtube.com/vi/_JgCbbQr-3o/0.jpg)](https://www.youtube.com/watch?v=_JgCbbQr-3o)

GAME: https://www.roblox.com/games/11864182128/NEW-MAP-Werewolf-Escape

### Video
[![](https://img.youtube.com/vi/QI2XYDuGuNw/0.jpg)](https://www.youtube.com/watch?v=QI2XYDuGuNw)

GAME: https://www.roblox.com/games/9680886326/NEW-MINIGAME-West-Elm-Home-Design

### Video
[![](https://img.youtube.com/vi/FYDpSNdZZhw/0.jpg)](https://www.youtube.com/watch?v=FYDpSNdZZhw)

CUDO - CUDO World RPG (Unreleased)
- Chest Reward System
- Shop System

Fishing Simulator
- Live Operations Data Management Tools

### Video
[![](https://img.youtube.com/vi/Zft_1YW6fkY/0.jpg)](https://www.youtube.com/watch?v=Zft_1YW6fkY)

PLAY: https://www.roblox.com/games/2866967438/Fishing-Simulator

# Projects
- Necromancers Tomb (Unreal Engine, Blueprint)
- Clan Labs (Node.js, JavaScript, Web, API)
- Area 24 (Roblox, Lua, Weapons, AI, RPG Systems)

## Necromancers Tomb - Blueprint
![](https://i.ibb.co/mrjnqDzW/Screenshot-57.png)

SOURCE: [SOON]

### About
- A project to test AI, Obstacles, Hiding Mechanic, and Unlocking Doors

### Features
- Patrolling AI
- Stealth Hide in Corners
- Floor Trap with Delay
- Collecable
- Unlockable Door
- Objective UI

### Code Snapshots

#### Shadow Cover
![](https://i.ibb.co/WWZCt8yZ/blueprint.png)
GRAPH: https://blueprintue.com/blueprint/njj2tnol/

#### Trap
![](https://i.ibb.co/ZpGxJZgZ/blueprint-1.png)
GRAPH: https://blueprintue.com/blueprint/ruoybuci/

## Clan Labs - Private Snapshot (Request Only) - JavaScript
<img src="https://clanlabs.co/resources/logo.png" width="200" height="200">

SOURCE CODE: https://github.com/TechProgramming/Clan-Labs-Snapshot

### About
- This is a older snapshot of a commercial project called "Clan Labs"
- The web API for customers to talk to the database.
- A discord bot is used to manage users and data within customer "groups"
- Created in 2018
- 3000+ Groups
- $5/mo

### Technology Stack
- MongoDB
- Express.js
- Discord.js
- Mongoose.js
- Node.js

### Features
- User Profiles
- User XP
- Quota Tracking
- Blacklist
- Automated Background Check
- Warnings
- Permission System
- Customization
- Subscription System
- Developer Web API

### Architecture
![](https://i.ibb.co/8DgHvPfZ/rs-w-1280-h-796.webp)

### Website
https://clanlabs.co/

### Tutorial
[![](https://img.youtube.com/vi/25bCIIZ-yYU/0.jpg)](https://www.youtube.com/watch?v=25bCIIZ-yYU)

### Excerpt - User API
SOURCE: https://github.com/TechProgramming/Clan-Labs-Snapshot/blob/main/ClanLabs-API/src/services/user.service.js

```javascript
const setUserExperience = async (group, userId, amount) => {

    // Get User

    let userData = await getUserById(group, userId);
    if(!userData) {
        userData = await createUserById(group, userId);
        if(userData == "User doesn't exist on Roblox") return userData;
    }

    // XP Magic? - Thanks Doge
    
    if(amount < userData.experience) {
        amount = userData.experience - amount;
        amount = -amount;
    } else if(amount > userData.experience) amount - amount - userData.experience;

    // Clamp to Settings

    let maxChangeAmount = group.settings.experienceAPIAwardLimit;
    if(amount >= 0) amount = require('../utils/clamp.js')(amount, 0, maxChangeAmount)
    else amount = require('../utils/clamp.js')(amount, -maxChangeAmount, 0);

    // Increment XP

    let newUserData = await userDB.incrementXP(group, userId, amount);

    // Create Audit Log

    auditService.createAudit(group, {
        targetId: newUserData.userid,
        targetName: newUserData.username,
        auditString: `(**API**): Changed **${newUserData.username}**'s XP from **${userData ? userData.experience : "0"}** to **${newUserData.experience + amount}**!`,
    });

    // Auto Rank Member
    
    await (require('../utils/autoRank.js')(group, newUserData));

    // Get Updated User

    newUserData = await getUserById(group, userId);
    return newUserData;

}
```

## Area 24 - Lua
![](https://tr.rbxcdn.com/180DAY-cf94f30a15d3c9f31203d27ed53a0b30/768/432/Image/Webp/noFilter)

SOURCE CODE: https://github.com/TechProgramming/Area-24

GAME: https://www.roblox.com/games/7027296354/SCP-Area-24

### About
This is a project that was developed as a fan game for an SCP community where you could roleplay different teams and SCP creatures.

### Features
- Weapons
- Teams
- Camera System
- Area Detection
- Lighting Management
- Time Management
- Items and Interaction
- Enemy AI
- Vehicles
- Currency
- Shop
- Monetization
- Character Customization
- Daily Login Award
- Jobs
- Data Saving

### Excerpt - SCP 042 AI
SOURCE: https://github.com/TechProgramming/Area-24/blob/master/src/Server/Server/Services/Core/SCP/Entities/035.luau

```lua
function SCP:BreachWander()
	
	if script:GetAttribute("Debug_Print") then
		print("035 Breach Wandering");
	end;
	local scpHumanoidRootPart = self.Agent.Character.HumanoidRootPart;
	local scpPosition = scpHumanoidRootPart.Position;
	
	if not self.Agent.Directive then
		if script:GetAttribute("Debug_Print") then
			print("035 No Directive is Available, requesting new Directive");
		end;
		math.randomseed(tick());
		local nodeTarget = SCPNodemap:GetChildren()[math.random(1, #SCPNodemap:GetChildren())]:GetAttribute("NodePosition");
		local success = AgentService:CreateDirective(self.Agent, nodeTarget, SCPNodemap);
		self:BreachWander();
	else
		if script:GetAttribute("Debug_Print") then
			print("035 Directive Available, continuing");
		end;
		SCP:DebugDirective();
		if self.Agent.WaypointType ~= "Breach" then
			local nodeTarget = self.Agent.Directive[1];
			local success = AgentService:CreateWaypoints(self.Agent,
				Vector3.new(math.random(scpPosition.X - PATH_START_BIAS, scpPosition.X + PATH_START_BIAS), scpPosition.Y,math.random(scpPosition.Z - PATH_START_BIAS, scpPosition.Z + PATH_START_BIAS)),
				Vector3.new(math.random(nodeTarget.X - PATH_END_BIAS, nodeTarget.X + PATH_END_BIAS), nodeTarget.Y, math.random(nodeTarget.Z - PATH_END_BIAS, nodeTarget.Z + PATH_END_BIAS)),
				"Breach");
			if success then
				self:BreachWander();
			else
				self:RemoveWaypoint();
				if not self.Agent.Directed then
					self.Agent.Directed = 0;
				end;
				self.Agent.Directed += 1;
				if self.Agent.Directed >= DIRECT_TIMEOUT then
					AgentService:ClearDirective(self.Agent);
				end;
			end;
		else
			
			self:DoWaypoint();
			
			if (Normalize(self.Agent.Directive[1]) - Normalize(scpHumanoidRootPart.Position)).magnitude <= DIRECTIVE_REACH_RADIUS then
				
				if math.random(1, 100) <= SPEECH_CHANCE then
					SCP:Speak();
				end;
				table.remove(self.Agent.Directive, 1);
				self:RemoveWaypoint();
				if #self.Agent.Directive <= 0 then
					AgentService:ClearDirective(self.Agent);
				else
					self:BreachWander();
				end;
			end;
		end;
	end;
end
```

