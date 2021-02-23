lua队列

queue.lua

```lua
--[[
2014-10-17 08:34:30
队列
]]--

local queue = class("queue")

queue.first = 1
queue.last = 1
queue.list = {}

function queue:ctor()
    self.first = 1
    self.last = 1
    self.list = {}
end

function queue:pop()
    self.list[self.first] = nil
    self.first = self.first + 1
end

function queue:top()
    return self.list[self.first]
end

function queue:empty()
    if self.first < self.last then  
        return false  
    else 
        self.first = 1  
        self.last = 1 
        return true  
    end
end

function queue:puch(tmp)
    self.list[self.last] = tmp
    self.last = self.last + 1 
end

function queue:size()
    if self:empty() then 
        return 0
    else
        return self.last - self.first
    end
end

return queue

```