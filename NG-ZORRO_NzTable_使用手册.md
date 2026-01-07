# NG-ZORRO NzTable 组件使用手册

**文档版本**：v1.0
**适配组件库版本**：NG-ZORRO 11.4.x
**适用场景**：Angular 项目中表格数据展示、交互开发（含排序、筛选、分页等基础功能）
**编写目的**：为前端开发人员提供 NzTable 组件的快速上手指南、核心 API 说明及常见问题解决方案，降低组件使用门槛

## 一、组件介绍

NzTable 是 NG-ZORRO（Ant Design of Angular）组件库中的高性能表格组件，基于 Angular 框架封装实现。该组件支持数据绑定、列自定义、排序、筛选、分页、单元格编辑等核心功能，具备良好的兼容性和可扩展性，广泛应用于后台管理系统、数据报表展示等业务场景。

核心优势：
1. 贴合 Angular 生态，支持响应式数据绑定与变更检测；
2. 提供丰富的交互能力，满足大部分数据展示场景需求；
3. 支持自定义模板，可灵活适配个性化UI设计；
4. 性能优化出色，支持大数据量（千级数据）高效渲染。

## 二、快速上手

### 2.1 前置依赖

使用 NzTable 组件前，需确保项目已集成 NG-ZORRO 组件库，具体集成要求：
1.  Angular 版本 ≥ 10.0.0；
2.  已安装 ng-zorro-antd 依赖包；
3.  项目已导入 NG-ZORRO 核心模块及样式。

### 2.2 安装与导入

#### 步骤1：安装依赖

通过 npm 或 yarn 安装 ng-zorro-antd 包，命令如下：

```bash
# npm 安装
npm install ng-zorro-antd --save

# yarn 安装
yarn add ng-zorro-antd
```

#### 步骤2：导入模块

在需要使用 NzTable 组件的模块（如 AppModule 或业务模块）中，导入 NzTableModule 及相关依赖模块，代码示例：

```typescript
import { NgModule } from '@angular/core';
import { BrowserModule } from '@angular/platform-browser';
import { BrowserAnimationsModule } from '@angular/platform-browser/animations'; // 必导，支持动画
import { NzTableModule } from 'ng-zorro-antd/table'; // NzTable 核心模块
import { NzButtonModule } from 'ng-zorro-antd/button'; // 可选，如需表格内按钮交互
import { AppComponent } from './app.component';

@NgModule({
  declarations: [AppComponent],
  imports: [
    BrowserModule,
    BrowserAnimationsModule,
    NzTableModule, // 导入表格模块
    NzButtonModule // 导入按钮模块（可选）
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

#### 步骤3：导入样式

在项目根目录的 styles.less 或 styles.scss 文件中，导入 NG-ZORRO 全局样式：

```less
// styles.less
@import "~ng-zorro-antd/ng-zorro-antd.less";
```

### 2.3 基础使用示例

以下为 NzTable 基础使用示例，实现简单的数据展示、分页功能：

#### 2.3.1 组件模板（HTML）

```html
<!-- app.component.html -->
<nz-table 
  #basicTable 
  [nzData]="tableData" 
  [nzPageSize]="5" 
  [nzTotal]="total"
  [nzLoading]="loading"
  nzBordered
>
  <thead>
    <tr>
      <th nzWidth="100px">序号</th>
      <th>姓名</th>
      <th>年龄</th>
      <th>职位</th>
      <th nzWidth="150px">操作</th>
    </tr>
  </thead>
  <tbody>
    <tr *ngFor="let data of basicTable.data; let i = index">
      <td>{{ i + 1 }}</td>
      <td>{{ data.name }}</td>
      <td>{{ data.age }}</td>
      <td>{{ data.position }}</td>
      <td>
        <button nz-button nzSize="small" nzType="primary">编辑</button>
        <button nz-button nzSize="small" nzType="default" style="margin-left: 8px;">查看</button>
      </td>
    </tr>
  </tbody>
</nz-table>
```

#### 2.3.2 组件类（TS）

```typescript
// app.component.ts
import { Component, OnInit } from '@angular/core';

// 定义表格数据类型接口
interface TableItem {
  name: string;
  age: number;
  position: string;
}

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.less']
})
export class AppComponent implements OnInit {
  // 表格数据源
  tableData: TableItem[] = [];
  // 数据总数（分页用）
  total = 0;
  // 加载状态
  loading = false;

  ngOnInit(): void {
    // 模拟接口请求数据
    this.loading = true;
    setTimeout(() => {
      this.tableData = [
        { name: '张三', age: 25, position: '前端开发工程师' },
        { name: '李四', age: 28, position: '产品经理' },
        { name: '王五', age: 30, position: '后端开发工程师' },
        { name: '赵六', age: 26, position: '测试工程师' },
        { name: '孙七', age: 27, position: 'UI设计师' }
      ];
      this.total = this.tableData.length;
      this.loading = false;
    }, 1000);
  }
}
```

#### 2.3.3 效果说明

上述代码实现效果：
1. 展示5条模拟数据，包含序号、姓名、年龄、职位及操作列；
2. 启用表格边框，显示加载状态；
3. 分页功能默认每页显示5条数据；
4. 操作列提供“编辑”“查看”按钮（仅展示UI，未实现具体逻辑）。

## 三、核心 API 说明

本节整理 NzTable 组件最常用的输入属性、输出事件及方法，完整 API 可参考[NG-ZORRO 官方文档](https://ng.ant.design/version/11.4.x/components/table/zh)。

### 3.1 输入属性（@Input()）

|属性名|类型|默认值|说明|
|---|---|---|---|
|nzData|any[]|[]|表格数据源，支持数组格式数据绑定|
|nzPageSize|number|10|每页显示数据条数，用于分页控制|
|nzTotal|number|0|数据总条数，分页组件会根据此值计算总页数|
|nzLoading|boolean|false|是否显示加载状态（加载时表格显示骨架屏）|
|nzBordered|boolean|false|是否显示表格边框|
|nzShowPagination|boolean / 'top' / 'bottom' / 'both'|'bottom'|是否显示分页组件，可指定显示位置（顶部、底部、上下都显示）|
|nzSortFn|(a: any, b: any, sortName: string) => number|undefined|自定义排序函数，用于实现复杂排序逻辑|
|nzFilterFn|(value: any, data: any, col: NzTableColumn) => boolean|undefined|自定义筛选函数，用于实现复杂筛选逻辑|
### 3.2 输出事件（@Output()）

|事件名|参数类型|说明|
|---|---|---|
|nzPageIndexChange|number|页码变化时触发，参数为新页码|
|nzPageSizeChange|number|每页条数变化时触发，参数为新的每页条数|
|nzSortChange|{ sortName: string; sortValue: 'ascend' | 'descend' | null }|排序变化时触发，参数包含排序字段名和排序方向（升序/降序/无）|
|nzFilterChange|{ [key: string]: any[] }|筛选变化时触发，参数为筛选条件（键为字段名，值为筛选值数组）|
### 3.3 常用方法（通过模板引用获取）

通过在模板中给 NzTable 组件添加 #basicTable 这样的模板引用，可在组件类中获取组件实例并调用以下方法：

|方法名|参数|说明|
|---|---|---|
|refreshData|无|刷新表格数据，重新渲染表格|
|clearSort|无|清除排序状态，恢复默认排序|
|clearFilter|无|清除筛选条件，恢复默认筛选状态|

## 四、常见问题与解决方案

### 4.1 问题1：表格数据更新后视图不刷新

#### 现象

通过接口请求获取新数据后，已将数据赋值给 nzData 绑定的变量，但表格视图仍显示旧数据，未同步更新。

#### 原因

Angular 的变更检测机制未检测到数据变化。若直接修改数组内部元素（如 push、splice 外的修改），或通过第三方库修改数据，可能导致变更检测未触发。

#### 解决方案

方案1：使用数组解构赋值，创建新数组触发变更检测：

```typescript
// 错误写法：直接修改数组元素，可能不触发变更检测
this.tableData[0].name = '新名称';

// 正确写法：解构赋值创建新数组
this.tableData = [...this.tableData.map(item => item.id === 1 ? { ...item, name: '新名称' } : item)];
```

方案2：手动触发变更检测（需导入 ChangeDetectorRef）：

```typescript
import { ChangeDetectorRef } from '@angular/core';

constructor(private cdr: ChangeDetectorRef) {}

// 数据更新后手动触发变更检测
this.tableData[0].name = '新名称';
this.cdr.detectChanges();
```

### 4.2 问题2：自定义列无法获取当前行数据

#### 现象

在表格列中使用自定义模板（如 ng-template）时，无法获取当前行的 data 数据，导致无法渲染行内个性化内容。

#### 原因

未正确使用 NG-ZORRO 提供的模板变量，自定义模板需通过特定变量获取当前行数据。

#### 解决方案

使用 ng-template 的 let-data 变量获取当前行数据，示例如下：

```html
<!-- 自定义列模板，通过 let-data 获取当前行数据 -->
<th>
  <ng-template #customColumn let-data>
    <span [style.color]="data.age > 28 ? 'red' : 'black'">
      {{ data.name }}（{{ data.age }}岁）
    </span>
  </ng-template>
</th>
```

### 4.3 问题3：分页功能不生效

#### 现象

设置了 nzPageSize 和 nzTotal 后，分页组件显示正常，但切换页码时，表格数据未发生变化。

#### 原因

未监听 nzPageIndexChange 事件，切换页码时未重新请求对应页码的数据，nzData 仍为原始完整数据。

#### 解决方案

监听 nzPageIndexChange 事件，在事件回调中根据新页码请求对应数据，示例如下：

```html
<!-- 模板中添加页码变化事件监听 -->
<nz-table 
  [nzData]="tableData" 
  [nzPageSize]="5" 
  [nzTotal]="total"
  (nzPageIndexChange)="onPageIndexChange($event)"
&gt;
  <!-- 表格内容省略 -->
</nz-table>
```

```typescript
// 组件类中实现事件回调
onPageIndexChange(pageIndex: number): void {
  // 根据新页码请求数据（示例为模拟接口请求）
  this.loading = true;
  setTimeout(() => {
    // 实际场景中需根据 pageIndex 和 nzPageSize 从后端获取对应页数据
    this.tableData = this.getAllData.slice((pageIndex - 1) * 5, pageIndex * 5);
    this.loading = false;
  }, 1000);
}
```

### 4.4 问题4：表格排序不生效

#### 现象

点击表格表头的排序按钮，表格数据未按预期排序。

#### 原因

1. 未给需要排序的列设置 nzSortKey 属性；
2. 未监听 nzSortChange 事件，未实现排序逻辑；
3. 自定义排序函数 nzSortFn 逻辑错误。

#### 解决方案

方案1：简单排序（基于单个字段）：

```html
<!-- 给需要排序的列设置 nzSortKey -->
<th nzSortKey="age">年龄</th>
```

方案2：自定义排序（复杂逻辑）：

```html
<!-- 模板中设置自定义排序函数并监听排序变化 -->
<nz-table 
  [nzData]="tableData" 
  [nzSortFn]="sortFn"
  (nzSortChange)="onSortChange($event)"
>
  <thead>
    <tr>
      <th nzSortKey="position"&gt;职位&lt;/th&gt;
      <!-- 其他列省略 -->
    </tr>
  </thead>
</nz-table>
```

```typescript
// 组件类中实现排序函数和事件回调
// 自定义排序函数
sortFn = (a: TableItem, b: TableItem, sortName: string): number => {
  if (sortName === 'position') {
    // 按职位拼音首字母排序（示例逻辑）
    return a.position.localeCompare(b.position, 'zh-CN');
  }
  return 0;
};

// 排序变化事件回调
onSortChange(sort: { sortName: string; sortValue: 'ascend' | 'descend' | null }): void {
  // 若需后端排序，可在此处根据 sortName 和 sortValue 请求排序后的数据
  console.log('排序字段：', sort.sortName, '排序方向：', sort.sortValue);
}
```

## 五、附录：参考资料

1. NG-ZORRO 官方文档 - NzTable 组件：[https://ng.ant.design/version/11.4.x/components/table/zh](https://ng.ant.design/version/11.4.x/components/table/zh)
2. Angular 官方文档 - 变更检测：[https://angular.cn/guide/changing-deetection](https://angular.cn/guide/changing-deetection)
> （注：文档部分内容可能由 AI 生成）