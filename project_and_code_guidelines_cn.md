# 1. 项目

## 1.1 项目结构

新的项目应该遵循Android Gradle 项目结构，详细请查询官方文档[Android Gradle plugin user guide](http://tools.android.com/tech-docs/new-build-system/user-guide#TOC-Project-Structure)。 [ribot Boilerplate](https://github.com/ribot/android-boilerplate)项目就是一个很好的例子。

## 1.2 文件命名

### 1.2.1 类文件
类的命名方法应该为驼峰式 [UpperCamelCase](http://en.wikipedia.org/wiki/CamelCase)。

对于集成至安卓系统组件的类来说，其类名应该以相应组件名称结尾。比如：`SignInActivity`, `SignInFragment`, `ImageUploaderService`, `ChangePasswordDialog`。


### 1.2.2 资源文件

资源文件命名采取小写字母+下划线方式。

#### 1.2.2.1 Drawable 文件

drawable文件，命名约定：


| Asset Type   | Prefix            |		Example               |
|--------------| ------------------|-----------------------------|
| Action bar   | `ab_`             | `ab_stacked.9.png`          |
| Button       | `btn_`	            | `btn_send_pressed.9.png`    |
| Dialog       | `dialog_`         | `dialog_top.9.png`          |
| Divider      | `divider_`        | `divider_horizontal.9.png`  |
| Icon         | `ic_`	            | `ic_star.png`               |
| Menu         | `menu_	`           | `menu_submenu_bg.9.png`     |
| Notification | `notification_`	| `notification_bg.9.png`     |
| Tabs         | `tab_`            | `tab_pressed.9.png`         |

有关icons的命名约定请查阅[Android iconography guidelines](http://developer.android.com/design/style/iconography.html)。


| Asset Type                      | Prefix             | Example                      |
| --------------------------------| ----------------   | ---------------------------- |
| Icons                           | `ic_`              | `ic_star.png`                |
| Launcher icons                  | `ic_launcher`      | `ic_launcher_calendar.png`   |
| Menu icons and Action Bar icons | `ic_menu`          | `ic_menu_archive.png`        |
| Status bar icons                | `ic_stat_notify`   | `ic_stat_notify_msg.png`     |
| Tab icons                       | `ic_tab`           | `ic_tab_recent.png`          |
| Dialog icons                    | `ic_dialog`        | `ic_dialog_info.png`         |

selector 文件的命名约定：

| State	       | Suffix          | Example                     |
|--------------|-----------------|-----------------------------|
| Normal       | `_normal`       | `btn_order_normal.9.png`    |
| Pressed      | `_pressed`      | `btn_order_pressed.9.png`   |
| Focused      | `_focused`      | `btn_order_focused.9.png`   |
| Disabled     | `_disabled`     | `btn_order_disabled.9.png`  |
| Selected     | `_selected`     | `btn_order_selected.9.png`  |


#### 1.2.2.2 布局文件

布局文件命名需要匹配安卓组件的名字，但需要以组件名字开头。举个栗子，如果我们要为`SignInActivity`创建一个布局文件，其命名应该为`activity_sign_in.xml`。
>如果不同模块有相同类名（少数情况），这里可加上模块前缀。

当我们为`Adapter`创建一个布局时，就和上面的约定一点轻微不同。比如填充一个列表`ListView`，在这种情况下，布局文件应该以`item_`开头。

以上规则并不能适用于所有情况。比如，当创建一个布局文件，它只是另外一个布局文件的一部分时，这种情况下，我们应该加上`partial_`前缀。

| Component        | Class Name             | Layout Name                   |
| ---------------- | ---------------------- | ----------------------------- |
| Activity         | `UserProfileActivity`  | `activity_user_profile.xml`   |
| Fragment         | `SignUpFragment`       | `fragment_sign_up.xml`        |
| Dialog           | `ChangePasswordDialog` | `dialog_change_password.xml`  |
| AdapterView item | ---                    | `item_person.xml`             |
| Partial layout   | ---                    | `partial_stats_bar.xml`       |


#### 1.2.2.3 Menu 文件

menu文件命名和layout文件类似。应该和安卓组件匹配。举个栗子，如果我们为`UserActivity`创建一个menu文件，那么其命名应该是`activity_user.xml`。

一个好的做法是:不要包含`menu`这个单词，因为文件已经在`menu`这个文件夹下。

#### 1.2.2.4 Values 文件

在values文件夹下的文件应该是复数形式，比如： `strings.xml`, `styles.xml`, `colors.xml`, `dimens.xml`, `attrs.xml`。

# 2 代码规范

## 2.1 Java 语言规则

### 2.1.1 请勿忽略异常

你一定不能这样做:

```java
void setServerPort(String value) {
    try {
        serverPort = Integer.parseInt(value);
    } catch (NumberFormatException e) {
		// do something
	}
}
```

_你可能会认为这个异常情况永远不会出现，或者没必要处理它。忽略异常就像挖了个坑在你的代码里，没准哪天别人就掉进去了。你必须以某种原则性的方法处理所有异常。具体处理根据情况而有所不同。_- ([Android code style guidelines](https://source.android.com/source/code-style.html))

更多资料请查询官网 [here](https://source.android.com/source/code-style.html#dont-ignore-exceptions)。

### 2.1.2 不要只捕获基类异常（Throwable、Exception）

你不应该这样写:

```java
try {
    someComplicatedIOFunction();        // may throw IOException
    someComplicatedParsingFunction();   // may throw ParsingException
    someComplicatedSecurityFunction();  // may throw SecurityException
    // phew, made it all the way
} catch (Exception e) {                 // I'll just catch all exceptions
    handleError();                      // with one generic handler!
}
```

更多资料请查询官网 [here](https://source.android.com/source/code-style.html#dont-catch-generic-exception)

### 2.1.3 不要使用 finalizers

_我们不使用finalizers。因为没办法保证一个finalizers何时被调用，甚至不知它是否会被调用。大部分情况下，你能做的只是在一个finalizers里处理好异常。如果确实需要它，那就定义一个`close()`方法（或者类似）并且附上正确的使用文档。以`InputStream`为例，在这种情况下它是合适的，但是不需要打印一个简短的finalizers日志消息，只要它不会淹没日志。_ - ([Android code style guidelines](https://source.android.com/source/code-style.html#dont-use-finalizers))


### 2.1.4 全路径导包

This is bad: `import foo.*;`

This is good: `import foo.Bar;`

查询更新消息 [here](https://source.android.com/source/code-style.html#fully-qualify-imports)

## 2.2 Java 代码风格

### 2.2.1 字段的定义和命名

所有字段应该定义在 __文件顶部__ 并且应该遵守以下命名规则。

* Private, non-static field names start with __m__.
* 私有， 非静态字段名称以__m__ 开头。
* Private, static field names start with __s__.
* 私有， 静态字段名称以__s__ 开头。
* Other fields start with a lower case letter.
* 其它字段小写字母开头。
* Static final fields (constants) are ALL_CAPS_WITH_UNDERSCORES.
* 静态final类型（常量）定义为全大写+下划线，比如：HAPPY_NEW_YEAR。

代码示例:

```java
public class MyClass {
    public static final int SOME_CONSTANT = 42;
    public int publicField;
    private static MyClass sSingleton;
    int mPackagePrivate;
    private int mPrivate;
    protected int mProtected;
}
```

### 2.2.3 Treat acronyms as words 字母缩写

| Good           | Bad            |
| -------------- | -------------- |
| `XmlHttpRequest` | `XMLHTTPRequest` |
| `getCustomerId`  | `getCustomerID`  |
| `String url`     | `String URL`     |
| `long id`        | `long ID`        |

### 2.2.4 空格缩进（建议code_style配合ide使用）

代码块的缩进:

```java
if (x == 1) {
    x++;
}
```

换行的缩进:

```java
Instrument i =
        someLongExpression(that, wouldNotFit, on, one, line);
```

### 2.2.5 使用标准的括号风格（建议code_style配合ide使用）

括号在它们之前与代码保持同一行。

```java
class MyClass {
    int func() {
        if (something) {
            // ...
        } else if (somethingElse) {
            // ...
        } else {
            // ...
        }
    }
}
```

括号应该包围语句， 除非条件和身体 适应在同一行。

如果条件和身体适应同一行，同时这行的长度小于最大行长，那么括号不不需要，比如：

```java
if (condition) body();
```

This is __bad__:

```java
if (condition)
    body();  // bad!
```

### 2.2.6 Annotations 注解

#### 2.2.6.1 Annotations practices 注解实践

根据安卓代码风格指南，在Java中的一些预定义注解的标准做法是：

* `@Override`: The @Override annotation __must be used__ whenever a method overrides the declaration or implementation from a super-class. For example, if you use the @inheritdocs Javadoc tag, and derive from a class (not an interface), you must also annotate that the method @Overrides the parent class's method.
* `@Override`：在方法重写申明或者从父类实现时必须使用 @Override注解。举个栗子，如果你使用@inheritdocs Javadoc 标签，并且定义在一个类中（非接口），你必须也在父类中使用@Override来注解该方法。

* `@SuppressWarnings`: The @SuppressWarnings annotation should only be used under circumstances where it is impossible to eliminate a warning. If a warning passes this "impossible to eliminate" test, the @SuppressWarnings annotation must be used, so as to ensure that all warnings reflect actual problems in the code.
* `@SuppressWarnings`: 在不可能消除警告的情况下应该使用@SuppressWarnings注解。如果警告通过这个“不可能消除”测试，那么@SuppressWarnings注解必须被使用，以便确保所有警告反映代码中的实际问题。

更多关于注解指导文档 [here](http://source.android.com/source/code-style.html#use-standard-java-annotations)。

#### 2.2.6.2 Annotations 样式

__Classes, Methods and Constructors__

当注解被应用到类，方法，构造函数，它们在注释文档块之后列出，并且格式为__一个注解占一行__。

```java
/* This is the documentation block about the class */
@AnnotationA
@AnnotationB
public class MyAnnotatedClass { }
```

__Fields__

当注解应用于字段时，它们应该和字段在__同一行被列出__，超过单行最大长度情况除外。

```java
@Nullable @Mock DataManager mDataManager;
```

### 2.2.7 Limit variable scope 限制变量作用域

局部变量的作用范围应该控制到最小（Effective Java Item 29）。通过这样做，你提高了代码的可读性和可维护性，减少了错误的可能性。每个变量都应该在语句块内被声明和使用。

局部变量应该在第一次使用时完成声明。几乎每一个局部变量声明必须包含一个初始化。如果你还没有足够的信息来初始化一个变量是明智的，你应该推迟直到你要使用时在完成声明。- ([Android code style guidelines](https://source.android.com/source/code-style.html#limit-variable-scope))


### 2.2.8 Order import statements 导包语句的顺序

If you are using an IDE such as Android Studio, you don't have to worry about this because your IDE is already obeying these rules. If not, have a look below.

如果你使用的是类似Android studio的开发工具，你不需要担心这个，因为IDE已经遵守了这些规则。如果不是，请看以下内容。

The ordering of import statements is:

1. Android imports
2. Imports from third parties (com, junit, net, org)
3. java and javax
4. Same project imports

To exactly match the IDE settings, the imports should be:

* Alphabetically ordered within each grouping, with capital letters before lower case letters (e.g. Z before a).
* There should be a blank line between each major grouping (android, com, junit, net, org, java, javax).

更多信息 [here](https://source.android.com/source/code-style.html#limit-variable-scope)

### 2.2.9 Logging guidelines 日志

使用`Log`类提供的打印log的方法，你可以打印错误消息或者其它消息，这样有助于开发者识别和定位问题：

* `Log.v(String tag, String msg)` (verbose)
* `Log.d(String tag, String msg)` (debug)
* `Log.i(String tag, String msg)` (information)
* `Log.w(String tag, String msg)` (warning)
* `Log.e(String tag, String msg)` (error)

作为一般规则，我们使用一个类名作为tag并且我们把它定义为`static final`类型的字段放在文件顶部。举个栗子：

```java
public class MyClass {
    private static final String TAG = MyClass.class.getSimpleName();

    public myMethod() {
        Log.e(TAG, "My error message");
    }
}
```
release版本__必须__关掉VERBOSE 和 DEBUG 日志。我们也建议关掉INFORMATION, WARNING 和 ERROR 日志，但如果你想在release版本也能收集一些有用的日志以便定位问题，在这种情况下你可能会保留它们。只要你决定保留它们（在release版本），你就必须保证它们不会泄露敏感信息，比如邮箱地址，用户id，等等。

To only show logs on debug builds:

```java
if (BuildConfig.DEBUG) Log.d(TAG, "The value of x is " + x);
```

### 2.2.10 Class member ordering 类成员排序

这个没有官方标准，但是使用__逻辑__和__一致的__（using a __logical__ and __consistent__）排序会提高代码的可学习型和可读性。这里推荐使用以下顺序：

1. Constants
2. Fields
3. Constructors
4. Override methods and callbacks (public or private)
5. Public methods
6. Private methods
7. Inner classes or interfaces

Example:

```java
public class MainActivity extends Activity {

	private String mTitle;
    private TextView mTextViewTitle;

    public void setTitle(String title) {
    	mTitle = title;
    }

    @Override
    public void onCreate() {
        ...
    }

    private void setUpView() {
        ...
    }

    static class AnInnerClass {

    }

}
```

如果你的类继承至__安卓系统组件__，比如Activity、Fragment，这是一个很好的例子来排序其实现父类的__生命周期__方法。举个栗子，如果你有一个Activity，并且实现了`onCreate()`, `onDestroy()`, `onPause()` and `onResume()`,那么，它们的顺序应该是：

```java
public class MainActivity extends Activity {

	//Order matches Activity lifecycle
    @Override
    public void onCreate() {}

    @Override
    public void onResume() {}

    @Override
    public void onPause() {}

    @Override
    public void onDestroy() {}

}
```

### 2.2.11 Parameter ordering in methods 方法参数的排序

开发安卓过程中，定义一个方法其带了`Context`参数是很常见的。如果你写了一个这样的方法，那么 __Context__ 必须是第一个参数。

另一种情况，当定义个 __callback__ 接口参数时，应该将其放在**最后一个**参数位置。

Examples:

```java
// Context always goes first
public User loadUser(Context context, int userId);

// Callbacks always go last
public void loadUserAsync(Context context, int userId, UserCallback callback);
```

### 2.2.13 String constants, naming, and values 字符串常量、命名、赋值

许多Android API都是提供了键值对的方法，比如`SharedPreferences`、`Bundle`、`Intent`类，所以很有可能你不得不写很多字符串常量，即使是一个小应用程序。

当使用这些组件时，你__必须__定义一些`static final`类型的字段，所以应该为其加一个前缀来区分。

| Element            | Field Name Prefix |
| -----------------  | ----------------- |
| SharedPreferences  | `PREF_`             |
| Bundle             | `BUNDLE_`           |
| Fragment Arguments | `ARGUMENT_`         |
| Intent Extra       | `EXTRA_`            |
| Intent Action      | `ACTION_`           |

请注意，尽管Fragment的arguments- `Fragment.getArguments()` -也是一个Bundle。但是，因为它是一个相当常见的Bundles,所以我们就为其定义一个不同的前缀。

Example:

```java
// Note the value of the field is the same as the name to avoid duplication issues
static final String PREF_EMAIL = "PREF_EMAIL";
static final String BUNDLE_AGE = "BUNDLE_AGE";
static final String ARGUMENT_USER_ID = "ARGUMENT_USER_ID";

// Intent-related items use full package name as value
static final String EXTRA_SURNAME = "com.myapp.extras.EXTRA_SURNAME";
static final String ACTION_OPEN_USER = "com.myapp.action.ACTION_OPEN_USER";
```

### 2.2.14 参数在 Fragments 和 Activities

当通过`Intent`或者`Bundle`将数据传入一个`Activity `或者 `Fragment`时，那些key值的字符串必须遵循上一章节（2.2.13）定义的规则。

当一个`Activity `或者 `Fragment`期望参数时，它应该提供一个`public static`类型的，可创建 `Intent` or `Fragment`的便利方法。

在Activity中使用的情况下，该方法名通常被定义为`getStartIntent()`:

```java
public static Intent getStartIntent(Context context, User user) {
	Intent intent = new Intent(context, ThisActivity.class);
	intent.putParcelableExtra(EXTRA_USER, user);
	return intent;
}
```

对于Fragments 它被命名为`newInstance()`并且使用正确的参数来创建Fragment：

```java
public static UserFragment newInstance(User user) {
	UserFragment fragment = new UserFragment;
	Bundle args = new Bundle();
	args.putParcelable(ARGUMENT_USER, user);
	fragment.setArguments(args)
	return fragment;
}
```

__Note 1__: 这些方法应该放在类的顶部，在方法`onCreate()`前。

__Note 2__: 如果我们在顶部声明了这些方法，那些用于extras和arguments的keys(常量字符串)应该定义为`private`，因为它们已经不需要对外部类提供访问权限了。

### 2.2.15 Line length limit 行长度限制

代码行不应该超过**100个字符**。如果超过了这个限制，通常有两种方式来减小其长度。

* 提取局部变量或方法（优选）。
* 应用换行将一行分割成多行。


有两个__例外__，可能有超过100的行：
* 不能拆分的行，例如注释中的长URL。
* 包和导入语句。

#### 2.2.15.1 Line-wrapping strategies 换行策略

没有一个确切的公式来解释如何换行，并且有很多不同的解决方案也是有效的。然而，有几个规则，可以适用于一些常见的情况。

__操作符__

当换行的地方有操作符时，换行应该在操作符__之前__。举例：

```java
int longName = anotherVeryLongVariable + anEvenLongerOne - thisRidiculousLongOne
        + theFinalOne;
```

__赋值操作符__
赋值操作符是一个例外。它的换行操作应该在赋值操作符__之后__。

```java
int longName =
        anotherVeryLongVariable + anEvenLongerOne - thisRidiculousLongOne + theFinalOne;
```

__方法链__

当更多的方法被串连在同一行时 - 比如适用Builders - 所有的方法调用都在同一行，这时，换行应该在`.`之前。
```java
Picasso.with(context).load("http://ribot.co.uk/images/sexyjoe.jpg").into(imageView);
```

```java
Picasso.with(context)
        .load("http://ribot.co.uk/images/sexyjoe.jpg")
        .into(imageView);
```

__很多参数的情况__

当一个方法有很多参数或者它的参数很长，我们应该在每一个逗号`,`后换行。

```java
loadPicture(context, "http://ribot.co.uk/images/sexyjoe.jpg", mImageViewProfilePicture, clickListener, "Title of the picture");
```

```java
loadPicture(context,
        "http://ribot.co.uk/images/sexyjoe.jpg",
        mImageViewProfilePicture,
        clickListener,
        "Title of the picture");
```

### 2.2.16 RxJava 链式风格

Rx 链式操作应该换行。每一个操作必须在新的一行，换行操作应该在`.`之前。

```java
public Observable<Location> syncLocations() {
    return mDatabaseHelper.getAllLocations()
            .concatMap(new Func1<Location, Observable<? extends Location>>() {
                @Override
                 public Observable<? extends Location> call(Location location) {
                     return mRetrofitService.getLocation(location.id);
                 }
            })
            .retry(new Func2<Integer, Throwable, Boolean>() {
                 @Override
                 public Boolean call(Integer numRetries, Throwable throwable) {
                     return throwable instanceof RetrofitError;
                 }
            });
}
```
## 2.3 XML 样式规则

### 2.3.1 自关闭标签

当一个XML元素没有子元素时，你**必须**使用自关闭标签。

This is __good__:

```xml
<TextView
	android:id="@+id/text_view_profile"
	android:layout_width="wrap_content"
	android:layout_height="wrap_content" />
```

This is __bad__ :

```xml
<!-- Don\'t do this! -->
<TextView
    android:id="@+id/text_view_profile"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content" >
</TextView>
```


### 2.3.2 资源文件命名

资源的IDs和names使用__小写+下划线__。

#### 2.3.2.1 ID 命名

IDs 应该使用组件名称的小写字母+下划线作为前缀。举个栗子：


| Element            | Prefix            |
| -----------------  | ----------------- |
| `TextView`           | `text_`             |
| `ImageView`          | `image_`            |
| `Button`             | `button_`           |
| `Menu`               | `menu_`             |

Image view example:

```xml
<ImageView
    android:id="@+id/image_profile"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content" />
```

Menu example:

```xml
<menu>
	<item
        android:id="@+id/menu_done"
        android:title="Done" />
</menu>
```

#### 2.3.2.2 Strings 字符资源

String字符命名以其定义的模块名为前缀。比如，`registration_email_hint` 或者 `registration_name_hint`。如果一个String不属于任何模块，那你应该遵守如下规则：


| Prefix             | Description                           |
| -----------------  | --------------------------------------|
| `error_`             | An error message                      |
| `msg_`               | A regular information message         |
| `title_`             | A title, i.e. a dialog title          |
| `action_`            | An action such as "Save" or "Create"  |



#### 2.3.2.3 Styles and Themes 样式和主题

Unless the rest of resources, style names are written in __UpperCamelCase__.
除非依赖资源，样式命名遵循**驼峰式**。

### 2.3.3 Attributes ordering 属性定义顺序

As a general rule you should try to group similar attributes together. A good way of ordering the most common attributes is:
作为一般规则，你应该尝试将类似的属性组合在一起。一个常见的排序方法：

1. View Id
2. Style
3. Layout width and layout height
4. Other layout attributes, sorted alphabetically
5. Remaining attributes, sorted alphabetically

## 2.4 Tests style rules 测试样式规则

### 2.4.1 Unit tests 单元测试

Test classes should match the name of the class the tests are targeting, followed by `Test`. For example, if we create a test class that contains tests for the `DatabaseHelper`, we should name it `DatabaseHelperTest`.
测试类名应该和被测试的目标类匹配，在末尾加上`Test`。

Test methods are annotated with `@Test` and should generally start with the name of the method that is being tested, followed by a precondition and/or expected behaviour.

* Template: `@Test void methodNamePreconditionExpectedBehaviour()`
* Example: `@Test void signInWithEmptyEmailFails()`

Precondition and/or expected behaviour may not always be required if the test is clear enough without them.

Sometimes a class may contain a large amount of methods, that at the same time require several tests for each method. In this case, it's recommendable to split up the test class into multiple ones. For example, if the `DataManager` contains a lot of methods we may want to divide it into `DataManagerSignInTest`, `DataManagerLoadUsersTest`, etc. Generally you will be able to see what tests belong together because they have common [test fixtures](https://en.wikipedia.org/wiki/Test_fixture).

### 2.4.2 Espresso tests

Every Espresso test class usually targets an Activity, therefore the name should match the name of the targeted Activity followed by `Test`, e.g. `SignInActivityTest`

When using the Espresso API it is a common practice to place chained methods in new lines.

```java
onView(withId(R.id.view))
        .perform(scrollTo())
        .check(matches(isDisplayed()))
```

# License

jiantao 2017
