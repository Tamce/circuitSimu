# Circuit Simu
 准备开一个写电子元件模拟的坑，基本原理跟 `Minecraft` 的红石电路类似，用于练习，随时弃坑预警。
 
## 想法
 * 整体由 1x1 的基本单元 `Unit` 组成，基本单元拥有基本功能如简单传输功能、信号增强功能、单项电流通过功能等。
 * 每个单元的功能由脚本描述，使用 `lua` ？ 这样就可以具有拓展性，`built-in` 的话也不是不可以，这样子新增元件的时候就需要重新编译。
 * 多个单元可以组成一个组件 `Component`，一个组件具有一定的形状（因为由多个单元组成），规定组件必须由连续的单元组成。
 * 一个组件可以由一个或多个单元和组件组成，组件具有形状（也可以看作矩形，没有元件的部分以空 `Null` 填满。