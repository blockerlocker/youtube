These commands allow you to feed Sticks to Creepers, which allows them to Mix with each other and combine into larger Creepers, as seen in [this video](https://youtube.com/shorts/L7qV4O1skkk). Made in Minecraft 26.3. If you have any issues, feel free to join my Discord server: https://discord.blocker.locker

First, run these setup commands in chat to create some scoreboard objectives.

```
scoreboard objectives add mix_creeper_count dummy
```
```
scoreboard objectives add mix_creeper_time dummy
```

Next, paste these commands into a Repeating Command Block pointing into 10 Chain Command Blocks (11 total) all with their arrows pointing to the next Command Block. They must all be either powered by Redstone or set to "Always Active" instead of "Needs Redstone".

```
execute as @e[type=creeper] unless score @s mix_creeper_count matches 1.. run scoreboard players set @s mix_creeper_count 1
```
```
item modify entity @a weapon.* {type:filtered,item_filter:{items:stick},on_pass:{type:set_components,components:{equippable:{slot:saddle,equip_sound:"entity.player.burp",allowed_entities:creeper,dispensable:0,equip_on_interact:1}}}}
```
```
execute as @e[type=creeper] unless score @s mix_creeper_time matches 1.. if items entity @s saddle stick run scoreboard players set @s mix_creeper_time 800
```
```
item replace entity @e[type=creeper,scores={mix_creeper_time=1..}] saddle with air
```
```
scoreboard players remove @e[type=creeper,scores={mix_creeper_time=1..}] mix_creeper_time 1
```
```
execute as @e[type=creeper,scores={mix_creeper_time=1..}] if predicate {type:entity_properties,entity:this,predicate:{periodic_tick:3}} at @s anchored eyes run particle heart ^ ^ ^ 0.3 0.2 0.3 0 1
```
```
execute as @e[type=creeper,scores={mix_creeper_time=1..}] at @s store success entity @s data.mix_creeper byte 1 if score @s mix_creeper_time < @n[type=creeper,scores={mix_creeper_time=1..},distance=0.01..3] mix_creeper_time run tag @n[type=creeper,scores={mix_creeper_time=1..},distance=0.01..3] add mix_creeper_absorb
```
```
execute as @e[type=creeper,nbt={data:{mix_creeper:true}}] store result entity @s attributes[{id:scale}].base float 1 store result entity @s ExplosionRadius byte 3 run scoreboard players operation @s mix_creeper_count += @n[type=creeper,tag=mix_creeper_absorb] mix_creeper_count
```
```
scoreboard players reset @e[type=creeper,nbt={data:{mix_creeper:true}}] mix_creeper_time
```
```
execute as @e[type=creeper,tag=mix_creeper_absorb] run data merge entity @s {Health:0,DeathTime:20}
```
```
execute as @e[type=creeper,nbt={data:{mix_creeper:true}}] run data remove entity @s data.mix_creeper```