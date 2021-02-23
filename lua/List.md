lua实现的list

list.lua

```lua
local List = class("List")
local Node = class("Node")
local ListIterator = class("ListIterator")

Node.data_ = nil
Node.prev_ = nil
Node.next_ = nil
Node.type_ = nil
function Node:New(data, prev, next)
    print("Node:New")
    local o = {}
    o.data_ = data
    o.prev_ = prev
    o.next_ = next
    o.type_ = "Node"
    setmetatable(o, self)
    self.__index = self
    return o
end

ListIterator.pos_ = nil
ListIterator.type_ = nil
function ListIterator:New(a) 
    print("ListIterator:New")
    print("a.type" , a.type_)
    local o = {}
    o.type_ = "ListIterator"
    
    if a.type_ == "List" then 
        o.pos_ = a.head_.next_
    elseif a.type_ == "Node" then
        o.pos_ = a
    end
    setmetatable(o, self)
    self.__index = self
    
    return o
end

function ListIterator:IsEnd()
    print("ListIterator:IsEnd")
    return not self.pos_.data_
end
function ListIterator:Cur()
    print("ListIterator:Cur")
    return self.pos_
end
function ListIterator:Data()
    print("ListIterator:Data")
    return self.pos_.data_
end
function ListIterator:MoveNext()
    print("ListIterator:MoveNext")
    self.pos_ = self.pos_.next_
 return self
 end

function ListIterator:MovePrev()
    print("ListIterator:MovePrev")
    self.pos_ = self.pos_.prev_
    return self
end

--[[function List:ctor()
    print("List:ctor")
    self.head_ = Node:New()
    self.head_.prev_ = self.head_
    self.head_.next_ = self.head_    
end]]--

List.head_ = nil
List.type_ = nil

function List:New()
    print("List:New")
    local o = {}
    o.head_ = Node:New()
    --print("o.head", o.head_, o.head_.type_, o.head_.prev_, o.head_.next_, o.head_.data_)
    o.head_.prev_ = o.head_
    o.head_.next_ = o.head_
    
    -- give it a type def
    o.type_ = "List"
    setmetatable(o, self)
    self.__index = self
    return o
end

function List:CreateIterator()
    print("List:CreateIterator")
    --if self == nil then
    --    print("List:CreateIterator error")
    --end
    return ListIterator:New(self)
end

function List:Insert(it, data)
    print("List:Insert")
    --if self == nil then
    --    print("error")
    --end
    local iter = ListIterator:New(self)
    local node = Node:New(data, it.pos_.prev_, it.pos_)
    it.pos_.prev_.next_ = node
    it.pos_.prev_ = node
    --if node == nil then
    --    print("error")
    --end
    --print(node.data_, node.next_, node.prev_)
    return ListIterator:New(node)
end

function List:Begin()
    print("List:Begin")
    return self:CreateIterator()
end

function List:End()
    print("List:End")
    --if self == nil then
    --    print("End error")
    --end
    --print("head_", self.head_)
    return ListIterator:New(self.head_)
end
  
function List:PushFront(data)
    print("List:PushFront")
    self:Insert(self:Begin(), data)
end

function List:PushBack(data)
    print("List:PushBack")
    self:Insert(self:End(), data)
end

function List:IsEmpty()
    print("List:IsEmpty")
    return self:Begin().pos_ == self:End().pos_
end

function List:Erase(it)
    print("List:Erase")
    --assert(not it.data, "you can't erase the head")
    it.pos_.prev_.next = it.pos_.next_
    it.pos_.next_.prev = it.pos_.prev_
    it = nil
end

function List:PopFront()
    print("List:PopFront")
    --assert(not self:IsEmpty(), "Can't PopFront to a Empty list")
    self:Erase(self:Begin())
end

function List:PopBack()
    print("List:PopBack")
    --assert(not self:IsEmpty(), "Can't PopBack to a Empty list")
    self:Erase(self:End():MovePrev())
end

function List:Clear()
    print("List:Clear")
     while not self:IsEmpty() do
         self:Erase(self:Begin())
    end
end

return List
```