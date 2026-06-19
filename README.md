# 智能家居控制系统（C++17）

本项目为课程设计《智能家居控制系统》控制台程序，采用面向对象设计，支持设备管理、状态控制与文件持久化，满足课程答辩提交要求。

## 1. OOP 设计说明

系统通过抽象基类统一设备行为，通过派生类扩展各设备专属能力：

- `SmartDevice`：抽象基类，封装设备名称与开关状态，定义虚函数接口。
- `Light`：灯光设备，支持亮度调节。
- `AirConditioner`：空调设备，支持温度调节。
- `SecurityCamera`：摄像头设备，支持监控模式切换。
- `HomeController`：控制器类，负责菜单交互、设备容器管理、文件读写。

## 2. 继承结构

`SmartDevice` 为父类，`Light`、`AirConditioner`、`SecurityCamera` 为子类，结构如下：

- `SmartDevice` (abstract)
  - `Light`
  - `AirConditioner`
  - `SecurityCamera`

## 3. 多态实现

基类声明纯虚函数 `displayStatus()`，各派生类重写后输出不同状态信息：

- `Light::displayStatus()` 输出亮度信息
- `AirConditioner::displayStatus()` 输出温度信息
- `SecurityCamera::displayStatus()` 输出监控模式

`HomeController` 通过 `std::vector<std::unique_ptr<SmartDevice>>` 存储不同设备对象，并通过基类指针调用虚函数，体现运行时多态。

## 4. 文件存储方式

使用文本文件 `data/devices.txt` 持久化设备数据，格式为：

```txt
设备类型|设备名称|开关状态|扩展字段
```

示例：

```txt
Light|客厅主灯|1|75
AirConditioner|主卧空调|1|24
SecurityCamera|门口摄像头|1|Infrared
```

程序行为：

- 启动时自动读取 `data/devices.txt`
- 菜单支持手动保存/读取
- 退出前析构自动保存

## 5. 现代 C++ 特性使用

项目中已使用以下现代 C++ 写法：

- `std::vector` 管理设备集合
- `std::string` 处理文本
- `std::unique_ptr` 管理多态对象生命周期
- `auto` 与范围 `for` 循环
- `enum class` 定义设备类型和摄像头模式
- `nullptr` 用于安全指针判断
- 统一初始化（列表初始化）如 `int value{0}`

并避免了全局变量，以及大量 C 风格数组/`char*` 用法。

## 6. 项目结构

```txt
智能家居/
├─ include/
│  ├─ SmartDevice.h
│  ├─ Light.h
│  ├─ AirConditioner.h
│  ├─ SecurityCamera.h
│  └─ HomeController.h
├─ src/
│  ├─ SmartDevice.cpp
│  ├─ Light.cpp
│  ├─ AirConditioner.cpp
│  ├─ SecurityCamera.cpp
│  └─ HomeController.cpp
├─ data/
│  └─ devices.txt
├─ main.cpp
└─ CMakeLists.txt
```

## 7. 功能清单

- 查看所有设备状态
- 添加新设备
- 删除设备
- 控制设备开关
- 调节空调温度
- 切换摄像头监控模式
- 保存设备数据到文件
- 读取设备数据
- 退出系统（自动保存）

## 8. 编译与运行（Visual Studio 2022）

### 方式 A：推荐（CMake 打开文件夹）

1. 打开 Visual Studio 2022
2. 选择“打开本地文件夹”，打开项目根目录 `智能家居`
3. Visual Studio 自动识别 `CMakeLists.txt`
4. 选择生成并运行 `SmartHomeControlSystem`

### 方式 B：命令行（需安装 CMake）

在项目根目录执行：

```bash
cmake -S . -B build
cmake --build build --config Release
```

运行可执行文件（Windows）：

```bash
build/Release/SmartHomeControlSystem.exe
```

---

如用于课程答辩，可重点展示：类图关系、多态调用、菜单交互流程、以及文件读写前后数据一致性。
