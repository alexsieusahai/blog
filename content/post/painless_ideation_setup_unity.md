---
author: Alex Sieusahai
title: The Virtues of Setup / Teardown for Scene Management In Unity
date: 2021-06-16
description: 
---

I'm currently developing an indie game, and it's my first time working with Unity.
I feel very comfortable working with Unity and C# now after 2-3 months of working with the tools around 8 hours a day, but there's still things I learn everyday.
There's something mildly painful and annoying about Unity's normal scene management, which is (to the best of my knowledge) not so much a fault of Unity, but moreso a fault of the way I designed things.
Moreover, there's a simple way to work around it.

# Problem
Most games need some kind of logic which is handled every frame, which handles a variety of things. For example, we could have an object which handles consuming keyboard and mouse inputs, another which handles night and day cycles, etc.

# Solution 1 (God)
There was (to the best of my knowledge this pattern has been phased out for obvious reasons) a common pattern known as God, which handles all of this logic all in one class.
The first problem I have with this solution is often touted; this violates the Single Responsibility Principle blatantly and as such many developers agree that this is a bad practice.
Additionally, this violates the principle of small classes.
Why, practically, does this matter?

The first, most obvious problem that I can see is handling of order registering when it comes to objects which would normally interact with one another.
Suppose we call function `foo`, we then call 10 other functions, then we call function `bar`. 
There is some strange behaviour.
We expect it to be coming from `foo`, but we aren't sure; how can we place the blame on `foo` when we also have to walk through 10 other functions' implementations to ensure that it's not caused by `bar`?
This is not so much a problem for a solo dev that's working on their work daily, but for teams of multiple people with improper review practice and dodgy code it could become a problem; tracking down seeminingly benign bugs can become a nightmare in this situation.

Another major problem is the overwriting of variables in a seemingly benign way, especially if you're doing async calls.
Variable names are often dependent on their context; `amount` in the context of a resource node (probably) means how much resource the node has remaining, whereas `amount` in the context of an inventory likely refers to the amount of a certain item.
This is something most people intuitively do, and in the context of a God class, it's very easy to overwrite variables you shouldn't be writing over; this is reminiscent of the common beginner pitfall of using too many global variables.

There's a myriad of other reasons why this pattern is probably not a good idea, which can be found snooping around on Google.

It's well worth noting, however, that we can (usually) refactor God classes into multiple smaller _Handlers_, all of which are responsible for maintaining (or _handling_) the game world.

# Solution 2 (Handlers)
Now, we alleviate the problem of the God class being too big, and we can allow all handlers to have a single responsibility (if they don't, just break them up into multiple handlers).
As far as I've found, this has been a very useful solution to the world maintenance problem; we can use these handlers to ensure that the game world functions as intended, and introducing more mechanics is as easy as defining a new handler.

There's a very annoying and troublesome problem within the way Unity does things, however.
These handlers are scene dependent; introducing a new handler to all scenes requires you to navigate to all scenes to add the handler.

This on the surface seems like kind of a pain, but we can steal a very useful design for transitioning between states while ensuring that some properties of the state are maintained.
Namely, we can use the `Setup` and `Teardown` design from unit testing frameworks, and we can use this during scene transition to maintain the state of whatever is desired during scene transitions, while _additionally_ handling the problem of scene handler introduction to all scenes.

There are a couple pain points with this, however.

# Why Bother?
The main pain point, by far, is consistency between all of your various scenes and tests. 
Namely, something will work with your tests that's consistent without some certain handler introduced, and introduction of your handler could break something.
I'd much rather break in the small unit tests instead of breaking somewhere in game that's much harder to reproduce and debug.

The other pain point is convenience; when you have dozens of these handlers lying around maintaining everything you need to, you really don't want to go through and introduce each of these prefabs into a new scene or test.

# Programmatic Loading of Prefabs Without _Any_ Editor Intervention
Let's go over the main way of loading a prefab object with the editor.
Note that the following code snippet is purely demonstrative and has not been formally tested; there might be some minor API call mistakes or things of that magnitude.
Let's take a Unity-speak example which should emulate the coin blocks from Super Mario Bros:

```
class CoinBlock : MonoBehaviour
{
    [SerializeField]
    GameObject coinPrefab, nullBlockPrefab;
    [SerializeField]
    Vector3 coinOffset;
    
    void OnPlayerUpwardCollision()
    {
        Destroy(this.gameObject);
        Instantiate(nullBlockPrefab, transform.position, transform.rotation);
        Instantiate(coinPrefab, transform.position + coinOffset, Quaternion.identity);
    }
}
```

The main thing we need to replace is the following:
```
[SerializeField]
GameObject coinPrefab, nullBlockPrefab;
```

Something we can do is load the `coinPrefab` and `nullBlockPrefab` from `Resources` programatically, using something along the lines of `Instantiate(Resources.Load<GameObject>("coinPrefab"))`.
The main problem I have with this is that there's all of these lingering string literals, but for now this seems to suffice.

Now, we can setup a setup function as following:
1. Define a `ResourceHandler` which defines `Setup` and `Teardown` functions.
2. Keep all of your prefabs under `Resources` in some way (my current preference is `Resources/Prefabs/<prefab_name>`).
3. Define your `Setup` function to load in all `Handler` type objects from `Prefabs/Handlers` (or whatever you so choose).
4. Remove all `Handler` objects from your scene, and just use this `Setup` object to instantiate all `Handler` objects.

# Handling of Awake

There's also a new problem in that any code in `Awake` that relies on any sort of information passing between the `Handler` and `Object` (such as subscription to a certain event where `UnityEvent`s won't suffice, introduction into some handler specific queue for updating or whatever) must now be refactored.

What we can do instead is just use `Invoke` a `UnityEvent` after handling the `Setup` which then runs the `Awake` code in invocation.

## Final Thoughts

This hopefully presents a very painless way of setting up all of your logic handlers that's consistent between scenes (and additionally tests). 
