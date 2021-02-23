战场人物管理demo

MapManager.lua
```lua
--战场人物管理demo
MapManager = class("MapManager")
local Grid = import("..Node.Grid")
--y
MapManager.rowMax = 0
--x
MapManager.colMax = 0
MapManager.map = {}
MapManager.allType = {"enemy", "friend"}
MapManager.bk_ = nil

function MapManager:ctor()
    self.bk_ = nil
    --y
    self.rowMax = 0
    --x
    self.colMax = 0
    self.map = {}
    self.allType = {"enemy", "friend"}
    self:Resize(3, 20)
end

function MapManager:Point2Index(point)
    --print(point.x, point.y)
    local index = {x = math.floor(point.x/40), y = math.floor(point.y/40)}
    --index.x = math.floor(point.x/30)
    --index.y = math.floor(point.y/30)
    
    return index
end


function MapManager:Index2Point(index)
    local point
    point.x = index.x*40+15
    point.y = index.y*40+15
    
    return point
end

function MapManager:setBk(bk)
    self.bk_ = bk
end

function MapManager:Resize(row, col)
    print("MapManager:Resize")
    self.map = {}
    self.colMax = col
    self.rowMax = row
    --print(self.map)
    for r = 0, row do
	    self.map[r] = {}
        --print(self.map[r])
	    for c = 0, col do
            self.map[r][c] = {}
            --print(self.map[r][c])
            self.map[r][c].data = {}
            --print(self.map[r][c].data)
            self.map[r][c].index = {x=c, y=r}
            for var, k in ipairs(self.allType) do
                --print(var, k) 
            	self.map[r][c].data[k] = {}
                --print(self.map[r][c].data[k])
            end
        end
    end
    
end

function MapManager:AddNode(node, type, point)
    print("MapManager:AddNode x", point.x, "y", point.y, "type", type)
	local index = self:Point2Index(point)
    --print(index.x, index.y)
    --print(self.map[0][0])
    print(self.map[index.y][index.x])
	print(self.map[index.y][index.x].data)
	print(self.map[index.y][index.x].data[type])
    print("MapManager:AddNode x", index.x, "y", index.y, "type", type)
	table.insert(self.map[index.y][index.x].data[type], node)
	node:SetIndex(index)
end

function MapManager:FindMinn2MaxEnemy(type, row, min, max, dir)
    --print("MapManager:FindMinn2MaxEnemy", type, min, max, dir)
    if "enemy" == type then 
        return self:FindMinn2Max("friend", row, min,max,dir)
    else
        return self:FindMinn2Max("enemy", row, min,max,dir)
    end
end

function MapManager:FindMinn2Max(type, row, min, max, dir)
    --print("MapManager:FindMinn2Max", type, row, min, max, dir)
    --self:PrintAllUser()
    
    for c = min, max, dir do 
        for r = 0, self.rowMax do
        --local r = row
            for key, var in ipairs(self.map[r][c].data[type]) do
                --print("MapManager:FindMinn2Max k" ,key, type, min, max, dir)
                if false == var:Death() then
                    return self.map[r][c].data[type][key]
                end
            end
        end  
    end
    
    return nil
end

function MapManager:Update(dt)
    
    dt = 0.017
    local x = 0
    --print("MapManager:Update--------------------------------------")
    for r = 0, self.rowMax do
        for c = 0, self.colMax do 
            for k, type in ipairs(self.allType) do
                --print("type", type, "k", k)
                --print(self.map[r][c].data[type])
                for key, var in ipairs(self.map[r][c].data[type]) do
                    --print("MapManager:Update",r, c, type, key, var, var:GetType())
                    --self.map[r][c].data[type][key]:Update(dt)
                    var:Update(dt)
                    if type == "friend" then
                        if x < var:getPositionX() then
                            x = var:getPositionX()
                        end
                    end
                    --local point = var:getPosition()
                    --print("var:point",point)
                    if var:DeleteFlag() then 
                        self.map[r][c].data[type][key]:removeFromParentAndCleanup()
                        self.map[r][c].data[type][key] = nil
                    elseif self:MoveNode(var, type) then
                        self.map[r][c].data[type][key] = nil
                    end
                end  
            end
        end  
    end
    
    self.bk_:Update(x)
end

function MapManager:MoveNode(node, type)
    --print(node, type)
    local newIndex = self:Point2Index(cc.p(node:getPositionX(), node:getPositionY()))
    local oldIndex = node:GetIndex()
    if newIndex.x == oldIndex.x and newIndex.y == oldIndex.y then
        --print(newIndex.x == oldIndex.x)
        --print(newIndex.y == oldIndex.y)
        --print("MapManager:MoveNode", newIndex.x, newIndex.y, "old",oldIndex.x, oldIndex.y)
    	return false
    end
    print("oldindex", oldIndex.x, oldIndex.y ,"newindex", newIndex.x, newIndex.y, type)
    node:SetIndex(newIndex)
    table.insert(self.map[newIndex.y][newIndex.x].data[type], node)
    --print("MapManager:MoveNode true")
    return true
end

function MapManager:PrintAllUser()
    print("MapManager:PrintAllUser")
    for r = 0, self.rowMax do
        for c = 0, self.colMax do 
            for k, type in ipairs(self.allType) do
                for key, var in ipairs(self.map[r][c].data[type]) do
                    print("row", r, "col", c, "type", type, key, var)
                end  
            end
        end  
    end
    print("MapManager:PrintAllUser end")
end

function MapManager:Arrange()
    
end

function MapManager:OnMessage(msg)
    print("MapManager:OnMessage")
    for r = 0, self.rowMax do
        for c = 0, self.colMax do 
            for k, type in ipairs(self.allType) do
                for key, var in ipairs(self.map[r][c].data[type]) do
                    if false == var:Death()then
                    	var:OnMessage(msg)
                    end
                end  
            end
        end  
    end 
end

return MapManager
```