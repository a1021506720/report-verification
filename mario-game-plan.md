# 玛丽奥小游戏开发规划文档

## 项目概述

### 项目名称
经典玛丽奥冒险游戏 (Classic Mario Adventure)

### 项目目标
开发一款基于Web技术的经典2D横版跳跃游戏，重现经典玛丽奥游戏的核心玩法体验。

### 目标用户
- 喜爱经典游戏的玩家
- Web游戏爱好者
- 移动端休闲游戏用户

## 游戏设计

### 核心玩法机制

#### 1. 角色控制
- **移动系统**: 左右移动、跳跃、下蹲
- **物理引擎**: 重力、碰撞检测、摩擦力
- **状态管理**: 正常状态、无敌状态、死亡状态

```javascript
// 玩家状态枚举
const PlayerState = {
  NORMAL: 'normal',      // 正常状态
  INVINCIBLE: 'invincible', // 无敌状态
  DEAD: 'dead'           // 死亡状态
};

// 玩家控制器示例
class PlayerController {
  constructor() {
    this.velocity = { x: 0, y: 0 };
    this.position = { x: 0, y: 0 };
    this.state = PlayerState.NORMAL;
    this.jumpPower = 15;     // 跳跃力度
    this.moveSpeed = 5;      // 移动速度
    this.gravity = 0.8;      // 重力加速度
  }
}
```

#### 2. 关卡设计
- **地形系统**: 平台、管道、隐藏区域
- **道具系统**: 金币、蘑菇、火花
- **敌人系统**: 板栗仔、乌龟、食人花

#### 3. 计分系统
- **得分机制**: 消灭敌人、收集金币、完成关卡
- **生命系统**: 3条生命，可通过道具获得额外生命
- **时间系统**: 关卡倒计时，增加紧迫感

### 游戏功能模块

#### 1. 核心游戏循环 (Game Loop)
```javascript
// 游戏主循环示例
class GameEngine {
  constructor() {
    this.isRunning = false;
    this.lastTime = 0;
    this.deltaTime = 0;
  }
  
  // 游戏循环
  gameLoop(currentTime) {
    this.deltaTime = currentTime - this.lastTime;
    this.lastTime = currentTime;
    
    this.update(this.deltaTime);  // 更新游戏逻辑
    this.render();                // 渲染画面
    
    if (this.isRunning) {
      requestAnimationFrame((time) => this.gameLoop(time));
    }
  }
}
```

#### 2. 场景管理 (Scene Management)
- **主菜单场景**: 开始游戏、设置、排行榜
- **游戏场景**: 核心游戏玩法
- **暂停场景**: 游戏暂停界面
- **结束场景**: 游戏结束、重新开始

#### 3. 资源管理 (Asset Management)
- **图片资源**: 角色精灵、背景、道具图标
- **音频资源**: 背景音乐、音效
- **关卡数据**: JSON格式的关卡配置

## 技术架构

### 技术选型

#### 前端技术栈
- **核心框架**: HTML5 Canvas + Vanilla JavaScript
- **构建工具**: Webpack / Vite
- **代码规范**: ESLint + Prettier
- **版本控制**: Git

#### 推荐替代方案
- **游戏引擎**: Phaser.js (更完整的2D游戏开发框架)
- **TypeScript**: 提供类型安全和更好的开发体验
- **状态管理**: 简单的状态机或Redux-like模式

### 系统架构设计

#### 1. 模块划分 (遵循SOLID原则)

```javascript
// 单一职责原则 - 每个类只负责一个功能
class InputManager {
  // 专门处理用户输入
}

class PhysicsEngine {
  // 专门处理物理计算
}

class RenderEngine {
  // 专门处理渲染
}

class AudioManager {
  // 专门处理音频
}

// 开闭原则 - 便于扩展新的游戏对象类型
abstract class GameObject {
  abstract update(deltaTime: number): void;
  abstract render(context: CanvasRenderingContext2D): void;
}

class Player extends GameObject {
  // 玩家具体实现
}

class Enemy extends GameObject {
  // 敌人具体实现
}
```

#### 2. 数据流设计

```
用户输入 → 输入管理器 → 游戏状态更新 → 物理引擎计算 → 渲染引擎绘制
    ↓
  音频管理器 ← 游戏事件系统 ← 碰撞检测系统
```

#### 3. 文件结构
```
mario-game/
├── src/
│   ├── core/           # 核心引擎代码
│   │   ├── Game.js     # 主游戏类
│   │   ├── Scene.js    # 场景基类
│   │   └── GameObject.js # 游戏对象基类
│   ├── managers/       # 各种管理器
│   │   ├── InputManager.js
│   │   ├── AudioManager.js
│   │   └── AssetManager.js
│   ├── entities/       # 游戏实体
│   │   ├── Player.js
│   │   ├── Enemy.js
│   │   └── Platform.js
│   ├── scenes/         # 游戏场景
│   │   ├── MenuScene.js
│   │   ├── GameScene.js
│   │   └── PauseScene.js
│   └── utils/          # 工具函数
│       ├── Vector2.js
│       └── Constants.js
├── assets/             # 游戏资源
│   ├── images/
│   ├── audio/
│   └── levels/
├── dist/              # 构建输出
└── docs/              # 项目文档
```

## 开发计划

### 阶段一: 基础框架搭建 (1-2周)
- [ ] 项目环境搭建
- [ ] 基础游戏引擎开发
- [ ] 输入系统实现
- [ ] 基础渲染系统

### 阶段二: 核心玩法实现 (2-3周)
- [ ] 玩家角色实现
- [ ] 物理引擎开发
- [ ] 碰撞检测系统
- [ ] 基础关卡系统

### 阶段三: 游戏内容丰富 (2-3周)
- [ ] 敌人AI系统
- [ ] 道具系统实现
- [ ] 音效和背景音乐
- [ ] 多关卡设计

### 阶段四: 优化和完善 (1-2周)
- [ ] 性能优化
- [ ] 用户界面完善
- [ ] 移动端适配
- [ ] 测试和调试

## 用户界面设计

### 主要界面

#### 1. 主菜单界面
- 游戏标题和Logo
- 开始游戏按钮
- 设置选项
- 最高分显示

#### 2. 游戏界面
- 游戏画面区域
- 分数显示
- 生命值显示
- 时间倒计时
- 暂停按钮

#### 3. 设置界面
- 音量控制
- 控制键位设置
- 画质选项

### 响应式设计
- 支持桌面端（键盘控制）
- 支持移动端（触摸控制）
- 自适应不同屏幕尺寸

## 测试策略

### 功能测试
- 玩家控制响应性测试
- 碰撞检测准确性测试
- 游戏逻辑正确性测试
- 关卡完成度测试

### 性能测试
- 帧率稳定性测试
- 内存使用量监控
- 资源加载时间测试
- 移动端兼容性测试

### 用户体验测试
- 操作流畅度评估
- 游戏难度平衡性
- 界面友好性测试
- 音效协调性测试

## 部署方案

### 静态网站部署
- **GitHub Pages**: 免费托管，适合开源项目
- **Netlify**: 自动构建和部署
- **Vercel**: 优秀的前端部署平台

### CDN优化
- 图片资源压缩和优化
- 代码压缩和混淆
- 启用Gzip压缩
- 设置合理的缓存策略

### 移动端发布
- PWA (Progressive Web App) 支持
- 应用商店发布（使用Cordova/PhoneGap包装）

## 技术风险评估

### 主要风险点
1. **性能问题**: Canvas渲染在低端设备上的性能表现
2. **兼容性**: 不同浏览器的兼容性问题
3. **资源管理**: 大量图片和音频资源的加载优化
4. **移动端体验**: 触摸操作的准确性和响应性

### 风险缓解策略
1. **性能优化**: 
   - 使用对象池模式减少GC压力
   - 实施LOD (Level of Detail) 系统
   - 合理使用requestAnimationFrame

2. **兼容性保证**:
   - 渐进式增强设计
   - 详细的浏览器兼容性测试
   - 提供fallback方案

3. **资源优化**:
   - 图片雪碧图技术
   - 音频压缩和格式优化
   - 分块加载策略

## 扩展计划

### 短期扩展 (1-3个月)
- 增加更多关卡
- 实现多人本地对战模式
- 添加成就系统
- 支持自定义关卡编辑器

### 长期扩展 (3-12个月)
- 在线多人对战
- 社交功能集成
- 手柄支持
- VR模式探索

## 总结

本规划文档遵循软件工程的最佳实践，采用模块化设计和渐进式开发方式。通过合理的架构设计和技术选型，确保项目的可维护性和可扩展性。同时，详细的测试策略和风险评估有助于保证项目质量和按时交付。

参考文档：
- [HTML5 Canvas API](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API)
- [Web Audio API](https://developer.mozilla.org/en-US/docs/Web/API/Web_Audio_API)
- [Phaser 3 游戏引擎](https://phaser.io/phaser3)
- [游戏开发模式](https://gameprogrammingpatterns.com/) 