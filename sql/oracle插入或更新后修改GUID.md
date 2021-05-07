oracle插入或更新后修改GUID


trigger.sql

```sql
create or replace trigger _trigger_name
  before insert or update on TABLE_NAME
  for each row
declare
  -- local variables here
begin
  :new.ID := SYS_GUID;
end _trigger_name;

-- 插入或更新数据后修改ID
```
