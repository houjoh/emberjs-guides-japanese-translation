# TRANSITIONING TO SHOW ONLY COMPLETE TODOS
# 完了済みTodoのみの表示に移動する

Next we'll update the application so a user can navigate to a url where only todos that have already been completed are displayed.

次に、すでに完了したTodoだけが表示されるURLにユーザーが移動できるよう、アプリケーションを更新します。

In `index.html` convert the `<a>` tag for 'Completed' todos into a Handlebars `{{link-to}}` helper:

`index.html`にて、’Completed’なTodoのための`<a>`タグを、Handlebarsの`{{link-to}}`ヘルパーに変換します。

```handlebars
<!--- ... additional lines truncated for brevity ... -->
<li>
  <a href="all">All</a>
</li>
<li>
  {{#link-to "todos.active" activeClass="selected"}}Active{{/link-to}}
</li>
<li>
  {{#link-to "todos.completed" activeClass="selected"}}Completed{{/link-to}}
</li>
<!--- ... additional lines truncated for brevity ... -->
```

In `js/router.js` update the router to recognize this new path and implement a matching route:

`js/router.js`にて、この新しいパスを認識できるようにRouterを更新し、対応するRouteを実装します。

```javascript
Todos.Router.map(function () {
  this.resource('todos', { path: '/' }, function () {
    // additional child routes
    this.route('active');
    this.route('completed');
  });
});

// ... additional lines truncated for brevity ...

Todos.TodosCompletedRoute = Ember.Route.extend({
  model: function(){
    return this.store.filter('todo', function (todo) {
      return todo.get('isCompleted');
    });
  },
  renderTemplate: function(controller){
    this.render('todos/index', {controller: controller});
  }
});
```

The model data for this route is the collection of todos whose `isCompleted` property is `true`. When a todo's `isCompleted` property changes this collection will automatically update to add or remove the todo appropriately.

このRouteのためのモデルデータは、`isCompleted`プロパティが`true`なTodoのコレクションです。Todoの`isCompleted`プロパティが変更されたとき、このコレクションは変更されたTodoを適切に追加または削除するように自動的に更新されます。

Normally transitioning into a new route changes the template rendered into the parent `{{outlet}}`, but in this case we'd like to reuse the existing `todos/index` template. We can accomplish this by implementing the `renderTemplate` method and calling `render` ourselves with the specific template and controller options.

通常、新しいRouteに移ると、親の`{{outlet}}`にレンダリングされるテンプレートが変更されます。しかしこの場合、私たちは既存の`todos/index`テンプレートを再利用したいのです。`renderTemplate`メソッドを実装し、特定のテンプレートとControllerのオプションを使って自身の`render`メソッドを呼ぶことで、私たちはこれを実現することができます。

Reload your web browser to ensure that there are no errors and the behavior described above occurs.

ウェブブラウザーをリロードして、エラーが何も起きないことと、上述の動作が行われることを確認してください。

### Live Preview
### ライブ・プレビュー
<a class="jsbin-embed" href="http://jsbin.com/OzUvuPu/1/embed?live">Ember.js • TodoMVC</a><script src="http://static.jsbin.com/js/embed.js"></script>

### Additional Resources
### 追加リソース

  * [Changes in this step in `diff` format](https://github.com/emberjs/quickstart-code-sample/commit/bba939a11197552e3a927bcb3a3adb9430e4f331)
  * [link-to API documentation](/api/classes/Ember.Handlebars.helpers.html#method_link-to)
  * [Route#renderTemplate API documentation](/api/classes/Ember.Route.html#method_renderTemplate)
  * [Route#render API documentation](/api/classes/Ember.Route.html#method_render)
  * [Ember Router Guide](/guides/routing)
  
(The original document’s commit SHA1: 2a44c2312b8828826e0b10ffdd42b8f3d9e956b2)
