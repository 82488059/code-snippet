Lua单例


Singleton1.lua

```lua
--单例1
local Singleton = class("Singleton")

Singleton.type_ = "Singleton"

function Singleton:GetInstance()
    local o = _G.Singleton
    print("old")
    if o then return o end
    
    o = {}
    _G.Singleton = o
    setmetatable(o, self)
    self.__index = self
    print("new")
    return o
end

return Singleton
--使用
--[[
    local Singleton = import("..Node.Singleton")
    s = Singleton:GetInstance()
    print(s.type_)
]]--
```

Singleton2.lua
```lua
--单例2
local Singleton2 = class("Singleton2")  

--local scheduler = require("framework.scheduler")

Singleton2.time_ = 0

function Singleton2:ctor()
    print("Singleton2")
    self.time_ = 1
    
    --scheduler.scheduleGlobal(Singleton2.UpdateManager_, 1)   
end

function Singleton2:UpdateManager_(dt)
    print(dt)
    self.time_ = self.time_ + 1
    print(self.time_)    
end

return Singleton2
--[[
    local message = require("scripts.app.Node.Singleton2")
    --message也是单例 lua方式
    message:UpdateManager_(dt)
]]--
```