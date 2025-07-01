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
| Demonstrations | Videos of Demonstrating Systems where code is not available |

# Experience
Game Design and Production Instructor
- Creating course projects for Unreal Engine 5.

Looty Games - Avatar Air Bender Fighting Game (Lua)
- Missions System
- Emote System
- Tutorial
- Leaderboard
- Sound System
- Round System

[VIDEO](https://www.youtube.com/watch?v=PenfqJHtU6w) |
[GAME](https://www.roblox.com/games/101874364639492/Avatar-Legends-Bending-Grounds-BETA)

Twin Atlas - MTV, Teen Wolf, West Elm (Lua)
- House Editor
- Voting System
- Obstacle Course
- Round System
- Shop System

[WEREWOLF ESCAPE PROMO](https://www.youtube.com/watch?v=_JgCbbQr-3o) | 
[WEREWOLF ESCAPE GAME](https://www.roblox.com/games/11864182128/NEW-MAP-Werewolf-Escape)

[WELD ELM PROMO](https://www.youtube.com/watch?v=QI2XYDuGuNw) | 
[WEST ELM GAME](https://www.roblox.com/games/9680886326/NEW-MINIGAME-West-Elm-Home-Design)

[MTV PROMO](https://www.youtube.com/watch?v=FYDpSNdZZhw)


CUDO - CUDO World RPG (Unreleased) (Lua)
- Chest Reward System
- Shop System

Fishing Simulator (Lua, JavaScript)
- Live Operations Data Management Tools

### Video Demonstration
[![](https://img.youtube.com/vi/Zft_1YW6fkY/0.jpg)](https://www.youtube.com/watch?v=Zft_1YW6fkY)

[GAME](https://www.roblox.com/games/2866967438/Fishing-Simulator)

# Projects
- Fallen Star Horizon (Unreal Engine, C++)
- Unreal VR Template Conversion to C++ (Unreal Engine, C++)
- Necromancer's Tomb (Unreal Engine, Blueprint)
- Clan Labs (Node.js, JavaScript, Web, API)
- Area 24 (Roblox, Lua, Weapons, AI, RPG Systems)

## Fallen Star Horizon - C++ - Current Work in Progress
![](https://i.ibb.co/s9nJrP2s/Screenshot-58.png)

### About
A project for testing networking and top-down gameplay.


### Excerpt - Interaction Component
SOURCE: https://github.com/TechProgramming/Fallen-Star-Horizon
```cpp
// Fill out your copyright notice in the Description page of Project Settings.


#include "FSHInteractComponent.h"

#include "Components/SphereComponent.h"
#include "FallenStarHorizon/Character/FSHCharacter.h"
#include "FallenStarHorizon/UserInterface/HUD/FSHInteractOption.h"
#include "FallenStarHorizon/Weapon/FSHWeapon.h"


// Sets default values for this component's properties
UFSHInteractComponent::UFSHInteractComponent()
{
	// Set this component to be initialized when the game starts, and to be ticked every frame.  You can turn these features
	// off to improve performance if you don't need them.
	PrimaryComponentTick.bCanEverTick = true;
	
}


// Called when the game starts
void UFSHInteractComponent::BeginPlay()
{
	Super::BeginPlay();

	AActor* Owner = this->GetOwner();
	SetWidgetVisible(false, nullptr);
	if (Owner && Owner->HasAuthority())
	{
		InteractionSphere->SetCollisionEnabled(ECollisionEnabled::QueryAndPhysics);
		InteractionSphere->SetCollisionResponseToChannel(ECC_Pawn, ECR_Overlap);
		InteractionSphere->OnComponentBeginOverlap.AddDynamic(this, &UFSHInteractComponent::OnSphereBeginOverlap);
		InteractionSphere->OnComponentEndOverlap.AddDynamic(this, &UFSHInteractComponent::OnSphereEndOverlap);
		
	}
}

void UFSHInteractComponent::SetWidgetVisible(bool bIsVisible, AActor* Instigator)
{
	AActor* Owner = this->GetOwner();
	 if (Owner && InteractPromptWidget)
	 {
	 	InteractPromptWidget->SetVisibility(bIsVisible);
	 	if (auto* Character = Cast<AFSHCharacter>(Instigator))
	 	{
	 		switch (InteractObjectType)
	 		{
	 		case EInteractObjectType::EIOT_Weapon:
	 			{
	 				auto* Weapon = Cast<AFSHWeapon>(GetOwner());
	 				Character->PushReplicationOverlappingWeapon(Weapon);
	 				return;
	 			}
	 		case EInteractObjectType::EIOT_Custom:
	 			{
	 				return;
	 			}
	 		}
	 	}
	 }
}

void UFSHInteractComponent::SetCanInteract(bool bNewIsActive)
{
	bActive = bNewIsActive;
	//UFSHInteractComponent::SetWidgetVisible(bIsVisible, Instigator);
}

bool UFSHInteractComponent::GetCanInteract()
{
	return bActive;
}

void UFSHInteractComponent::Trigger(AActor* Instigator)
{
	OnTriggered.Broadcast(Instigator);
	Triggered(Instigator);
}

void UFSHInteractComponent::Triggered_Implementation(AActor* Instigator)
{
	
}


void UFSHInteractComponent::OnSphereBeginOverlap(UPrimitiveComponent* OverlapComponent, AActor* OtherActor,
                                                 UPrimitiveComponent* OtherComp, int32 OtherBoxIndex, bool bFromSweep, const FHitResult& SweepResult)
{
	AActor* Owner = this->GetOwner();
	AFSHCharacter* FSHCharacter = Cast<AFSHCharacter>(OtherActor);
	if (Owner && FSHCharacter)
	{
		if(UFSHInteractOption* InteractionOption = Cast<UFSHInteractOption>(InteractPromptWidget->GetUserWidgetObject()))
		{
			InteractionOption->ActionTextBlock->SetText(FText::FromString(DisplayText));
			// Push
			FSHCharacter->InteractableComponentsNearby.Emplace(this);
		}
	}
}

void UFSHInteractComponent::OnSphereEndOverlap(UPrimitiveComponent* OverlapComponent, AActor* OtherActor,
											UPrimitiveComponent* OtherComp, int32 OtherBoxIndex)
{
	AActor* Owner = this->GetOwner();
	AFSHCharacter* FSHCharacter = Cast<AFSHCharacter>(OtherActor);
	if (Owner && FSHCharacter)
	{
		//if(GEngine) { 
		//	GEngine->AddOnScreenDebugMessage(-1, 15.0f, FColor::Red, TEXT("END 1"));
		//}
		if(UFSHInteractOption* InteractionOption = Cast<UFSHInteractOption>(InteractPromptWidget->GetUserWidgetObject()))
		{
			//if(GEngine) { 
			//	GEngine->AddOnScreenDebugMessage(-1, 15.0f, FColor::Red, TEXT("END 2"));
			//}
			const int32 ItemIndex = FSHCharacter->InteractableComponentsNearby.IndexOfByKey(this);
			FSHCharacter->InteractableComponentsNearby.RemoveAt(ItemIndex);
			SetWidgetVisible(false, nullptr);
		}
	}
}

// Called every frame
void UFSHInteractComponent::TickComponent(float DeltaTime, ELevelTick TickType,
                                          FActorComponentTickFunction* ThisTickFunction)
{
	Super::TickComponent(DeltaTime, TickType, ThisTickFunction);
 
}


```


### Demonstration
[![](https://img.youtube.com/vi/-AwoyUF-20A/0.jpg)](https://www.youtube.com/watch?v=-AwoyUF-20A)
[![](https://img.youtube.com/vi/2Vq1pN7qJkw/0.jpg)](https://www.youtube.com/watch?v=2Vq1pN7qJkw)

### Features
- Interact Prompt
- Crouch
- Environment Obstacles
- Wandering AI
- Equip Weapon
- Aim Weapon
- Lobby Networking

## Unreal VR Template C++ Conversion (WIP) - C++
![](https://i.ibb.co/KpKh81D7/Screenshot-59.png)

SOURCE: https://github.com/TechProgramming/VRProjectExample

### Video
[![](https://img.youtube.com/vi/26paAkoAD-E/0.jpg)](https://youtu.be/26paAkoAD-E)

### Changes
- Converted Grab Component to C++
- Made a new Pistol that supports the C++ Grab Component
- Added Locomotion and Smooth Turn
- Added Pullable Lever

### Excerpt - Lever
Source: https://github.com/TechProgramming/VRProjectExample/blob/master/Source/VRProjectExample/VRPLever.cpp
```cpp


#include "VRPLever.h"
#include <Kismet/KismetMathLibrary.h>

// Sets default values
AVRPLever::AVRPLever()
{
 	// Set this actor to call Tick() every frame.  You can turn this off to improve performance if you don't need it.
	PrimaryActorTick.bCanEverTick = true;
	Scene = CreateDefaultSubobject<USceneComponent>(TEXT("Scene"));
	Lever = CreateDefaultSubobject<UStaticMeshComponent>(TEXT("Lever"));
	GrabComponent = CreateDefaultSubobject<UVRPGrabComponent>(TEXT("GrabComponent"));

	RootComponent = Scene;
	Lever->SetupAttachment(Scene);
	GrabComponent->SetupAttachment(Lever);
}

// Called when the game starts or when spawned
void AVRPLever::BeginPlay()
{
	Super::BeginPlay();

	GrabComponent->OnGrabbedDelegate.AddDynamic(this, &ThisClass::OnGrabbed);
	GrabComponent->OnDroppedDelegate.AddDynamic(this, &ThisClass::OnDropped);
}

// Called every frame
void AVRPLever::Tick(float DeltaTime)
{
	Super::Tick(DeltaTime);

}

void AVRPLever::UpdateRotation()
{
    if (!MotionControllerRef || !bIsGrabbed) {
        return;
    }


    FVector ControllerWorldPos = MotionControllerRef->GetComponentLocation();
    FVector LeverPivotPos = Scene->GetComponentLocation();
    FVector ControllerLocalPos = Scene->GetComponentTransform().InverseTransformPosition(ControllerWorldPos);
    FVector MovementDelta = ControllerLocalPos - InitialGrabOffset;
    float RotationDelta = -MovementDelta.X * RotationSensitivity;

    float NewPitch = FMath::Clamp(
        InitialLeverRotation.Pitch + RotationDelta,
        -70.0f,
        70.0f
    );

    FRotator NewRotation = FRotator(
        NewPitch,
        InitialLeverRotation.Yaw,
        InitialLeverRotation.Roll
    );

    Lever->SetRelativeRotation(NewRotation);
}

void AVRPLever::OnGrabbed(UMotionControllerComponent* MotionController)
{
	MotionControllerRef = MotionController;
	GetWorldTimerManager().SetTimer(UpdateRotationTimerHandle, this, &ThisClass::UpdateRotation, 0.01f, true);

	bIsGrabbed = true;
	InitialLeverRotation = Lever->GetRelativeRotation();
	FVector ControllerWorldPos = MotionController->GetComponentLocation();
	InitialGrabOffset = Scene->GetComponentTransform().InverseTransformPosition(ControllerWorldPos);
}

void AVRPLever::OnDropped()
{
	GetWorldTimerManager().ClearTimer(UpdateRotationTimerHandle);
	MotionControllerRef = nullptr;
	bIsGrabbed = false;
}
```

## Necromancer's Tomb - Blueprint
![](https://i.ibb.co/mrjnqDzW/Screenshot-57.png)

SOURCE: https://github.com/TechProgramming/NecromancersTomb

WEBSITE: https://clanlabs.co/

### About
- A project to test AI, Obstacles, Hiding Mechanic, and Unlocking Doors

### Features
- Patrolling AI
- Stealth Hide in Corners
- Floor Trap with Delay
- Collectible
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

### Demonstration
[![](https://img.youtube.com/vi/DpmLirBJx6A/0.jpg)](https://www.youtube.com/watch?v=DpmLirBJx6A)

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

# Demonstrations (UE)

## Unreal Engine Networking
[![](https://img.youtube.com/vi/2Vq1pN7qJkw/0.jpg)](https://www.youtube.com/watch?v=2Vq1pN7qJkw)

## Cutscene Test + Dialogue
[![](https://img.youtube.com/vi/6KJwC-tAlX0/0.jpg)](https://www.youtube.com/watch?v=6KJwC-tAlX0)

## Interaction System + Dialogue
[![](https://img.youtube.com/vi/EgUR2pNJinc/0.jpg)](https://www.youtube.com/watch?v=EgUR2pNJinc)

## Architecture Camera and Floor Change + Control Time
[![](https://img.youtube.com/vi/VwO04t9ggW4/0.jpg)](https://www.youtube.com/watch?v=VwO04t9ggW4)

## Pickup Object + Lean + Camera Bob + Camera Zoon + Sprint and Stamina + Breathing Change with Stamina + Hold Breath and Hear Heartbeat + Flashlight
[![](https://img.youtube.com/vi/hLULw2bLDUQ/0.jpg)](https://www.youtube.com/watch?v=hLULw2bLDUQ)
