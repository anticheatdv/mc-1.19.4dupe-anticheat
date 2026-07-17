mc-1.19.4dupe-anticheat
This plugin patches a critical item duplication glitch utilizing Allays in Minecraft version 1.19.4. The exploit takes advantage of vulnerabilities in the server tick processing order and entity hitbox logic. Below is a detailed technical breakdown of how the glitch operates and how this plugin prevents it.

Technical Breakdown of the Glitch
1. The 0.5 Coordinate and Hitbox Intersection
The fundamental prerequisite for this exploit is locking the Allay at the exact center of a block at the 0.5 coordinate. When positioned here, the item pickup radius of the Allay perfectly overlaps with the suction radius of a Hopper placed directly beneath it.

2. Precise Dispenser Targeting
A Dispenser shoots items directly into the intersection point of both detection ranges. This creates a physical environment where the dropped item entity falls into the collection range of both entities simultaneously.

3. Simultaneous Pickup Events Within a Single Tick
The core conflict occurs within the Minecraft server tick system. On the exact tick the dispensed item becomes active, the entity logic for the Allay attempting to pick up the item and the block entity logic for the Hopper attempting to suck it in are executed concurrently.

4. Item Multiplication via Entity Deletion Delay
Normally, once an item is collected, the original dropped item entity should be removed from the world instantly. However, when the Allay and the Hopper successfully register a pickup event at the exact same timing, the server issues the item reward command to both inventories before processing the deletion of the original item. As a result, one item goes to the Allay and another goes to the Hopper slot, effectively doubling the item.

Prevention Mechanism
This plugin detects multiple pickup events occurring on the same item entity within a single tick. When a Hopper and an Allay attempt to collect the item simultaneously, the plugin forces one event to cancel or immediately invalidates the item object state the moment it is collected by either side. This fundamentally blocks the possibility of both entities taking the item.
