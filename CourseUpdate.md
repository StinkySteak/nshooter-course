# Udemy Course Update

## Ep 8. Deprecation MatchManager OnSceneLoaded
From Netick `0.11.35` and onwards, `OnSceneLoaded` and `OnSceneLoadStarted` has been marked obsolete. However they still works perfectly fine. 

There is no a perfect replacement for now, which are `OnSceneOperationBegan` and `OnSceneOperationDone`. Because this callbacks won't get called if we are enter play mode inside the game scene directly.
You can ignore the warning message and continue to the next episode. I will notify and update this readme if there's a better replacement.


## Ep 16. Player Manager - Fix Racing Condition
If you are using Netick 0.11.16 or later, you don't really have to follow this episode. From Netick 0.11.16, It introduce the concept of `[ExecutionOrder]` Which allow developer to customize their `NetworkStart()` call order for both client and server.
We can fix this to solve racing issue where the `PlayerCharacter` `NetworkStart` is trying to get It's player session however the `PlayerSession` itself hasn't registered on `PlayerManager`.
Lower execution order means more priority.
### Player Session
```cs
[ExecutionOrder(-100)]
public class PlayerSession : NetworkBehaviour
{
      //...
}
```

### Player Character
```cs
[ExecutionOrder(-99)]
public class PlayerCharacter : NetworkBehaviour
{
      //...
}
```

Thus you don't have to modify your PlayerManager at all.

## Ep 30. Spawn & Respawning
From Netick `0.11.4` it now introduces the concept of Server and Host. Host is just a server with a playable character. While in server, you don't have any input source.

There are two different ways to upgrade your code

### 1 - OnPlayerConnected Callback (Recommended)
```cs
// Get called when host start the game or a client joined the game
public override void OnPlayerConnected(NetworkSandbox sandbox, NetworkPlayer player)
{
      SpawnPlayerSession(player);
}
```

### 2 - API Update
```cs
public override void OnSceneLoaded(NetworkSandbox sandbox)
{
     //...

    if (sandbox.IsHost) // Use IsHost instead of IsServer.
    {
        // ...
    }
}
```



