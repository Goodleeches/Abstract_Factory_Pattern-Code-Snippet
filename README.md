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
    unordered_map<string, BlockFactory*> m_mapBlockFactory;
};

#define g_BLOCK_CREATOR BlockCreator::GetInstance()


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
    m_mapBlockFactory.insert(make_pair("SpawnerBlockFactory", SpawnerBlockFactory::create()));
    m_mapBlockFactory.insert(make_pair("MaterialBlockFactory", MaterialBlockFactory::create()));
    m_mapBlockFactory.insert(make_pair("RewardBlockFactory", RewardBlockFactory::create()));
    m_mapBlockFactory.insert(make_pair("EventBlockFactory", EventBlockFactory::create()));
    m_mapBlockFactory.insert(make_pair("ItemBlockFactory", ItemBlockFactory::create()));

    return true;
}

BaseBlock* BlockCreator::createBlock(TileTable playTable, const UserBlockInfo& userBlockInfo, Vec2 vArrIndex)
{
    BlockFactory* pFactory = nullptr;

    switch (userBlockInfo.blockType.mainType)
    {
        case 1:
        case 3:
        case 7:
        {
            auto iter = m_mapBlockFactory.find("SpawnerBlockFactory");
            if (iter == m_mapBlockFactory.end())
            {
                log("Can't Find SpawnerBlockFactory");
                return nullptr;
            }
            pFactory = iter->second;
        }
        break;

        case 2:
        case 8:
        {
            auto iter = m_mapBlockFactory.find("MaterialBlockFactory");
            if (iter == m_mapBlockFactory.end())
            {
                log("Can't Find MaterialBlockFactory");
                return nullptr;
            }
            pFactory = iter->second;
        }
        break;

        case 4:
        {
            auto iter = m_mapBlockFactory.find("RewardBlockFactory");
            if (iter == m_mapBlockFactory.end())
            {
                log("Can't Find RewardBlockFactory");
                return nullptr;
            }
            pFactory = iter->second;
        }
        break;

        case 5:
        {
            auto iter = m_mapBlockFactory.find("EventBlockFactory");
            if (iter == m_mapBlockFactory.end())
            {
                log("Can't Find EventBlockFactory");
                return nullptr;
            }
            pFactory = iter->second;
        }
        break;

        case 6:
        {
            auto iter = m_mapBlockFactory.find("ItemBlockFactory");
            if (iter == m_mapBlockFactory.end())
            {
                log("Can't Find ItemBlockFactory");
                return nullptr;
            }
            pFactory = iter->second;
        }
        break;

        default:
            return nullptr;
    }

    if (pFactory == nullptr)
        return nullptr;

    return pFactory->createBlock(playTable, userBlockInfo, vArrIndex);
}

