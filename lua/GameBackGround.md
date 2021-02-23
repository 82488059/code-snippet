lua实现的循环背景 

`GameBackGround.lua` 

```lua
--2014-10-21 15:12:17
local GameBackGround = class("GameBackGround", function()
    return display.newLayer("GameBackGround")
end)

GameBackGround.bk_ = {}
GameBackGround.bkSize_ = 0

function GameBackGround:ctor()
    self.bk_[0] = display.newSprite("main_bg_grass_left.jpg" , display.cx, display.cy)
    self.bk_[0]:setAnchorPoint(0,0)
    self.bk_[0]:setPosition(0,0)
    self:addChild(self.bk_[0])   
    self.bk_[1] = display.newSprite("main_bg_grass_right.jpg" , display.cx, display.cy)
    self.bk_[1]:setAnchorPoint(0,0)
    self.bk_[1]:setPosition(display.cx*2,0)
    self:addChild(self.bk_[1])   
    
    self.bkSize_ = 2 -- 背景数量
end

function GameBackGround:Update(nx)
    -- 人物的固定位置
    local tx = display.cx/2 
    if nx > tx then
        -- 移动背景层
        self:setPositionX(display.cx/2-nx)
        -- 当前移动过的背景图片数
        num = math.floor(nx/display.cx/2)
        -- 修正
        xx = nx - num*display.cx*2
        bk = {}     
        --   
        for var=0, self.bkSize_ do
            bk[var] = self.bk_[math.mod(num + var, 2)]
        end
        -- 人物移动到tx时移动左边的背景到最右
        if xx > tx then 
            for var=0, self.bkSize_-1 do
            print("var", var)
                bk[var]:setPositionX((num+var)*display.cx*2)
            end 
        end
    end    
end


return GameBackGround
```