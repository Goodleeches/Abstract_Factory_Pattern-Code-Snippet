# **Abstract Factory Pattern - C++ Code**

## **🔹 BlockCreator.h**
```cpp
#pragma once
#include "../Header/Defines.h"
USING_NS_CC;
USING_NS_STD;

class BaseBlock;
class BlockFactory;

class BlockCreator
{
    _DECLARE_SINGLETON(BlockCreator)

private:
    explicit BlockCreator();
    virtual ~BlockCreator() = default;

public:
    bool init();
    BaseBlock* createBlock(TileTable playTable, const UserBlockInfo& userBlockInfo, Vec2 vArrIndex);

private:
    unordered_map<string, BlockFactory*> m_mapBlockFactory;
};

#define g_BLOCK_CREATOR BlockCreator::GetInstance()
