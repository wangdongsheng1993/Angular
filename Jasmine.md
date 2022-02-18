# Jasmine
Jasmine是一个用于测试JavaScript代码的行为驱动开发框架。它不依赖于浏览器、DOM或任何JavaScript框架。因此，它适用于网站、Node.js 项目或 JavaScript 可以运行的任何地方。

## 兼容
Environment|Supported versions
--|--
Node|12.17+, 14, 16
Safari|14-15
Chrome|Evergreen
Firefox|Evergreen, 91
Edge|Evergreen

## 基本概念
### Suite
> describe(string, function)
describe函数用于对相关spec进行分组，通常每个测试文件在顶层都有一个。
字符串参数用于命名spec集合，并将与spec连接以形成spec的全名。这有助于在大型套件中查找规格。
如果你把它们命名得当，你的规范会读成传统 BDD 风格的完整句子。
### Spec
> it(string, function) 
Specs是通过调用全局Jasmine函数来定义的。
字符串参数是spec的标题，函数是一个spec或测试。 一个spec包含一个或多个测试代码状态的期望。
Jasmine中的期望是对或错的断言。具有所有真实期望的规范是合格的spec。具有一个或多个错误期望的规范是失败的spec。

由于describe和it块是函数，它们可以包含实现测试所需的任何可执行代码。由于JavaScript的scope规则，因此在describe中声明的变量可用于套件内的任何it块。

### Expectations
Expectations是使用函数expect构建的，该函数接受一个值，称为实际值。 它与一个Matcher函数链接，该函数采用预期值。
## Matchers
每个匹配器都实现了实际值和期望值之间的布尔比较。它负责向Jasmine报告预期是真是假。Jasmine将通过或不通过此spec。
Jasmine 包含一组丰富的匹配器 [API Docs](https://jasmine.github.io/api/edge/matchers.html)
## Setup and Teardown
为了帮助测试Suite去除任何重复的设置和拆卸代码，Jasmine 提供了全局的beforeEach、afterEach、beforeAll和afterAll函数。
beforeEach函数在被调用的describe中的每个spec之前被调用一次。在每个规范之后调用一次afterEach 函数。
在describe中的所有spec运行之前，beforeAll函数仅被调用一次。在所有规范完成后调用afterAll函数。
嵌套describe块会沿着依次执行每个 beforeEach 函数。 规范执行后，Jasmine 会类似地遍历afterEach函数。

### Spies
> spyOn(Object, string)
> jasmine.createSpyObj(baseName, methodNames, propertyNames)  创建一个包含多个Spies的对象
字符串参数是对象的函数名。
Spies可以存根任何函数并跟踪对它的调用和所有参数。Spies只存在于定义它的describe或it块中，并且将在每个spec之后被删除。有与Spies互动的特殊Matchers。
可以定义spies在使用and调用时将执行的操作。

### this
在 beforeEach、it和afterEach之间共享变量是通过this关键字。每个spec的beforeEach/it/afterEach都有this作为相同的空对象，该对象会在下一个规范的beforeEach/it/afterEach设置回空。
```
  describe("A spec", function() {
    beforeEach(function() {
      this.foo = 0;
    });
    
    it("can use the `this` to share state", function() {
      expect(this.foo).toEqual(0);
      this.bar = "test pollution?";
    });
    
    it("prevents test pollution by having an empty `this` created for the next spec", function() {
      expect(this.foo).toEqual(0);
      expect(this.bar).toBe(undefined);
    });
  });
```

### xdescribe
禁用Suites。这些Suite和其中的任何spec在运行时都会被跳过，因此它们的结果将显示为pending。

### xit
Pending Spec不会运行，但它们将在结果中显示为pending。
调用pending函数也可以使得spec标记为pending。

### Jasmine Clock
Jasmine Clock可用于测试与时间相关的代码。
完成后请务必卸载时钟以恢复原始功能。
```
describe("Manually ticking the Jasmine Clock", function() {
  var timerCallback;

  beforeEach(function() {
    timerCallback = jasmine.createSpy("timerCallback");
    jasmine.clock().install();
  });

  afterEach(function() {
    jasmine.clock().uninstall();
  });

  it("causes a timeout to be called synchronously", function() {
    setTimeout(function() {
      timerCallback();
    }, 100);

    expect(timerCallback).not.toHaveBeenCalled();

    jasmine.clock().tick(101);

    expect(timerCallback).toHaveBeenCalled();
  });

  it("causes an interval to be called synchronously", function() {
    setInterval(function() {
      timerCallback();
    }, 100);

    expect(timerCallback).not.toHaveBeenCalled();

    jasmine.clock().tick(101);
    expect(timerCallback.calls.count()).toEqual(1);

    jasmine.clock().tick(50);
    expect(timerCallback.calls.count()).toEqual(1);

    jasmine.clock().tick(50);
    expect(timerCallback.calls.count()).toEqual(2);
  });

  describe("Mocking the Date object", function(){
    it("mocks the Date object and sets it to a given time", function() {
      var baseTime = new Date(2013, 9, 23);

      jasmine.clock().mockDate(baseTime);

      jasmine.clock().tick(50);
      expect(new Date().getTime()).toEqual(baseTime.getTime() + 50);
    });
  });
});
```

