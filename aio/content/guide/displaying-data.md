<!--
# Displaying data in views
-->
# 화면에 데이터 표시하기

<!--
Angular [components](guide/glossary#component) form the data structure of your application.
The HTML [template](guide/glossary#template) associated with a component provides the means to display that data in the context of a web page.
Together, a component's class and template form a [view](guide/glossary#view) of your application data.

The process of combining data values with their representation on the page is called [data binding](guide/glossary#data-binding).
You display your data to a user (and collect data from the user) by *binding* controls in the HTML template to the data properties of the component class.

In addition, you can add logic to the template by including [directives](guide/glossary#directive), which tell Angular how to modify the page as it is rendered.

Angular defines a *template language* that expands HTML notation with syntax that allows you to define various kinds of data binding and logical directives.
When the page is rendered, Angular interprets the template syntax to update the HTML according to your logic and current data state.
Before you read the complete [template syntax guide](guide/template-syntax), the exercises on this page give you a quick demonstration of how template syntax works.

In this demo, you'll create a component with a list of heroes.
You'll display the list of hero names and conditionally show a message below the list.
The final UI looks like this:

<div class="lightbox">
  <img src="generated/images/guide/displaying-data/final.png" alt="Final UI">
</div>

<div class="alert is-helpful">

The <live-example></live-example> demonstrates all of the syntax and code snippets described in this page.

</div>
-->
애플리케이션의 데이터 구조는 Angular [컴포넌트](guide/glossary#component)로 구성합니다.
그리고 컴포넌트 데이터를 웹 페이지로 표시하려면 HTML로 [템플릿](guide/glossary#template)을 작성하면 됩니다.
결국 애플리케이션 데이터를 표시하는 [화면](guide/glossary#view)은 컴포넌트 클래스와 템플릿이 합쳐진 결과물입니다.

컴포넌트 데이터와 화면이 연결지는 과정을 [데이터 바인딩](guide/glossary#data-binding)이라고 합니다.
데이터 바인딩을 활용하면 사용자가 볼 수 있게 데이터를 화면에 표시할 수 있고, 사용자가 입력하는 데이터를 컴포넌트 클래스에 있는 데이터 프로퍼티로 전달할 수 있습니다.

그리고 템플릿에 [디렉티브](guide/glossary#directive)를 활용하면 화면이 렌더링되는 과정을 조작할 수 있습니다.

Angular는 데이터 바인딩과 디렉티브를 활용할 수 있도록 HTML 문법을 확장한 *템플릿 언어(template language)*를 제공합니다.
Angular는 화면이 렌더링되는 시점에 템플릿 문법으로 작성된 대로 HTML 엘리먼트를 조작하거나 컴포넌트 데이터를 템플릿에 연결합니다.
이 문서의 내용을 진행하면 템플릿 문법이 어떻게 동작하는지 간단하게 확인할 수 있으며, 자세한 내용을 확인하려면 [템플릿 문법](guide/template-syntax) 문서를 참고하세요.

이 문서에서는 히어로 목록을 표시하는 컴포넌트를 생성해 봅니다.
히어로 목록에 있는 히어로들의 이름을 화면에 표시해 볼 것이며, 조건에 따라 목록 아래에 메시지를 표시해 보기도 합시다.
이 과정을 끝내고 나면 다음과 같은 화면이 구성될 것입니다:

<div class="lightbox">
  <img src="generated/images/guide/displaying-data/final.png" alt="최종 화면">
</div>

<div class="alert is-helpful">

이 문서에서 설명하는 내용은 <live-example></live-example> 에서 확인하거나 다운받을 수 있습니다.

</div>


{@a interpolation}

<!--
## Showing component properties with interpolation
-->
## 문자열 바인딩

<!--
The easiest way to display a component property is to bind the property name through interpolation.
With interpolation, you put the property name in the view template, enclosed in double curly braces: `{{myHero}}`.

Use the CLI command [`ng new displaying-data`](cli/new) to create a workspace and app named `displaying-data`.

Delete the <code>app.component.html</code> file. It is not needed for this example.

Then modify the <code>app.component.ts</code> file by
changing the template and the body of the component.

When you're done, it should look like this:

<code-example path="displaying-data/src/app/app.component.1.ts" header="src/app/app.component.ts"></code-example>

You added two properties to the formerly empty component: `title` and `myHero`.

The template displays the two component properties using double curly brace
interpolation:

<code-example path="displaying-data/src/app/app.component.1.ts" header="src/app/app.component.ts (template)" region="template"></code-example>

<div class="alert is-helpful">

The template is a multi-line string within ECMAScript 2015 backticks (<code>\`</code>).
The backtick (<code>\`</code>)&mdash;which is *not* the same character as a single
quote (`'`)&mdash;allows you to compose a string over several lines, which makes the
HTML more readable.

</div>

Angular automatically pulls the value of the `title` and `myHero` properties from the component and
inserts those values into the browser. Angular updates the display
when these properties change.

<div class="alert is-helpful">

More precisely, the redisplay occurs after some kind of asynchronous event related to
the view, such as a keystroke, a timer completion, or a response to an HTTP request.

</div>

Notice that you don't call **new** to create an instance of the `AppComponent` class.
Angular is creating an instance for you. How?

The CSS `selector` in the `@Component` decorator specifies an element named `<app-root>`.
That element is a placeholder in the body of your `index.html` file:

<code-example path="displaying-data/src/index.html" header="src/index.html (body)" region="body"></code-example>

When you bootstrap with the `AppComponent` class (in <code>main.ts</code>), Angular looks for a `<app-root>`
in the `index.html`, finds it, instantiates an instance of `AppComponent`, and renders it
inside the `<app-root>` tag.

Now run the app. It should display the title and hero name:

<div class="lightbox">
  <img src="generated/images/guide/displaying-data/title-and-hero.png" alt="Title and Hero">
</div>

The next few sections review some of the coding choices in the app.
-->
컴포넌트 프로퍼티를 화면에 표시하는 방법 중 가장 간단한 방법은
문자열 바인딩(interpolation)을 사용하는 것입니다.
문자열 바인딩은 프로퍼티 이름을 이중 중괄호로 감싸서 템플릿에 `{{myHero}}` 와 같은 형태로 넣는 방법입니다.

워크스페이스를 생성하면서 `displaying-data`라는 이름으로 앱을 생성하기 위해 [`ng new displaying-data`](cli/new) 명령을 실행합니다.

그리고 이번 예제에서는 <code>app.component.html</code> 파일을 사용하지 않으니 삭제하고,
화면에 히어로의 이름을 표시하도록 <code>app.component.ts</code> 파일을 다음과 같이 작성합니다:

<code-example path="displaying-data/src/app/app.component.1.ts" header="src/app/app.component.ts"></code-example>

컴포넌트 프로퍼티에는 `title` 과 `myHero`를 선언했습니다.

그리고 두 프로퍼티 값을 화면에 표시하도록 다음과 같이 템플릿에 문자열 바인딩 합니다:

<code-example path="displaying-data/src/app/app.component.1.ts" header="src/app/app.component.ts (템플릿)" region="template"></code-example>

<div class="alert is-helpful">

템플릿에 사용된 역따옴표(<code>\`</code>)는 문자열을 여러 줄에 걸쳐 선언하는 ECMAScript 2015 표준 문법이며,
역따옴표를 사용하면 HTML 코드의 가독성을 더 높일 수 있습니다.
역따옴표(<code>\`</code>)와 홑따옴표(`'`)를 혼동하지 않도록 주의하세요.

</div>

그러면 컴포넌트에 있는 `title` 과 `myHero` 프로퍼티 값을 Angular가 끌어와서 템플릿에 표시합니다.
이렇게 바인딩 된 프로퍼티는 값이 변경될 때마다 Angular가 감지해서 화면을 갱신합니다.

<div class="alert is-helpful">

조금 더 정확하게 이야기하면, 키 입력이나 타이머, HTTP 응답과 같은 비동기 이벤트가 발생했을 때 화면이 갱신됩니다.

</div>

Angular에서는 `new` 키워드를 사용하지 않아도 알아서 컴포넌트의 인스턴스를 생성하고 DOM에 추가합니다. 이 과정이 어떻게 이루어 질까요?

`@Component` 데코레이터에 지정된 메타데이터를 보면 `selector` 항목에 `<app-root>` 가 지정되어 있고,
`index.html` 파일에는 `<app-root>` 가 다음과 같이 작성되어 있습니다:

<code-example path="displaying-data/src/index.html" header="src/index.html (body)" region="body"></code-example>

Angular 애플리케이션이 시작되면서 `AppComponent` 클래스가 부트스트랩 될 때, Angular는 `index.html` 파일에서 `<app-root>` 엘리먼트를 찾습니다.
그리고 이 엘리먼트를 찾은 위치에 `AppComponent` 인스턴스를 생성하고 화면에 렌더링합니다.

여기까지 작성하고 애플리케이션을 시작하면, 페이지 제목과 히어로 이름이 다음과 같이 표시됩니다:

<div class="lightbox">
  <img src="generated/images/guide/displaying-data/title-and-hero.png" alt="페이지 제목과 히어로가 표시된 화면">
</div>

다음 섹션에서는 앱을 개발할때 고민해 볼만한 문제에 대해 생각해 봅시다.


<!--
## Choosing the template source
-->
## 템플릿 구성방식 결정하기

<!--
The `@Component` metadata tells Angular where to find the component's template.
You can store your component's template in one of two places.

* You can define the template *inline* using the `template` property of the `@Component` decorator. An inline template is useful for a small demo or test.
* Alternatively, you can define the template in a separate HTML file and link to that file in the `templateUrl` property of the `@Component` decorator. This configuration is typical for anything more complex than a small test or demo, and is the default when you generate a new component.

In either style, the template data bindings have the same access to the component's properties.
Here the app uses inline HTML because the template is small and the demo is simpler without the additional HTML file.

<div class="alert is-helpful">

  By default, the Angular CLI command [`ng generate component`](cli/generate) generates components with a template file.
  You can override that by adding the "-t" (short for `inlineTemplate=true`) option:

  <code-example hideCopy language="sh" class="code-shell">
    ng generate component hero -t
  </code-example>

</div>
-->
`@Component` 메타데이터의 내용을 변경하면 컴포넌트 템플릿을 구성하는 두가지 방식 중 하나를 선택할 수 있습니다.

* `@Component` 데코레이터에 `template` 프로퍼티를 사용하면 템플릿을 *인라인(inline)*으로 구성할 수 있습니다. 이 방식은 간단한 데모용, 테스트용 컴포넌트를 만들때 유용합니다.
* `@Component` 데코레이터에 `templateUrl` 프로퍼티를 사용하면 HTML 파일을 불러와서 템플릿으로 구성할 수 있습니다. 이 방식은 데모용이나 테스트용 컴포넌트보다 템플릿이 좀 더 복잡할 때 사용할 수 있으며, Angular 컴포넌트 템플릿을 사용하는 일반적인 방식입니다. Angular CLI로 컴포넌트를 생성하면 이 방식이 사용됩니다.

두 경우 모두 템플릿에 사용된 데이터 바인딩은 같은 방식으로 컴포넌트 프로퍼티와 연결됩니다.
아직까지는 템플릿을 별도 HTML 파일로 구성할만큼 복잡하지 않기 때문에 이 문서에서는 인라인 HTML 템플릿 방식을 사용하도록 합시다.

<div class="alert is-helpful">

  기본 옵션으로 Angular CLI [`ng generate component`](cli/generate) 명령을 실행하면 템플릿 파일이 별도로 생성됩니다.
  이 때 `-t` 옵션이나 `inlineTemplate=true` 옵션을 사용하면 템플릿을 인라인으로 생성할 수 있습니다:

  <code-example hideCopy language="sh" class="code-shell">
    ng generate component hero -t
  </code-example>

</div>


<!--
## Initialization
-->
## 변수 초기화

<!--
The following example uses variable assignment to initialize the components.

<code-example path="displaying-data/src/app/app-ctor.component.1.ts" region="class"></code-example>

 You could instead declare and initialize the properties using a constructor.
 This app uses more terse "variable assignment" style simply for brevity.
-->
아래 코드는 컴포넌트 프로퍼티의 초기값을 지정하는 코드입니다.

<code-example path="displaying-data/src/app/app-ctor.component.1.ts" region="class"></code-example>

컴포넌트 프로퍼티는 생성자에서 선언하고 바로 값을 할당할 수도 있습니다.
간결함만 따진다면 이 방식이 더 좋습니다.


{@a ngFor}

<!--
## Add logic to loop through data
-->
## 데이터 순회 로직 추가하기

<!--
The `*ngFor` directive (predefined by Angular) lets you loop through data. The following example uses the directive to show all of the values in an array property.

To display a list of heroes, begin by adding an array of hero names to the component and redefine `myHero` to be the first name in the array.


<code-example path="displaying-data/src/app/app.component.2.ts" header="src/app/app.component.ts (class)" region="class"></code-example>


Now use the Angular `ngFor` directive in the template to display each item in the `heroes` list.


<code-example path="displaying-data/src/app/app.component.2.ts" header="src/app/app.component.ts (template)" region="template"></code-example>


This UI uses the HTML unordered list with `<ul>` and `<li>` tags. The `*ngFor`
in the `<li>` element is the Angular "repeater" directive.
It marks that `<li>` element (and its children) as the "repeater template":


<code-example path="displaying-data/src/app/app.component.2.ts" header="src/app/app.component.ts (li)" region="li"></code-example>

<div class="alert is-important">

Don't forget the leading asterisk (\*) in `*ngFor`. It is an essential part of the syntax.
Read more about `ngFor` and `*` in the [ngFor section](guide/built-in-directives#ngfor) of the [Built-in directives](guide/built-in-directives) page.

</div>

Notice the `hero` in the `ngFor` double-quoted instruction;
it is an example of a template input variable. Read
more about template input variables in the [microsyntax](guide/built-in-directives#microsyntax) section of
the [Built-in directives](guide/built-in-directives) page.

Angular duplicates the `<li>` for each item in the list, setting the `hero` variable
to the item (the hero) in the current iteration. Angular uses that variable as the
context for the interpolation in the double curly braces.

<div class="alert is-helpful">

In this case, `ngFor` is displaying an array, but `ngFor` can
repeat items for any [iterable](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols) object.

</div>

Now the heroes appear in an unordered list.

<div class="lightbox">
  <img src="generated/images/guide/displaying-data/hero-names-list.png" alt="After ngfor">
</div>
-->
Angular 내장 디렉티브인 `*ngFor`를 활용하면 데이터를 순회할 수 있습니다.
그래서 배열을 순회하며 데이터를 표시할 때도 이 디렉티브를 활용합니다.

히어로의 목록을 표시하려면 먼저 컴포넌트에 배열을 정의해야 합니다. `myHero` 배열을 만들고, 이 배열에 히어로들의 이름을 할당합시다.


<code-example path="displaying-data/src/app/app.component.2.ts" header="src/app/app.component.ts (클래스)" region="class"></code-example>


그리고 Angular에서 제공하는 `ngFor` 디렉티브를 사용하면 `heroes` 배열의 각 항목을 화면에 표시할 수 있습니다.


<code-example path="displaying-data/src/app/app.component.2.ts" header="src/app/app.component.ts (템플릿)" region="template"></code-example>


이 템플릿에는 목록을 표시하기 위해 `<ul>` 태그와 `<li>` 태그를 사용했습니다.
그리고 `<li>` 태그 안에 사용된 `*ngFor` 는 무언가를 반복할 때 사용하는 디렉티브입니다.
이 디렉티브를 아래 예제처럼 `<li>` 엘리먼트에 사용하면 `<li>` 엘리먼트와 그 하위 엘리먼트를 반복할 수 있습니다.


<code-example path="displaying-data/src/app/app.component.2.ts" header="src/app/app.component.ts (li)" region="li"></code-example>

<div class="alert is-important">

`*ngFor` 를 사용할 때 별표(\*)를 잊지 마세요. 이 표기방식은 템플릿 문법에서도 특히 중요합니다.
좀 더 자세한 설명은 [기본 디렉티브](guide/built-in-directives) 문서의 [ngFor](guide/built-in-directives#ngFor) 섹션을 참고하세요.

</div>

이 코드에서 `*ngFor` 가 지정된 엘리먼트 안의 `hero` 에는 이중 중괄호가 사용되었는데, 이 문법은 템플릿에 데이터를 바인딩하는 방법 중 가장 간단한 방법입니다.
더 자세한 내용은 [템플릿 문법](guide/template-syntax) 문서의 [세부 문법](guide/built-in-directives#microsyntax) 섹션을 참고하세요.

이렇게 코드를 작성하면 Angular는 목록에 있는 항목의 개수만큼 `<li>` 태그를 반복하면서 `hero` 변수를 하나씩 전달합니다.
이 때 전달된 변수는 이중 중괄호 안에서만 유효합니다.

<div class="alert is-helpful">

이 코드에서 `ngFor` 는 배열을 순회하기 위해 사용했습니다. `ngFor`는 배열 뿐 아니라 [이터러블(interable)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols) 객체에도 사용할 수 있습니다.

</div>

여기까지 작성하면 이제 히어로의 목록이 화면에 표시됩니다.

<div class="lightbox">
  <img src="generated/images/guide/displaying-data/hero-names-list.png" alt="ngfor 적용 화면">
</div>


<!--
## Creating a class for the data
-->
## 데이터 클래스 정의하기

<!--
The app's code defines the data directly inside the component, which isn't best practice.
In a simple demo, however, it's fine.

At the moment, the binding is to an array of strings.
In real applications, most bindings are to more specialized objects.

To convert this binding to use specialized objects, turn the array
of hero names into an array of `Hero` objects. For that you'll need a `Hero` class:

<code-example language="sh" class="code-shell">
  ng generate class hero
</code-example>

This command creates the following code.


<code-example path="displaying-data/src/app/hero.ts" header="src/app/hero.ts"></code-example>

You've defined a class with a constructor and two properties: `id` and `name`.

It might not look like the class has properties, but it does.
The declaration of the constructor parameters takes advantage of a TypeScript shortcut.

Consider the first parameter:


<code-example path="displaying-data/src/app/hero.ts" header="src/app/hero.ts (id)" region="id"></code-example>

That brief syntax does a lot:

* Declares a constructor parameter and its type.
* Declares a public property of the same name.
* Initializes that property with the corresponding argument when creating an instance of the class.
-->
지금은 데이터를 그대로 컴포넌트에 표시하지만, 이 방식이 최선은 아닙니다.
간단하게 테스트하는 목적이라면 이대로도 좋지만요.

지금까지는 데이터가 간단한 문자열 배열이기 때문에 이정도로 충분했습니다.
하지만 실제 애플리케이션에서는 복잡하게 정의된 객체를 바인딩해서 사용하는 경우가 대부분입니다.

이제 객체를 바인딩하는 방법을 알아보기 위해, 배열의 항목을 `Hero` 객체로 만들어 봅시다.
Angular CLI를 사용해서 `Hero` 클래스를 생성합니다.

<code-example language="sh" class="code-shell">
  ng generate class hero
</code-example>

그리고 클래스 코드는 다음과 같이 작성합니다.


<code-example path="displaying-data/src/app/hero.ts" header="src/app/hero.ts"></code-example>

이 코드에서는 생성자에서 `id` 프로퍼티와 `name` 프로퍼티를 정의했습니다.

코드를 이렇게 작성하면 클래스에 프로퍼티를 선언하지 않은 것 같지만, 실제로는 프로퍼티가 2개 선언됩니다.
이 문법은 TypeScript 문법으로, 생성자에서 프로퍼티를 간단하게 정의하는 문법입니다.

생성자에 사용된 첫번째 인자를 봅시다:


<code-example path="displaying-data/src/app/hero.ts" header="src/app/hero.ts (id)" region="id"></code-example>

이 간단한 문법이 다음과 같은 역할을 합니다:

* 생성자로 받을 인자와 인자의 타입을 정의합니다.
* 인자와 같은 이름으로 클래스에 public 프로퍼티를 선언합니다.
* 클래스가 생성될 때 생성자로 인자를 받으면 그 값을 해당 프로퍼티에 할당합니다.


<!--
### Using the Hero class
-->
### Hero 클래스 적용하기

<!--
After importing the `Hero` class, the `AppComponent.heroes` property can return a _typed_ array
of `Hero` objects:


<code-example path="displaying-data/src/app/app.component.3.ts" header="src/app/app.component.ts (heroes)" region="heroes"></code-example>



Next, update the template.
At the moment it displays the hero's `id` and `name`.
Fix that to display only the hero's `name` property.


<code-example path="displaying-data/src/app/app.component.3.ts" header="src/app/app.component.ts (template)" region="template"></code-example>


The display looks the same, but the code is clearer.
-->
이제 `AppComponent.heroes` 프로퍼티를 `Hero` 객체 타입으로 다시 정의합니다.

<code-example path="displaying-data/src/app/app.component.3.ts" header="src/app/app.component.ts (heroes)" region="heroes"></code-example>

그리고 템플릿을 수정합니다.
히어로 객체에는 `id` 프로퍼티와 `name` 프로퍼티가 있지만, 지금은 `name` 프로퍼티만 화면에 표시합시다.

<code-example path="displaying-data/src/app/app.component.3.ts" header="src/app/app.component.ts (템플릿)" region="template"></code-example>

앱을 실행해보면 화면에 표시되는 모습은 이전과 같지만, 이제 객체의 어떤 프로퍼티를 참조하는지 명확해졌습니다.


{@a ngIf}

<!--
## Conditional display with NgIf
-->
## 특정 조건을 만족할 때만 표시하기 : ***ngIf***

<!--
Sometimes an app needs to display a view or a portion of a view only under specific circumstances.

Let's change the example to display a message if there are more than three heroes.

The Angular `ngIf` directive inserts or removes an element based on a _truthy/falsy_ condition.
To see it in action, add the following paragraph at the bottom of the template:

<code-example path="displaying-data/src/app/app.component.ts" header="src/app/app.component.ts (message)" region="message"></code-example>

<div class="alert is-important">

Don't forget the leading asterisk (\*) in `*ngIf`. It is an essential part of the syntax.
Read more about `ngIf` and `*` in the [ngIf section](guide/built-in-directives#ngIf) of the [Built-in directives](guide/built-in-directives) page.

</div>


The template expression inside the double quotes,
`*ngIf="heroes.length > 3"`, looks and behaves much like TypeScript.
When the component's list of heroes has more than three items, Angular adds the paragraph
to the DOM and the message appears.
If there are three or fewer items, Angular omits the paragraph, so no message appears.

For more information, see [template expression operators](guide/interpolation#template-expressions).


<div class="alert is-helpful">

Angular isn't showing and hiding the message. It is adding and removing the paragraph element from the DOM. That improves performance, especially in larger projects when conditionally including or excluding
big chunks of HTML with many data bindings.

</div>

Try it out. Because the array has four items, the message should appear.
Go back into <code>app.component.ts</code> and delete or comment out one of the elements from the heroes array.
The browser should refresh automatically and the message should disappear.
-->
어떤 뷰는 특정 조건을 만족할 때만 화면에 표시할 수 있습니다.

히어로 데이터가 3건을 넘어가면 간단한 메시지를 화면에 표시하도록 예제를 수정해 봅시다.

Angular에서 제공하는 `ngIf` 디렉티브는 _참/거짓으로 평가되는_ 조건에 따라 엘리먼트를 DOM에 추가하거나 제거합니다.
`ngIf` 의 동작을 확인하기 위해 템플릿에 다음과 같은 코드를 작성합시다.

<code-example path="displaying-data/src/app/app.component.ts" header="src/app/app.component.ts (message)" region="message"></code-example>

<div class="alert is-important">

`*ngIf` 를 사용할 때 별표(\*) 를 빼먹지 마세요. 이 표기방식은 템플릿 문법에서 특히 중요합니다.
`ngIf` 나 `*` 에 대해 자세히 살펴보려면 [기본 디렉티브](guide/built-in-directives) 문서에 있는 [ngIf 섹션](guide/built-in-directives#ngIf)을 참고하세요.

</div>

템플릿 표현식으로 사용된 `*ngIf="heroes.length > 3"` 구문은 TypeScript 문법처럼 보이고 실제로 동작하는 것도 비슷합니다.
이 코드를 추가하면 컴포넌트에 있는 히어로 목록의 길이가 3를 넘었을 때 DOM에 `<p>` 엘리먼트를 추가하고 이 엘리먼트에 있는 메시지를 표시합니다.
목록의 길이가 3 이하라면 `<p>` 엘리먼트가 DOM에서 제거되면서 메시지도 함께 사라집니다.

자세한 내용은 [템플릿 표현식](guide/interpolation#template-expressions) 문서를 참고하세요.


<div class="alert is-helpful">

`*ngIf` 조건을 만족하지 않아서 엘리먼트가 화면에 표시되지 않을 때, 엘리먼트는 감춰지는 것이 아니고 DOM에서 완전히 제거됩니다.
이 방식은 성능 향상에 도움이 되며, 조건문이 많고 데이터 바인딩을 많이 하는 프로젝트에 더욱 유용합니다.

</div>


앱을 실행해서 결과를 직접 확인해 보세요. 예제에서 다룬 배열은 길이가 4이기 때문에 메시지가 표시됩니다.
그리고 <code>app.component.ts"</code> 코드에서 히어로 배열 코드를 삭제하거나 주석 처리하면, 브라우저가 자동으로 갱신되면서 메시지가 표시되지 않을 것입니다.


<!--
## Summary
-->
## 정리

<!--
Now you know how to use:

* **Interpolation** with double curly braces to display a component property.
* **ngFor** to display an array of items.
* A TypeScript class to shape the **model data** for your component and display properties of that model.
* **ngIf** to conditionally display a chunk of HTML based on a boolean expression.

Here's the final code:
-->
이 문서에서는 이런 내용들을 다뤘습니다:

* 이중 중괄호를 사용해서 컴포넌트 프로퍼티를 템플릿에 넣는 방법을 **문자열 바인딩(interpolation)** 이라고 합니다.
* 배열은 **ngFor** 를 사용해서 순회할 수 있습니다.
* 컴포넌트와 템플릿에 사용할 데이터에 형식을 지정하고 싶다면 TypeScript 클래스를 사용할 수 있습니다.
* **ngIf** 를 사용하면 불리언 값으로 평가되는 조건에 맞춰 HTML 조각을 DOM에 추가하거나 제거할 수 있습니다.

그리고 이 문서에서 다뤘던 코드의 최종 버전은 다음과 같습니다:


<code-tabs>

  <code-pane header="src/app/app.component.ts" path="displaying-data/src/app/app.component.ts" region="final">

  </code-pane>

  <code-pane header="src/app/hero.ts" path="displaying-data/src/app/hero.ts">

  </code-pane>

  <code-pane header="src/app/app.module.ts" path="displaying-data/src/app/app.module.ts">

  </code-pane>

  <code-pane header="main.ts" path="displaying-data/src/main.ts">

  </code-pane>

</code-tabs>