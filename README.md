# 创建型模式
## 简单工厂模式(Simple Factory Pattern)
	又称为静态工厂方法模式，它属于类创建型模式。在简单工厂模式中，可以根	据参数的不同返回不同类的实例。简单工厂模式专门定义一个类来负责创建其 	他类的实例，被创建的实例通常都具有共同的父类。
### 模式结构
- Factory  工厂角色：负责实现创建所有实例的内部逻辑
- Product 抽象产品角色：所创建的所有对象的父类，负责描述所有实例所共有的公共接口
- ConcreteProduct 具体产品角色：是创建的目标，所有创建的对象都充当这个角色的某个具体类的实例

``` c++

Product* Factory::createProduct(string proname){
	if ( "A" == proname )
	{
		return new ConcreteProductA();
	}
	else if("B" == proname)
	{
		return new ConcreteProductB();
	}
	return  NULL;
}
```

### 模式分析
- 将对象的创建和对象本身业务分离可以降低系统的耦合度，使得两者修改起来都相对容易
- 在调用工厂类的工厂方法时，由于工厂方法是静态方法，使用起来很方便，可通过类名直接调用，而且只需要传入一个简单的参数即可，在实际开发中，还可以在调用时将所传入的参数保存在XML等格式的配置文件中，修改参数时无须修改任何源代码
- 简单工厂模式最大的问题在于工厂类的职责相对过重，增加新的产品需要修改工厂类的判断逻辑，这一点与开闭原则是相违背的
- 简单工厂模式的要点在于：当你需要什么，只需要传入一个正确的参数，就可以获取你所需要的对象，而无须知道其创建细节

### 优缺点
#### 优点
- 工厂类决定在什么时候创建哪一个产品类的实例，客户端可以免除直接创建产品对象的责任，而仅仅“消费”产品；简单工厂模式通过这种做法实现了对责任的分割，它提供了专门的工厂类用于创建对象
- 只需要知道产品类对应的参数即可
- 引入配置文件，可以提高系统的灵活性

#### 缺点
- 集中了所有产品创建逻辑，一旦不能正常工作，整个系统都要受到影响
- 会增加系统中类的个数，在一定程度上增加了系统的复杂度
- 扩展困难，每次增加新产品都需要修改工厂逻辑，产品类型较多时就很复杂
- 静态工厂方法，造成工厂角色无法形成基于继承的等级结构

### 适用环境
- 工厂类负责创建的对象比较少
- 客户端只知道传入工厂类的参数，对于创建对象漠不关心，客户端既不关心创建细节，甚至连类名都不需要记住

## 工厂方法模式
	又称为工厂模式，也叫虚拟构造器模式或者多态工厂模式，它属于类创建型模式。在工	厂方法模式中，工厂父类负责定义创建产品对象的公共接口，而工厂子类则负责生成具	体的产品对象，这样的目的是将产品类的实例化操作延迟到工厂子类中完成，即通过工	厂子类来确定究竟使用确定究竟应该实例化哪个具体产品类
### 模式结构
- Product：抽象产品
- ConcreteProduct：具体产品
- Factory：抽象工厂
- ConcreteFactory：具体工厂

```  c++
Product* ConcreteFactory::factoryMethod(){

	return  new ConcreteProduct();
}

int main(int argc, char *argv[])
{
	Factory * fc = new ConcreteFactory();
	Product * prod = fc->factoryMethod();
	prod->use();
	
	delete fc;
	delete prod;
	
	return 0;
}
```

### 模式分析
	保持了简单工厂模式的有点，而且克服了它的缺点。核心的工厂类不再负责所有产品的 	创建，而是将具体创建工作交给子类去做。这个核心类仅仅负责给出具体工厂必须实现	的接口，而不负责哪个产品类被实例化这种细节，这使得工厂方法模式可以允许系统在	不修改工厂角色的情况下引进新产品
### 优缺点
#### 优点
- 工厂方法用来创建客户所需要的产品，同时还向客户隐藏了哪种具体产品类将被实例化这一细节，用户只需要关心所需产品对应的工厂，无须关心创建细节，甚至无须知道具体产品类的类名
- 基于工厂角色和产品角色的多态性设计师工厂方法模式的关键。它能够使工厂可以自主确定创建何种产品对象，而如何创建这个对象的细节则完全被封装在具体工厂内部。工厂方法模式之所以又被称为多态工厂模式，是因为所有的具体工厂类都具有同一抽象父类
- 系统中加入新产品时，无须修改抽象工厂和抽象产品提供的接口，无须修改客户端，也无须修改其他的具体工厂和具体产品，而只要添加一个具体工厂和具体产品就可以了。这样，系统的可拓展性也很棒

#### 缺点
- 在添加新产品时，需要编写新的具体产品类，而且还要提供与之对应的具体工厂类，系统中类的个数增加，在一定程度上增加了系统的复杂性，带来额外的开销
- 需要引入抽象层，在客户端代码中均使用抽象层进行定义，增加了系统的抽象性和理解难度，且在实现时可能需要用到DOM，反射等技术，增加了系统的实现难度

### 适用环境
- 一个类不知道它所需要的对象的类
- 一个类通过子类来指定创建哪个对象
- 将创建对象的任务委托给多个工厂子类中的某一个，客户端在使用时可以无须关心是哪一个工厂子类创建产品子类，需要时再动态指定

## 抽象工厂模式
	提供一个创建一系列相关或者相互依赖对象的接口，而无须指定它们具体的类。
### 模式结构
- AbstractFactory：抽象工厂
- ConcreteFactory：具体工厂
- AbstractProduct：抽象产品
- Product：具体产品

```  c++

int main(int argc, char *argv[])
{
	AbstractFactory * fc = new ConcreteFactory1();
	AbstractProductA * pa =  fc->createProductA();
	AbstractProductB * pb = fc->createProductB();
	pa->use();
	pb->eat();
	
	AbstractFactory * fc2 = new ConcreteFactory2();
	AbstractProductA * pa2 =  fc2->createProductA();
	AbstractProductB * pb2 = fc2->createProductB();
	pa2->use();
	pb2->eat();
}

AbstractProductA * ConcreteFactory1::createProductA(){
	return new ProductA1();
}


AbstractProductB * ConcreteFactory1::createProductB(){
	return new ProductB1();
}

void ProductA1::use(){
	cout << "use Product A1" << endl;
}
```
### 优缺点
#### 优点
- 隔离了具体类的生成，更换一个具体工厂就变得相对容易。应用抽象工厂模式可以实现高内聚低耦合的设计目的
- 当一个产品族中的多个对象被设计成一起工作时，它能够保证客户端始终只使用同一个产品组中的对象。
- 增加新的具体工厂和产品族非常方便

#### 缺点
- 难以扩展抽象工厂来生产新种类的产品
- 增加新的工厂和产品族容易，增加新的产品等级结构麻烦

### 适用环境
- 不应当依赖于产品类实例如何被创建、组合和表达的细节
- 系统中有多于一个的产品族，而每次只使用其中一个产品族
- 属于同一产品族的产品将在一起使用
- 系统提供一个产品类的库，所有产品以同样的接口出现

## 建造者模式
	将一个复杂对象的构建和它的表示分离，使得这样的构建过程可以创建不同的表示。
### 模式结构
- Builder：抽象建造者
- ConcreteBuilder：具体建造者
- Director：指挥者
- Product：产品角色

``` c++
int main(int argc, char *argv[])
{
	ConcreteBuilder * builder = new ConcreteBuilder();
	Director  director;
	director.setBuilder(builder);
	Product * pd =  director.constuct();
	pd->show();
	
	delete builder;
	delete pd;
	return 0;
}

ConcreteBuilder::ConcreteBuilder(){

}



ConcreteBuilder::~ConcreteBuilder(){

}

void ConcreteBuilder::buildPartA(){
	m_prod->setA("A Style "); //不同的建造者，可以实现不同产品的建造  
}


void ConcreteBuilder::buildPartB(){
	m_prod->setB("B Style ");
}


void ConcreteBuilder::buildPartC(){
	m_prod->setC("C style ");
}

Director::Director(){
}

Director::~Director(){
}

Product* Director::constuct(){
	m_pbuilder->buildPartA();
	m_pbuilder->buildPartB();
	m_pbuilder->buildPartC();
	
	return m_pbuilder->getResult();
}


void Director::setBuilder(Builder* buider){
	m_pbuilder = buider;
}
```
### 模式分析
	引入了Director，隔离了客户与生产过程，并控制产品的生产过程。在客户端代码中，	无须关心产品对象的具体组装过程，只需确定具体建造者的类型即可
### 优缺点
#### 优点
- 客户端不必知道产品内部组成的细节，将产品本身与产品的创建过程解耦，使得相同的创建过程可以创建不同的产品对象
- 用户使用不同的具体建造者即可得到不同的产品对象
- 可以更加精细地控制产品的创建过程
- 增加新的具体建造者无须修改原有类库的代码，指挥者类针对抽象建造者类编程，系统扩展方便

#### 缺点
- 如果产品差异性很大不适合
- 产品内部变化复杂不适合

### 适用环境
- 需要生产的产品对象有复杂的内部结构
- 需要生产的产品对象的属性相互依赖
- 对象的创建过程独立于创建该对象的类。
- 隔离复杂对象的创建和使用，并使得相同的创建可以创建不同的产品

## 单例模式
	确保某一个类只有一个实例，而且自行实例化并向整个系统提供这个实例，这个类称为	单例类，它提供全局访问的方法
	有三大要点：1. 只能有一个实例 2. 必须自行创建这个实例 3. 它必须自行向整个系统提	供这个实例
### 模式结构
- Singleton：单例

``` c++
int main(int argc, char *argv[])
{
	Singleton * sg = Singleton::getInstance();
	sg->singletonOperation();
	
	return 0;
}

Singleton * Singleton::instance = NULL;
Singleton::Singleton(){

}

Singleton::~Singleton(){
	delete instance;
}

Singleton* Singleton::getInstance(){
	if (instance == NULL)
	{
		instance = new Singleton();
	}
	
	return  instance;
}


void Singleton::singletonOperation(){
	cout << "singletonOperation" << endl;
}
```
### 模式分析
	单例类拥有一个私有构造函数，确保用户无法通过new关键字直接实例化它。该模式中	包含一个静态私有成员变量与静态公有的工厂方法，该工厂方法负责检验实例的存在性	并实例化自己，然后存储在静态成员变量中，以确保只有一个实例被创建
### 优缺点
#### 优点
- 提供了对唯一实例的受控访问。可以严格控制客户怎样以及何时访问它
- 由于在系统内存中只存在一个对象，因此可以节约系统资源
- 允许可变数目的实例

#### 缺点
- 没有抽象层，进行单例类的扩展有很大的困难
- 单例类职责过重
- 滥用单例将带来一些负面问题

### 适用环境
- 系统只需要一个实例对象 
- 客户调用类的单个实例只允许使用一个公共访问点，除了该公共访问点，不能通过其他途径访问该实例
- 在一个系统中要求一个类只有一个实例时才应当使用单例模式

# 结构型模式
## 适配器模式
	将一个接口转换成客户希望的另一个接口，适配器模式使接口不兼容的那些类	一起工作，其别名为包装器。适配器模式既可以作为类结构模型，也可以作为	对象结构模型
### 模式结构
- Target：目标抽象类
- Adapter：适配器类
- Adaptee：适配者类
- Client：客户类

``` c++
int main(int argc, char *argv[])
{
	Adaptee * adaptee  = new Adaptee();
	Target * tar = new Adapter(adaptee);
	tar->request();
	
	return 0;
}

class Adapter : public Target
{

public:
	Adapter(Adaptee *adaptee);
	virtual ~Adapter();

	virtual void request();

private:
	Adaptee* m_pAdaptee;

};

Adapter::Adapter(Adaptee * adaptee){
	m_pAdaptee =  adaptee;
}

Adapter::~Adapter(){

}

void Adapter::request(){
	m_pAdaptee->specificRequest();
}

class Adaptee
{

public:
	Adaptee();
	virtual ~Adaptee();

	void specificRequest();

};
```
### 优缺点
#### 优点
- 将目标类和适配者类解耦，通过引入一个适配器类来重用现在的适配者类，而无须修改原有代码
- 增加了类的透明性和复用性，将具体的实现封装在适配者类中，对于客户端类来说是透明的，而且提高了适配者的复用性
- 灵活性和扩展性都很好，通过使用配置文件，可以很方便地更换适配器，也可以在不修改原有代码的基础上增加新的适配器类

#### 缺点
- 使用有一定的局限性，不能将一个适配者类和它的子类都适配到目标结构

### 适用环境
- 系统需要使用现有的类，而这些类的接口不符合系统的需要
- 想要建立一个可以重复使用的类，用于一些彼此之间没有太大关联的一些类，包括一些可能在将来引进的类一起工作

## 桥接模式
	将抽象部分与它的实现部分分离，使它们都可以独立地变化。它是一种对象结构型模式
### 模式结构
- Abstraction：抽象类
- RefinedAbstraction：扩充抽象类
- Implementor：实现类接口
- ConcreteImplement：具体实现类

``` c++
int main(int argc, char *argv[])
{
	
	Implementor * pImp = new ConcreteImplementorA();
	Abstraction * pa = new RefinedAbstraction(pImp);
	pa->operation();
	
	Abstraction * pb = new RefinedAbstraction(new ConcreteImplementorB());
	pb->operation();		
	
	delete pa;
	delete pb;
	
	return 0;
}

class RefinedAbstraction : public Abstraction
{

public:
	RefinedAbstraction();
	RefinedAbstraction(Implementor* imp);
	virtual ~RefinedAbstraction();

	virtual void operation();

};

RefinedAbstraction::RefinedAbstraction(){

}

RefinedAbstraction::RefinedAbstraction(Implementor* imp)
	:Abstraction(imp)
{
}

RefinedAbstraction::~RefinedAbstraction(){

}

void RefinedAbstraction::operation(){
	cout << "do something else ,and then " << endl;
	m_pImp->operationImp();
}
```
### 模式分析
- 抽象化：忽略一些信息，把不同的实体当做同样的实体对待。在面向对象中，将对象的共同性质抽取出来形成类的过程即为抽象化的过程
- 实现化：针对抽象化给出的具体实现，就是实现化，抽象化与实现化是一对互逆的概念，实现化产生的对象比抽象化更具体，是对抽象化事务的进一步具体化的产物
- 脱藕：将抽象化和实现化之间的耦合解脱开

### 优缺点
#### 优点
- 分离抽象接口及其实现部分
- 多继承方案的更好解决
- 实现细节对客户透明，可以对用户隐藏实现细节

#### 缺点
- 会增加系统的理解与设计难度

### 适用环境
- 一个系统需要在构建的抽象化角色和具体化角色之间增加更多的灵活性，避免在两个层次之间建立静态的继承联系
- 一个类存在两个独立变化的维度，且这两个维度都需要进行扩展
- 对于那些不希望使用继承或者因为层次继承导致系统类的个数急剧增加的系统

## 装饰模式
	动态地给一个对象增加一些额外的职责，就增加对象功能来说，装饰模式比生成子类实	现更加灵活。其别名也称为包装器，与适配器模式的别名相同，但它们适用于不同的场	合
### 模式结构
- Component：抽象构件
- ConcreteComponent：具体构建
- Decorator：抽象装饰类
- ConcreteDecorator：具体装饰类

```c++
ConcreteComponent::ConcreteComponent(){

}

ConcreteComponent::~ConcreteComponent(){

}

void ConcreteComponent::operation(){
	cout << "ConcreteComponent's normal operation!" << endl;
}

class ConcreteDecoratorA : public Decorator
{

public:
	ConcreteDecoratorA(Component* pcmp);
	virtual ~ConcreteDecoratorA();

	void addBehavior();
	virtual void operation();

};

ConcreteDecoratorA::ConcreteDecoratorA(Component* pcmp)
:Decorator(pcmp)
{

}

ConcreteDecoratorA::~ConcreteDecoratorA(){

}

void ConcreteDecoratorA::addBehavior(){
	cout << "addBehavior AAAA" << endl;
}


void ConcreteDecoratorA::operation(){
	Decorator::operation();
	addBehavior();
}
```
### 模式分析
- 与继承关系相比，关联关系主要优势在于不会破坏类的封装性，而且继承是一种耦合度较大的静态关系，无法在程序运行时动态拓展
- 使用装饰模式来实现拓展比继承更加灵活

### 优缺点
#### 优点
- 可以比继承提供更多的灵活性
- 可以通过一种动态的方式来扩展一个对象的功能
- 可以创造很多不同行为的组合
- 具体构建类与具体装饰类可以独立变化，用户可以根据需要增加新的具体构建类和具体装饰类

#### 缺点
- 会产生很多小的对象和装饰类，增加系统的复杂度
- 更加容易出现错误

### 适用环境
- 在不影响其他对象的情况下，以动态、透明的方式给单个对象添加职责
- 需要动态地给一个对象增加功能，这些功能也可以动态地被撤销
- 需要大量的独立扩展 类定义不能继承

## 外观模式
## 享元模式
## 代理模式