# **Abstract Factory Pattern - C++ Code**

## **BlockCreator.h**
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
    std::unordered_map<int, std::unique_ptr<BlockFactory>> m_mapBlockFactory;
};

#define g_BLOCK_CREATOR BlockCreator::GetInstance()
```

## **BlockCreator.cpp**
```cpp
#include "BlockCreator.h"
#include "SpawnerBlockFactory.h"
#include "MaterialBlockFactory.h"
#include "RewardBlockFactory.h"
#include "EventBlockFactory.h"
#include "ItemBlockFactory.h"

_IMPLEMENT_SINGLETON(BlockCreator)

BlockCreator::BlockCreator()
{
}

bool BlockCreator::init()
{
    m_mapBlockFactory[1] = std::make_unique<SpawnerBlockFactory>();
    m_mapBlockFactory[3] = std::make_unique<SpawnerBlockFactory>();
    m_mapBlockFactory[7] = std::make_unique<SpawnerBlockFactory>();
    m_mapBlockFactory[2] = std::make_unique<MaterialBlockFactory>();
    m_mapBlockFactory[8] = std::make_unique<MaterialBlockFactory>();
    m_mapBlockFactory[4] = std::make_unique<RewardBlockFactory>();
    m_mapBlockFactory[5] = std::make_unique<EventBlockFactory>();
    m_mapBlockFactory[6] = std::make_unique<ItemBlockFactory>();

    return true;
}

BaseBlock* BlockCreator::createBlock(TileTable playTable, const UserBlockInfo& userBlockInfo, Vec2 vArrIndex)
{
    auto iter = m_mapBlockFactory.find(userBlockInfo.blockType.mainType);
    if (iter == m_mapBlockFactory.end()) {
        log("Can't find factory for type %d", userBlockInfo.blockType.mainType);
        return nullptr;
    }
    return iter->second->createBlock(playTable, userBlockInfo, vArrIndex);
}

