# Angular

## AngularJS

### 指令

   ng-app

   ng-controller

   ng-model

   ng-bind

   ng-init

   CSS

​      ng-empty

​      ng-not-empty

​      ng-touched

​      ng-untouched

​      ng-valid

​      ng-invalid

​      ng-dirty

​      ng-pending

​      ng-pristine

\### Scope

   当在控制器中添加 $scope 对象时, 视图 (HTML) 可以获取了这些属性

   视图中, 不需要添加 $scope 前缀, 只需要添加属性名即可, 如： {{carname}}

   AngularJS 应用组成如下:

\- View(视图), 即 HTML。

\- Model(模型), 当前视图中可用的数据。

\- Controller(控制器), 即 JavaScript 函数，可以添加或修改属性。

   Scope为Model

   Scope 是一个 JavaScript 对象，带有属性和方法，这些属性和方法可以在视图和控制器中使用。

   所有的应用都有一个 $rootScope，它可以作用在 ng-app 指令包含的所有 HTML 元素中。

\### 过滤器

   |

   currency   格式化数字为货币格式。

   filter   从数组项中选择一个子集。

   lowercase   格式化字符串为小写。

   orderBy   根据某个表达式排列数组。

   uppercase   格式化字符串为大写。

   过滤输入

\### 服务

   $http 是 AngularJS 应用中最常用的服务。 服务向服务器发送请求，应用响应服务器传送过来的数据。

   $timeout 服务对应了 JS window.setTimeout 函数。

   $timeout(function () {}, 2000);

   $interval 服务对应了 JS window.setInterval 函数。

   自定义服务  

   app.service('hexafy', function() {

   this.myFunc = function (x) {

​      return x.toString(16);

   }

   });