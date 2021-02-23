
quick加载骨骼动画

Skeleton.lua

```lua
local Skeleton = class("Skeleton", function()
    return display.newSprite()
end)


function Skeleton:ctor()
    -- 加载骨骼资源
    ccs.ArmatureDataManager:getInstance():addArmatureFileInfo("cyborg.png", "cyborg.plist", "cyborg.xml")
    -- addArmatureFileInfo("png名","plist名","xml名字") 
    
    -- 创建
    local armature3 = ccs.Armature:create("cyborg") -- 骨骼名
    armature3:getAnimation():play("run")  -- 动作名
    armature3:setPosition(display.cx, display.cy) 
    self:addChild(armature3)

end


return Skeleton
```