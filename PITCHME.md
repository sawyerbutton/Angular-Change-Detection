---
## @color[orange](Angular推进器)

### Angular从原理到应用 - @color[#DC143C](变更检测)
---
### 我是谁？
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
- <img src="http://teropa.info/images/onchange_watch.svg" width="400">
---
### 多种解决方案
- HTTP 请求 + server 重新渲染
- 区分前后DOM的差异, 渲染不同的部分 -> React Virtual DOM
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
###
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





