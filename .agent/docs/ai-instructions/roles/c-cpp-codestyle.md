# c/cpp 命名约定规范

> **适用范围**：c/cpp代码
> **核心原则**：代码格式由 `clang-format` 强制执行。

---

## 1. 类 (Classes)
- **风格**：`PascalCase`
- **示例**：`class MyPlayer`、`class EnemyManager`

## 2. 函数与方法 (Functions & Methods)
- **风格**：`snake_case`
- **示例**：`void move_player()`、`int get_health()`

## 3. 变量 (Variables)
- **风格**：`snake_case`
- **示例**：`int player_speed`、`Vector3 target_position`
- **要求**：指针/引用运算符 `*` 和 `&` **必须紧贴变量名**（如 `int *my_pointer`），而非类型名。

## 4. 常量 (Constants)
- **风格**：`ALL_CAPS`
- **示例**：`const int MAX_SPEED = 100;`
- **建议**：优先使用 `constexpr` 代替 `const`。

## 5. 结构体 (Structs)
- **风格**：`PascalCase`
- **示例**：`struct Transform3D`、`struct Color`
- **要求**：与类的命名规则保持一致。

## 6. 枚举 (Enums)
- **类型名风格**：`PascalCase`
- **枚举值风格**：`ALL_CAPS` 或 `PascalCase`（视具体上下文）
- **示例**：`enum class Direction { UP, DOWN, LEFT, RIGHT };`

## 7. 宏定义 (Macros)
- **风格**：`ALL_CAPS`
- **示例**：`#define ARRAY_SIZE (10)`
- **要求**：数值宏定义应使用括号包裹参数（如 `#define SQUARE(x) ((x)*(x))`）。

## 8. 头文件保护 (Header Guards)
- **方式**：标准 `#ifdef` / `#define` 方式
- **示例**：
  ```cpp
  #ifndef MY_CLASS_H
  #define MY_CLASS_H
  // ...
  #endif