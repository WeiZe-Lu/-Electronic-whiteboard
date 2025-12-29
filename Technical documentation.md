# 电子白板应用文档

## 概述

这是一个功能丰富的电子白板应用程序，支持连续涂绘模式、橡皮擦功能、画板大小调整等特性。用户可以通过鼠标或快捷键进行绘画创作。

## 功能特性

### 1. 绘画模式

- **连续涂绘模式**：单击左键开始/停止绘制，无需持续按住鼠标
- **普通涂绘模式**：按住左键进行绘制
- 模式切换时自动停止当前绘制

### 2. 工具

- **画笔**：支持多种颜色和粗细调整
- **橡皮擦**：独立的橡皮擦功能，粗细可调
- **清空画布**：一键清除所有内容

### 3. 调整功能

- **画板大小调整**：支持自定义宽度(400-1600)和高度(300-1000)
- **画笔粗细**：1-30px可调
- **橡皮擦粗细**：1-50px可调

### 4. 快捷键

- **F键**：切换连续涂绘模式
- **E键**：切换橡皮擦/画笔模式
- **C键**：清空画布
- **Enter键**：调整画板大小（在尺寸输入框中）

## 界面布局

### 顶部说明区域

- 应用标题
- 使用说明和快捷键提示

### 状态显示

- 连续涂绘模式状态显示
- 当前模式状态指示

### 画布区域

- 主要绘图区域
- 白色背景
- 边框设计

### 工具栏

- 颜色选择器
- 画笔粗细调节
- 橡皮擦粗细调节
- 画板尺寸调整
- 橡皮擦按钮
- 清空按钮
- 模式切换按钮

## 技术实现

### 核心技术

- HTML5 Canvas API
- JavaScript事件处理
- CSS3样式设计

### 主要功能实现

1. **绘图逻辑**：通过Canvas的lineTo和stroke方法实现线条绘制
2. **模式切换**：通过isDrawing标志控制绘制状态
3. **橡皮擦功能**：使用globalCompositeOperation='destination-out'
4. **画板调整**：保存当前内容到临时画布，重新设置尺寸后恢复

### 数据管理

- 绘图状态管理（isDrawing, drawingMode, eraserMode）
- 工具设置（颜色、粗细等）
- 画布内容保存与恢复

## 代码修改详细指南

### 1. 调整画板尺寸限制

**修改位置**：JavaScript中的`handleResize`函数



```javascript
// 找到以下代码
if (newWidth >= 400 && newWidth <= 1600 && newHeight >= 300 && newHeight <= 1000)
```

**修改方法**：

- 将`1600`改为所需的最大宽度（如2000）
- 将`1000`改为所需的最大高度（如1200）
- 将`400`改为所需的最小宽度（如300）
- 将`300`改为所需的最小高度（如200）

**同时修改HTML输入框限制**：



```javascript
<!-- 修改前 -->
<input type="number" id="width" min="400" max="1600" value="800">
<input type="number" id="height" min="300" max="1000" value="500">

<!-- 修改后，例如将最大值改为2000和1200 -->
<input type="number" id="width" min="300" max="2000" value="800">
<input type="number" id="height" min="200" max="1200" value="500">
```

### 2. 调整画笔粗细范围

**修改位置**：HTML中的画笔粗细滑块



```javascript
<!-- 修改前 -->
<input type="range" id="size" min="1" max="30" value="5">

<!-- 修改后，例如将最大值改为50 -->
<input type="range" id="size" min="1" max="50" value="5">
```

- 修改`max`属性为所需的最大值（如50）
- 修改`min`属性为所需的最小值（如1）
- 修改`value`为默认值（如10）

**JavaScript中同步修改**（如果需要验证）： 在sizeSlider事件监听器中，可以添加验证逻辑：


```javascript
// 确保值在范围内
const newSize = Math.max(1, Math.min(50, parseInt(sizeSlider.value)));
```

### 3. 调整橡皮擦粗细范围

**修改位置**：HTML中的橡皮擦粗细滑块


```javascript
<!-- 修改前 -->
<input type="range" id="eraserSize" min="1" max="50" value="20">

<!-- 修改后，例如将最大值改为100 -->
<input type="range" id="eraserSize" min="1" max="100" value="20">
```

- 修改`max`属性为所需的最大值（如100）
- 修改`min`属性为所需的最小值（如1）
- 修改`value`为默认值（如30）

### 4. 添加新的快捷键

**修改位置**：JavaScript中的键盘事件监听器



```javascript
// 找到键盘事件监听部分
document.addEventListener('keydown', (e) => {
    // 使用F键切换连续涂绘模式
    if (e.key.toLowerCase() === 'f') {
        toggleDrawingMode();
    }
    // 使用E键切换橡皮擦模式
    if (e.key.toLowerCase() === 'e') {
        toggleEraserMode();
    }
    // 使用C键清空画布
    if (e.key.toLowerCase() === 'c') {
        clearCanvas();
    }
    // 使用Enter键调整画板大小
    if (e.key === 'Enter' && document.activeElement !== sizeSlider && 
        document.activeElement !== eraserSizeSlider && 
        document.activeElement !== colorPicker) {
        e.preventDefault(); // 防止表单提交
        handleResize();
    }
    
    // 在这里添加新的快捷键
    if (e.key.toLowerCase() === '新键名') {
        // 执行相应功能
    }
});
```

**示例：添加Z键作为撤销功能**


```javascript
if (e.key.toLowerCase() === 'z') {
    // 调用撤销函数
    undoAction();
}
```

### 5. 修改默认颜色

**修改位置**：HTML中的颜色选择器


```javascript
<!-- 修改前 -->
<input type="color" id="color" value="#000000">

<!-- 修改后，例如改为红色 -->
<input type="color" id="color" value="#FF0000">
```

- 将`value`属性改为所需的颜色值（十六进制格式）
- 常用颜色值：
    - 红色：`#FF0000`
    - 绿色：`#00FF00`
    - 蓝色：`#0000FF`
    - 紫色：`#800080`
    - 橙色：`#FFA500`

### 6. 修改默认画板尺寸

**修改位置**：HTML中的尺寸输入框



```javascript
<!-- 修改前 -->
<input type="number" id="width" min="400" max="1600" value="800">
<input type="number" id="height" min="300" max="1000" value="500">

<!-- 修改后，例如默认改为1000x700 -->
<input type="number" id="width" min="400" max="1600" value="1000">
<input type="number" id="height" min="300" max="1000" value="700">
```

**同时修改Canvas标签**：



```javascript
<!-- 修改Canvas的默认尺寸 -->
<canvas id="whiteboard" width="1000" height="700"></canvas>
```

### 7. 添加新的绘图工具（如圆形工具）

**步骤1：在HTML工具栏中添加新按钮**



```javascript
<!-- 在现有按钮后添加 -->
<button id="circleTool">圆形工具</button>
```

**步骤2：在JavaScript中添加相应功能**（在现有函数后添加）



```javascript
// 添加圆形工具按钮的事件监听
const circleToolBtn = document.getElementById('circleTool');
let circleToolMode = false;

circleToolBtn.addEventListener('click', () => {
    circleToolMode = !circleToolMode;
    if (circleToolMode) {
        circleToolBtn.textContent = '画笔';
        circleToolBtn.className = 'active';
    } else {
        circleToolBtn.textContent = '圆形工具';
        circleToolBtn.className = 'inactive';
    }
});

// 在鼠标事件中添加圆形绘制逻辑
// 需要添加鼠标按下、移动、释放事件处理
```

**步骤3：在键盘事件中添加快捷键**


```javascript
// 在键盘事件监听器中添加
if (e.key.toLowerCase() === 'o') { // O键用于圆形工具
    circleToolMode = !circleToolMode;
    // 更新按钮状态
}
```

### 8. 修改界面样式

**修改位置**：CSS样式部分

**修改颜色方案**：

- 找到`.status.active`类），修改背景色和文字色
- 找到`.status.inactive`类，修改背景色和文字色
- 找到按钮样式，修改背景色

**调整布局**：

- 修改`.controls`类，调整按钮排列
- 修改`.container`类，调整整体布局

**更改画布样式**：

- 修改`canvas`选择器，调整边框、阴影等

### 9. 添加撤销功能

**步骤1：在JavaScript中添加历史记录数组**



```javascript
// 在变量声明部分添加
let drawingHistory = []; // 存储绘制历史
let historyIndex = -1;   // 当前历史位置

// 保存当前状态的函数
function saveState() {
    // 保存当前画布内容到历史记录
    const imageData = ctx.getImageData(0, 0, canvas.width, canvas.height);
    // 如果当前不是最新的历史记录，删除后续记录
    if (historyIndex < drawingHistory.length - 1) {
        drawingHistory = drawingHistory.slice(0, historyIndex + 1);
    }
    drawingHistory.push(imageData);
    historyIndex++;
    // 限制历史记录数量，避免内存过大
    if (drawingHistory.length > 50) {
        drawingHistory.shift();
        historyIndex--;
    }
}

// 撤销函数
function undoAction() {
    if (historyIndex > 0) {
        historyIndex--;
        ctx.putImageData(drawingHistory[historyIndex], 0, 0);
    }
}

// 重做函数
function redoAction() {
    if (historyIndex < drawingHistory.length - 1) {
        historyIndex++;
        ctx.putImageData(drawingHistory[historyIndex], 0, 0);
    }
}
```

**步骤2：在绘制函数中调用保存状态**



```javascript
// 在绘制函数（如mousemove事件）中，在绘制完成后添加
ctx.stroke();
[lastX, lastY] = [e.offsetX, e.offsetY];
saveState(); // 保存当前状态到历史记录
```

**步骤3：添加撤销/重做按钮**



```javascript
<!-- 在工具栏中添加 -->
<button id="undo">撤销 (Z)</button>
<button id="redo">重做 (Y)</button>
```

**步骤4：添加按钮事件**



```javascript
// 在按钮事件监听部分添加
document.getElementById('undo').addEventListener('click', undoAction);
document.getElementById('redo').addEventListener('click', redoAction);
```

### 10. 保存和加载功能

**步骤1：添加保存功能**


```javascript
// 保存画布为图片
function saveCanvas() {
    const link = document.createElement('a');
    link.download = 'whiteboard.png';
    link.href = canvas.toDataURL('image/png');
    link.click();
}
```

**步骤2：添加加载功能**



```javascript
// 在HTML中添加文件输入
<input type="file" id="loadImage" accept="image/*" style="display: none;">

// 在JavaScript中添加加载功能
document.getElementById('loadImage').addEventListener('change', function(e) {
    const file = e.target.files[0];
    if (file) {
        const reader = new FileReader();
        reader.onload = function(event) {
            const img = new Image();
            img.onload = function() {
                ctx.clearRect(0, 0, canvas.width, canvas.height);
                ctx.drawImage(img, 0, 0);
            };
            img.src = event.target.result;
        };
        reader.readAsDataURL(file);
    }
});
```


**修改默认模式**：



```javascript
// 当前默认是关闭连续模式
let drawingMode = false; // 连续涂绘模式状态

// 如果想默认开启连续模式，改为：
let drawingMode = true; // 连续涂绘模式状态
```

**修改默认橡皮擦状态**：



```javascript
// 当前默认是关闭橡皮擦模式
let eraserMode = false; // 橡皮擦模式

// 如果想默认开启橡皮擦模式，改为：
let eraserMode = true; // 橡皮擦模式
```

### 12. 添加网格功能（如果需要）

**在drawGrid函数中修改网格样式**（如果重新启用）：



```javascript
function drawGrid() {
    ctx.save();
    ctx.strokeStyle = '#e0e0e0'; // 修改网格线颜色
    ctx.lineWidth = 0.5;        // 修改网格线粗细
    
    // 修改网格间距（当前是20像素）
    for (let x = 0; x <= canvas.width; x += 10) { // 改为10像素间距
        ctx.beginPath();
        ctx.moveTo(x, 0);
        ctx.lineTo(x, canvas.height);
        ctx.stroke();
    }
    
    for (let y = 0; y <= canvas.height; y += 10) { // 改为10像素间距
        ctx.beginPath();
        ctx.moveTo(0, y);
        ctx.lineTo(canvas.width, y);
        ctx.stroke();
    }
    
    ctx.restore();
}
```

## 使用说明

### 基本操作

1. 在画布上点击并拖动鼠标进行绘制
2. 使用工具栏调整画笔颜色和粗细
3. 点击橡皮擦按钮切换到橡皮擦模式

### 模式切换

1. 点击"切换模式"按钮或按F键进入连续涂绘模式
2. 在连续模式下，单击左键开始绘制，再次单击停止
3. 按F键返回普通模式

### 画板调整

1. 在宽度和高度输入框中输入新尺寸
2. 点击"调整"按钮或按Enter键应用更改

### 橡皮擦使用

1. 点击"橡皮擦"按钮或按E键切换到橡皮擦模式
2. 在画布上绘制以擦除内容
3. 再次点击按钮或按E键返回画笔模式

## 扩展功能

### 可能的增强

1. 添加更多颜色选择
2. 实现撤销/重做功能
3. 添加形状绘制工具
4. 支持导入/导出图片
5. 添加图层管理功能

### 性能优化

1. 使用离屏Canvas提高绘图性能
2. 实现绘制历史记录的压缩
3. 优化事件处理机制

## 兼容性

- 现代浏览器（Chrome, Firefox, Safari, Edge）
- 支持触摸设备（需添加触摸事件支持）

## 维护说明

- 核心绘图逻辑在JavaScript中实现
- 样式通过CSS管理
- 事件处理采用原生JavaScript
- 无需外部依赖

这个电子白板应用提供了完整的绘图体验，结合了直观的界面设计和丰富的功能特性，适合各种绘图需求。通过文档中的详细修改指南，开发人员可以轻松定制应用功能。
