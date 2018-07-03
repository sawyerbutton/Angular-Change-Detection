---
## @color[orange](Angular推进器)

### Angular从原理到应用 - @color[#DC143C](变更检测)
---
### 我是谁?
- Angular开发从业者 from angular4
- NodeJS开发者 from express to Nestjs
- 我也是
<img src="assets/img/logo1.jpg" width="88">
和
<img src="assets/img/logo2.jpg" width="88">
---
### 什么是变更检测
- 获取程序的内部状态
- 使其以某种方式对用户界面可见
- 状态可以是: objects, Arrays, Primitives...
---
### 当检测发生在运行时
- 模型中的变化 -> 更新DOM的位置
- 操作DOM开销昂贵
- <img style="background: #0c4eb2; padding: 0 1em; width: 300px;text-align: center" src="https://blog.thoughtram.io/images/cd-4.svg">
---
### 多种解决方案
- HTTP 请求 + server 重新渲染
- 区分前后DOM的差异, 渲染不同的部分
- React Virtual DOM
---
### 回到Angular
- 变化什么时候发生?
```javascript
@Component({
  template: `
    <h1>{{firstname}} {{lastname}}</h1>
    <button (click)="changeName()">Change name</button>
  `
})
export class AppComponent {

  firstname:string = 'Material';
  lastname:string = 'Angular';

  changeName() {
    this.firstname = 'Awesome';
    this.lastname = 'Angular';
  }
}
```
---
### 另一个例子
```javascript
@Component()
export class ContactsComponent implements OnInit{

  people:Person[] = [];

  constructor(private http: HttpClient) {}

  ngOnInit() {
    this.http.get('/people')
      .map(res => res.json())
      .subscribe(contacts => this.contacts = contacts);
  }
}
```
---
### 换句话说 什么会触发变更
- Events - click, submit...事件
- XHR - 从远端服务器获取数据
- Timers - setTimeout(), setInterval() 浏览器web api
---
### 触发变更?

#### Asynchronous
---
### 谁通知了Angular? 
- [Zone.js](https://github.com/angular/zone.js#augmenting-a-zones-hook)
- [NgZone](https://angular.io/api/core/NgZone) implements from Zone.js
```javascript
// 源码简化
class ApplicationRef {

  changeDetectorRefs:ChangeDetectorRef[] = [];
  // applicationRef在构造器中监听onTurnDone事件
  constructor(private zone: NgZone) {
    this.zone.onTurnDone
      .subscribe(() => this.zone.run(() => this.tick());
  }
// tick函数遍历所有的探测器的接口/对象 对其执行检测
  tick() {
    this.changeDetectorRefs
      .forEach((ref) => ref.detectChanges());
  }
}
```
---
### 变更检测是如何进行的呢?
- Key: 每一个组件都有属于自己的变更检测器(change detector)
- <img style="background: #0c4eb2; padding: 0 1em; width: 300px" src="https://blog.thoughtram.io/images/cd-tree-2.svg"> <img style="background: #0c4eb2; padding: 0 1em; width: 300px" src="https://blog.thoughtram.io/images/cd-tree-7.svg">
- 变更检测树 change detector tree: 有向图 数据流从上而下
---
### 数据流单向从上到下?
- 变更检测 from top to bottom
- 优点多多
---
### 效率如何?
- 感觉上很慢实际上很快 得益于 Angular 生成 VM 友好的代码
- VM 不喜欢动态不确定的代码 VM的优化得益于 object的单态 而不是多态
- Angular在运行时 创造变更检测器 - 单态 - 确定的model
- Don't worry, Angular 帮我们处理好了这些复杂的部分
---
### 更聪明的变更检测
- Angular默认的变更检测是自动的
- 两个好帮手: Immutable(不可变) & Observables
---
### 理解可变和不可变(Mutability)
- reference 没变 但是 property 改变 -> Angular负责地进行检测
```javascript
@Component({
  template: '<child [data]="data"></child>'
})
export class ParentComponent {

  constructor() {
    this.data = {
      name: 'Button',
      email: 'github.com'
    }
  }

  changeData() {
    this.data.name = 'Tommy';
  }
}
```
---
### 不可变对象
- reference change
```javascript
var data = someAPIForImmutables.create({
              name: 'Button'
            });

var data2 = data.set('name', 'Tommy');

data === data2 // false reference are different
```
---
### 优化?
- 变更检测可以跳过某些component子树(@Input()属性immutable)
- 需要告诉angular 
---
### 比如
- OnPush Strategy
```javascript
@Component({
  template: `
    <h2>{{data.name}}</h2>
    <span>{{data.email}}</span>
  `
  // onPush 策略会在@Input()的内容属性不变时生效
  changeDetection: ChangeDetectionStrategy.OnPush
})
class VCardCmp {
  @Input() data;
}
```
---
### 结果?
- immutable object + OnPush
- <img style="background: #0c4eb2; padding: 0 1em; width: 300px" src="https://blog.thoughtram.io/images/cd-tree-8.svg">
---