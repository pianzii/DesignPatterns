# DesignPatterns for Java

# 设计模式简介
## 1. 单例模式

内部类实现线程安全的单例模式

## 2. 责任链模式
### 2.1 概念

一个请求有多个对象来处理，这些对象形成一条链，根据条件确定具体由谁来处理，如果当前对象不能处理则传递给该链中的下一个对象，直到有对象处理它为止。
责任链模式通过将请求和处理分离开来，以进行解耦。职责链模式结构的核心在于引入了一个抽象处理者。

职责链模式通过建立一条链来组织请求的处理者，请求将沿着链进行传递，请求发送者无须知道请求在何时、何处以及如何被处理，实现了请求发送者与处理者的解耦。
在软件开发中，如果遇到有多个对象可以处理同一请求时可以应用职责链模式，例如在Web应用开发中创建一个过滤器(Filter)链来对请求数据进行过滤，
在工作流系统中实现公文的分级审批等等，使用职责链模式可以较好地解决此类问题。

### 2.2 模式结构

Handler（抽象处理者）： 定义一个处理请求的接口，提供对后续处理者的引用
ConcreteHandler（具体处理者）： 抽象处理者的子类，处理用户请求，可选将请求处理掉还是传给下家；在具体处理者中可以访问链中下一个对象，以便请求的转发。

### 2.3 优点

降低耦合度，分离了请求与处理，无须知道是哪个对象处理其请求
简化对象的相互连接，仅保持一个指向后者的引用，而不需保持所有候选接受者的引用
扩展容易，新增具体请求处理者，只需要在客户端重新建链即可，无需破坏原代码

### 2.4 纯与不纯

纯：要么承担全部责任，要么将责任推给下家，不允许出现某一个具体处理者对象在承担了一部分或全部责任后又将责任向下传递的情况。
不纯：允许某个请求被一个具体处理者部分处理后再向下传递，或者一个具体处理者处理完某请求后其后继处理者可以继续处理该请求，而且一个请求可以最终不被任何处理者对象所接收。

一个纯的责任链模式要求一个具体的处理者对象只能在两个行为中选择一个：一是承担责任，而是把责任推给下家。不允许出现某一个具体处理者对象在承担了一部分责任后又 把责任向下传的情况。
在一个纯的责任链模式里面，一个请求必须被某一个处理者对象所接收；在一个不纯的责任链模式里面，一个请求可以最终不被任何接收端对象所接收。
纯的责任链模式的实际例子很难找到，一般看到的例子均是不纯的责任链模式的实现。

### 2.5 demo

demo1:

客户端创建了三个处理者对象，并指定第一个处理者对象的下家是第二个处理者对象，第二个处理者对象的下家是第三个处理者对象。
然后客户端将请求传递给第一个处理者对象。

demo2:

1.用Filter模拟处理Request、Response

2.思路细节技巧：

(1)Filter的doFilter方法改为doFilter(Request,Resopnse,FilterChain),有FilterChain引用，为利用FilterChain调用下一个Filter做准备

(2)FilterChain继承Filter,这样，FilterChain既是FilterChain又是Filter,那么FilterChain就可以调用Filter的方法doFilter(Request,Resopnse,FilterChain)

(3)FilterChain的doFilter(Request,Resopnse,FilterChain)中，有index标记了执行到第几个Filter,当所有Filter执行完后request处理后，就会return,以倒序继续执行response处理

## 3. 策略模式

OOP三个基本特征是:封装、继承、多态。通过继承，我们可以基于差异编程，也就是说，对于一个满足我们大部分需求的类，可以创建它的一个子类并只改变我们不期望的那部分。
但是在实际使用中，继承很容易被过度使用，并且过度使用的代价是比较高的，所以我们减少了继承的使用，使用组合或委托代替。策略模式使用委托的方式。

### 3.1 概念

策略模式定义了一系列的算法，并将每一个算法封装起来，而且使他们可以相互替换，让算法独立于使用它的客户而独立变化。

分析下定义，策略模式定义和封装了一系列的算法，它们是可以相互替换的，也就是说它们具有共性，而它们的共性就体现在策略接口的行为上，
另外为了达到最后一句话的目的，也就是说让算法独立于使用它的客户而独立变化，我们需要让客户端依赖于策略接口。

### 3.2 使用场景

1. 针对同一类型问题的多种处理方式，仅仅是具体行为有差别时；
2. 需要安全地封装多种同一类型的操作时；
3. 出现同一抽象类有多个子类，而又需要使用 if-else 或者 switch-case 来选择具体子类时。

### 3.3 策略模式涉及到的角色

1. 环境(Context)角色：持有一个Strategy的引用。

2. 抽象策略(Strategy)角色：这是一个抽象角色，通常由一个接口或抽象类实现。此角色给出所有的具体策略类所需的接口。

3. 具体策略(ConcreteStrategy)角色：包装了相关的算法或行为。

### 3.4 demo

demo1:简单的使用工厂进行封装策略
demo2:使用注解动态配置策略

## 4. 模板方法模式

模板方法模式使用继承的方式。

### 4.1 概念

定义一个算法的骨架，将骨架中的特定步骤延迟到子类中。模板方法模式使得子类可以不改变算法的结构即可重新定义该算法的某些特定步骤。

### 4.2 demo

模板方法模式展示了经典重用的一种形式，通用算法被放在基类中，通过继承在不同的子类中实现该通用算法。
我们通过定义通用类BubbleSorter，把冒泡排序的算法骨架放在基类，然后实现不同的子类分别对int数组、double数组、List集合进行排序。但这样是有代价的，因为继承是非常强的关系，派生类不可避免地与基类绑定在一起了。但如果我现在需要用快速排序而不是冒泡排序来进行排序，
但快速排序却没有办法重用setArray、getLength、needSwap和swap方法了。不过，策略模式提供了另一种可选的方案。

## 5. 工厂模式

工厂模式（Factory）允许我们只依赖于抽象接口就能创建出具体对象的实例，所以在开发中，如果具体类是高度易变的，那么该模式就非常有用。

### 5.1 简单工厂模式

又称为静态工厂方法(Static Factory Method)模式，它属于类创建型模式（同属于创建型模式的还有工厂方法模式，抽象工厂模式，单例模式，建造者模式）。
在简单工厂模式中，可以根据参数的不同返回不同类的实例。简单工厂模式专门定义一个类来负责创建其他类的实例，被创建的实例通常都具有共同的父类。

### 5.2 工厂方法模式

工厂方法模式把简单工厂模式的内部逻辑判断移到了客户端

### 5.3 总结

简单工厂模式和工厂方法模式都封装了对象的创建，它们使得高层策略模块在创建类的实例时无需依赖于这些类的具体实现。但是两种工厂模式之间又有差异：

1. 简单工厂模式：最大的优点在于工厂类包含了必要的判断逻辑，根据客户端的条件动态地实例化相关的类。但这也是它的缺点，当扩展功能的时候，需要修改工厂方法，违反了开放封闭原则

2. 工厂方法模式：符合开放封闭原则，但这带来的代价是扩展的时候要增加相应的工厂类，增加了开发量，而且需要修改客户端代码

## 6. 建造者模式

### 6.1 概念

将一个复杂对象的构建与它的表示分离，使得同样的构建过程可以创建不同的表示

### 6.2 作用

在用户不知道对象的建造过程和细节的情况下就可以直接创建复杂的对象。

1. 用户只需要给出指定复杂对象的类型和内容；
2. 建造者模式负责按顺序创建复杂对象（把内部的建造过程和细节隐藏起来)

### 6.3 解决的问题

1. 方便用户创建复杂的对象（不需要知道实现过程）
2. 代码复用性 & 封装性（将对象构建过程和细节进行封装 & 复用）

### 6.4 实现步骤

1. 指挥者（Director）直接和客户（Client）进行需求沟通；
2. 沟通后指挥者将客户创建产品的需求划分为各个部件的建造请求（Builder）；
3. 将各个部件的建造请求委派到具体的建造者（ConcreteBuilder）；
4. 各个具体建造者负责进行产品部件的构建；
5. 最终构建成具体产品（Product）。

### 6.5 优缺点
在全面解析完后，我来分析下其优缺点：

#### 6.5.1 优点

1. 易于解耦
将产品本身与产品创建过程进行解耦，可以使用相同的创建过程来得到不同的产品。也就说细节依赖抽象。
2. 易于精确控制对象的创建
将复杂产品的创建步骤分解在不同的方法中，使得创建过程更加清晰
3. 易于拓展
增加新的具体建造者无需修改原有类库的代码，易于拓展，符合“开闭原则“。
每一个具体建造者都相对独立，而与其他的具体建造者无关，因此可以很方便地替换具体建造者或增加新的具体建造者，用户使用不同的具体建造者即可得到不同的产品对象。

#### 6.5.2 缺点

建造者模式所创建的产品一般具有较多的共同点，其组成部分相似；如果产品之间的差异性很大，则不适合使用建造者模式，因此其使用范围受到一定的限制。
如果产品的内部变化复杂，可能会导致需要定义很多具体建造者类来实现这种变化，导致系统变得很庞大。

### 6.6 应用场景

1. 需要生成的产品对象有复杂的内部结构，这些产品对象具备共性；

2. 隔离复杂对象的创建和使用，并使得相同的创建过程可以创建不同的产品。

### 6.7 demo

实际项目中的一个案例：系统集成中，需要把客户传过来的数据进行封装处理，然后统一交给后端系统处理，然后集成到自己的业务系统中。
假设集成过来的数据有以下几部分组成：

1. 用户信息（user_profile）
2. 订单计划信息（plan）
3. 订单信息（order）
4. 订单成员信息（member)
5. ...

由于实际业务系统太过复杂，这边就简化为以上四个部分，实际业务中Builder中的方法都有相应的返回类型，这里直接简单些为void，然后实现为控制台输出。

具体实现参见代码部分即可。

# 设计模式综合运用

## 1. 项目背景

在公司的一个实际项目中，需要做一个第三方公司（以下简称GMG）的系统集成工作，把该公司的一些订单数据集成到自己公司平台下，各个订单具有一些共性，但是也有其特有的特征。
经过设计，目前我把订单分为POLICY和BOB类型（暂且这么说吧，反正就是一种订单类型，大家参照着看就OK）。

在订单数据集成到公司平台前，需要对订单数据进行一些必要的业务逻辑校验操作，并且每个订单都有自己的校验逻辑（包含公共的校验逻辑）。
本节介绍的便是整个订单集成系统中的校验逻辑在综合利用设计模式的基础上进行架构设计。

## 2. 校验逻辑

本校验逻辑主要分为四个部分：
1. 校验文件名称（RequestValidator.validateFileInfo）
2. 校验文件内容中的概要部分（RequestValidator.validateSummary）
3. 校验文件内容中的列名称（RequestValidator.validateHeaders）
4. 校验文件内容中的明细（RequestValidator.validateDetails）

其实上面的RequestValidator的实现逻辑最后都是委托给RequestValidationFacade这个门面类进行相应的校验操作。

## 3. 实现细节

### 3.1 domain介绍

主要分为RequestFile和RequestDetail两个domain,RequestFile接收泛型的类型（即RequestFile<T extends RequestDetail>）,
使得其子类能够自动识别相应的RequestDetail的子类。RequestFile为抽象类，定义了以下抽象方法，由子类实现：

```
//由子类实现具体的获取文件明细内容
public abstract List<T> getRequestDetails();
//由子类实现具体的获取workflow的值
public abstract WorkflowEnum getProcessWorkFlow();
//由子类实现文件列字段名列表
public abstract String[] getDetailHeaders();
```

RequestDetail及其子类就是workflow对应文件的明细内容。

### 3.2 WorkflowEnum枚举策略

本例中如下规定：
1. workflow为WorkflowEnum.POLICY对应文件名为：csync_policy_yyyyMMdd_HHmmss_count.txt
2. workflow为WorkflowEnum.BOB对应文件名为：csync_bob_integration_yyyyMMdd_HHmmss_count.txt

以上校验逻辑在AbstractRequestValidation类相应的子类中实现（validateFileName方法），其实这个枚举贯穿整个校验组件，它就是一个针对每个业务流程定义的一个枚举策略。

### 3.3 涉及到的设计模式实现思路

#### 3.3.1 门面模式

在客户端调用程序中，采用门面模式进行统一的入口。此案例中，门面类为RequestValidationFacade，然后各个门面方法的参数均为抽象类
RequestFile，通过RequestFile->getProcessWorkFlow()决定调用AbstractRequestValidation中的哪个子类。AbstractRequestValidation类构造方法
中定义了如下逻辑：

```
requestValidationHandlerMap.put(this.accessWorkflow(),this.accessBeanName());
```

把子类中Spring自动注入的实体bean缓存到requestValidationHandlerMap中，key即为WorkflowEnum枚举值，value为spring bean name,
然后在门面类中可以通过对应的枚举值取得BeanName，进而得到AbstractRequestValidation相应的子类对象，进行相应的校验操作。

注：这边动态调用到AbstractRequestValidation相应的子类对象，其实也是隐藏着【策略模式】的影子。

#### 3.3.2 模版方法模式

在具体的校验逻辑中，用到核心设计模式便是模版方法模式，AbstractRequestValidation抽象类中定义了以下抽象方法：
```
    /**
     * validate the file details
     * @param errMsg
     * @param requestFile
     * @return
     */
    protected abstract StringBuilder validateFileDetails(StringBuilder errMsg,RequestFile requestFile);

    /**
     * validate the file name
     * @param fileName
     * @return
     */
    protected abstract String validateFileName(String fileName);

    /**
     * return the current CSYNC_UPDATE_WORKFLOW.UPDATE_WORKFLOW_ID
     * @return
     */
    protected abstract WorkflowEnum accessWorkflow();

    /**
     * return the current file name's format ,such as: csync_policy_yyyyMMdd_HHmmss_count.txt
     * @return
     */
    protected abstract String accessFileNameFormat();

    /**
     * return the subclass's spring bean name
     * @return
     */
    protected abstract String accessBeanName();

```

以上抽象方法就类似我们常说的钩子函数，由子类实现即可。

#### 3.3.3 责任链模式

在AbstractRequestValidation抽象类中有个抽象方法validateFileDetails，校验的是文件的明细内容中的相应业务规则，此为核心校验，
较为复杂，而且针对每个业务流程，其校验逻辑相差较大，在此处，利用了责任链模式进行处理。

Validator为校验器的父接口，包含两个泛型参数(即：<R extends RequestDetail,F extends RequestFile>)，其实现类可以方便的转换需要校验的文件明细。
```
String doValidate(R detail, F file, ValidatorChain chain) throws BusinessValidationException;
```
该方法含有一个ValidatorChain参数，就自然而然的为该校验器形成一个链条提供便利条件。

ValidatorChain为校验器链，含有两个接口方法：
```
 String doValidate(T requestDetail, F requestFile) throws BusinessValidationException;

 ValidatorChain addValidator(Validator validator, WorkflowEnum workflowId);

```
该处有一个addValidator方法，为ValidatorChain对象添加校验器的方法，返回本身。对应于每个业务流程需要哪些校验器就在此实现即可（即AbstractRequestValidation的子类方法validateFileDetails）。

#### 3.3.4 策略模式

如果单单从上面的校验器实现上来看，如果需要增加一个校验器，就需要在AbstractRequestValidation的子类方法validateFileDetails中添加，然后进行相应的校验操作。这样就会非常的麻烦，没有做到真正的解耦。
此时，策略模式就发挥到了可以动态选择某种校验策略的作用。

AbstractValidatorHandler抽象类持有FileDetailValidatorChain类的对象，并且实现累Spring的一个接口ApplicationListener（是为了Spring容器启动完成的时候自动把相应的校验器加入到校验器链中）。核心就是WorkflowEnum这个策略枚举的作用，在子类可以动态的取得相应的校验器对象。

根据子类提供需要的校验器所在的包名列表和不需要的校验器列表，动态配置出需要的校验器链表。核心实现逻辑如下：

```
private void addValidators() {
    List<Class<? extends Validator>> validators = getValidators();

    validators.forEach((validator) -> {
        String simpleName = validator.getSimpleName();
        String beanName = simpleName.substring(0, 1).toLowerCase() + simpleName.substring(1);

        LOGGER.info("Added validator:{},spring bean name is:{}",simpleName,beanName);

        Validator validatorInstance = ApplicationUtil.getApplicationContext().getBean(beanName,validator);

        fileDetailValidatorChain.addValidator(validatorInstance,getWorkflowId());

    });
}
```
具体实现可以参考代码即可。

该类含有以下几个抽象方法：
```
protected abstract WorkflowEnum getWorkflowId();
/**
 * the package need to be added the validators
 * @return
 */
protected abstract Set<String> getBasePackages();

/**
 * the classes need to be excluded
 * @return
 */
protected abstract Set<Class> excludeClasses();

```
