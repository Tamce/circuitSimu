# MiCircuit (Project MCC)
 [GitHub](https://github.com/tamce/MiCircuit)  
 准备开一个写电子元件模拟的坑，基本原理跟 `Minecraft` 的红石电路类似，用于练习，随时弃坑预警。
 
## 想法
 * 整体由 1x1 的基本单元 `Unit` 组成，基本单元拥有基本功能如简单传输功能、信号增强功能、单项电流通过功能等。
 * ~~每个单元的功能由脚本描述，使用 `lua` ？ 这样就可以具有拓展性，`built-in` 的话也不是不可以，这样子新增元件的时候就需要重新编译。~~
    * 不引入脚本，先实现基本功能
 * ~~多个单元可以组成一个组件 `Component`，一个组件具有一定的形状（因为由多个单元组成），规定组件必须由连续的单元组成。~~
 * 一个组件可以由一个或多个单元和组件组成，组件具有形状（也可以看作矩形，没有元件的部分以空 `Null` 填满。
 * `BoardIterator` 类提供简单的方法在 `Board` 上进行移动
 * `Unit` 类用于表示基本单元，其应该包括：
    * 类型 - `unsigned int`
    * 位置、方向信息 - `tmc::mcc::Transform`
    * 状态信息
 * 每一个详细的基本单元应该从 `Unit` 类派生
    * 类型信息是所有派生于 `Unit` 类所共有的特性，故理所当然作为基类属性先（
 * Board 中的 Unit 线性存储。
 * `Unit` 状态相关：
    * 每个 `Unit` 存储其自身边界的 `电位` 信息，方位包括前后左右上下六个。
    * 两个 `Unit` 相交的边界处电位相同
    * 每个 `Unit` 的状态发生改变时触发与其相邻的其他 `Unit` 状态改变。
    * `Unit` 的状态改变即为其电位信息变化
 * 状态更新相关
    * 由主要控制类负责控制（模拟器主线程）
    * 维护两个队列，其中一个为当前 `tick` 更新状态的操作，另一个为下一 `tick` 更新状态的操作
    * 当前 `tick` 队列空的时候，交换两个队列（的指针），等待下一个 `tick` 的到来
    * 一个更新操作包括：
        * 要更新的 `Unit`
        * 触发更新的来源方向与电位信息
* 暂时不考虑直接通过指针/迭代器修改 `Unit` 信息导致的状态变化和更新。
    * 如果通过 `Unit` 或者 `Board` 的更新反馈来实现的话就无需纠结于 `const iterator/const board/const unit` 之类的问题了。
    * 初步觉得使用 `Board` 的更新反馈比较好，但 `Board` 允许直接返回 `Unit &`，如果直接对其赋值而不是修改的话无法触发
    * 修改 `Board` 使其从不返回 `Unit &` 而只用其提供的接口对 `Unit` 进行修改。
    * 这样子无论如何即使持有 `Board &`， `BoardIterator &` 也无法绕过 `Board` 对 `Unit` 直接修改；因此可通过 `Board` 相关接口进行事件捕获（

##　TODO
 * [x] 框架
 * [x] `Unit` 的方向和旋转 (旋转仅提供绕 `z轴` 顺时针旋转)
 * [x] `Unit` 的状态相关方法
 * [x] 测试 `BoardIterator`
 * [ ] `Board` 的更新反馈
 * [ ] 主要流程控制类，包括时钟、更新触发机制 - Compile Test and Functional Test need
 * [ ] 主要流程控制类的事件系统
 * [ ] 具体的基础 `Unit` 派生类编写
    * `Wire`
    * `Repeater`
    * `Power`
    * `Clock`
 * 下面的测试再说，其他先跑起来
    * 测试 `Transform` 的加减操作
