![swift logo](https://devimages.apple.com.edgekey.net/assets/elements/icons/swift/swift-64x64_2x.png) 
# Swift API 디자인 가이드라인
[Swift API Design Guidelines 원문보기](https://swift.org/documentation/api-design-guidelines)

> 영문-국문 용어 번역

|      영문     | 국문              |
|--------------------|-----------------------|
| parameter     | 매개변수 |
| argument | 인자(전달인자) |
| method | 메서드 |
| property | 프로퍼티 |
| convention| 약속 |
| declare|선언 |
| entity| 개체|
| mutating | 가변|
|non-mutating|불변|
|term of art|전문용어|


# 목차
* [기초](#기초)
* [이름짓기](#이름짓기)
	* [명확하게 이름짓기](#명확하게이름짓기)
	* [자연스러운 이름짓기](#자연스러운이름짓기)
	* [용어 잘 사용하기](#용어잘사용하기)
* [약속들](#약속들)
	* [일반적인 약속](#일반적인약속)
	* [매개변수](#매개변수)
	* [전달인자 레이블](#전달인자레이블)
* [특별한 경우](#특별한경우)


# 기초
* **사용하는 시점에서의 명확함이 가장 중요하다.** 왜냐하면 메소드나 프로퍼티등의 개체 선언은 한번이지만 이후, 반복적으로 사용되기 때문이다. API를 사용하기 쉽고 간결하게 만들자. 만든 것을 한번 읽어보는 것으로 판단하는 것은 충분하지 않다. 항상 실례에서 쉽게 사용할 수 있는지 확인하라.

* **명확함이 간결함보다 중요하다.** 스위프트 코드는 많이 축약 할 수는 있지만, 실행 가능한 가장 짧은 코드를 만드는 것이 목적이 아니다. 스위프트 코드의 간결함은 강력한 타입 시스템(type system) 특징으로부터 야기되며, 이는 자연스럽게 상용구를 줄여 간결한 코드로 만들어주지만 명확함을 떨어뜨리는 부작용이 되기도한다.  

* **모든 선언에 주석을 달아라.** 주석은 디자인에 많은 도움을 주므로 잊지말자.

	> 만약 API 기능을 간단한 용어로 표현하기 힘들다면, API를 디자인 잘 못했을 가능성이 크다.


**<세부내용>**

* **[스위프트 마크다운][] 표현을 사용하라.**
* **선언된 개체를 설명할 때 요약(Summary)과 함께 시작하라.** API를 빠르게 이해하는데 도움이 될 것이다.

	```swift
	/// Returns a "view" of `self` containing the same 	elements in
	/// reverse order.
	func reversed() -> ReverseCollection
	```

* **요약에 집중하라.** 이것이 가장 중요한 부분이다. 훌륭한 문서 주석은 훌륭한 요약으로 이루어져있다.
* **가능한 마침표로 끝나는 단일문장의 일부분을 사용하라.** 완전한 문장을 사용하지 말자.
* **함수나 메소드가 무엇을 하는지 무엇을 반환하는지 설명하라.** 아무것도 하지 않거나 아무것도 반환하지 않는 것은 생략하라.

	```swift
	/// Inserts `newHead` at the beginning of `self`.
	mutating func prepend(_ newHead: Int)

	/// Returns a `List` containing `head` followed by 	the elements
	/// of `self`.
	func prepending(_ head: Element) -> List

	/// Removes and returns the first element of `self` 	if non-empty;
	/// returns `nil` otherwise.
	mutating func popFirst() -> Element?
	```
	* 주의: 위의 popFirst  같이, 요약이 여러 문장들로 구성될 때는 세미콜론으로 나누자.

* **첨자(Subscript)가 무엇에 접근하는지 설명하라.**

	```	swift
	/// Accesses the `index`th element.
	subscript(index: Int) -> Element { get set }
	```		
* **이니셜라이저가 무엇을 생성하는지 설명하라.**

	```swift
	/// Creates an instance containing `n` repetitions of `x`.
	init(count n: Int, repeatedElement x: Element)
	```
* **선언된 개체가 무엇인지 설명하라.**

	```swift
	/// A collection that supports equally efficient insertion/removal
	/// at any position.
	struct List {

	/// The element at the beginning of `self`, or `nil` if self is
	/// empty.
	var first: Element?
	...
	```  


* **선택적으로 계속 단락과 항목을 사용하는 것도 좋다.** 단락은 빈 줄과 완전한 문장으로 구분한다.

	```swift
	/// Writes the textual representation of each    ← Summary
	/// element of `items` to the standard output.
	///                                              ← Blank line
	/// The textual representation for each item `x` ← Additional discussion
	/// is generated by the expression `String(x)`.
	///
	/// - Parameter separator: text to be printed    ⎫
	///   between items.                             ⎟
	/// - Parameter terminator: text to be printed   ⎬ Parameters section
	///   at the end.                                ⎟
	///                                              ⎭
	/// - Note: To print without a trailing          ⎫
	///   newline, pass `terminator: ""`             ⎟
	///                                              ⎬ Symbol commands
	/// - SeeAlso: `CustomDebugStringConvertible`,   ⎟
	///   `CustomStringConvertible`, `debugPrint`.   ⎭
	
	public func print(
	_ items: Any..., separator: String = " ", terminator: String = "\n")
	```

* 요약 이외의 정보를 쓰고 싶을때 적절하게 알려진 **마크업 문서 기호를 사용하자.**
* **명령문 블릿 알고 사용하자.** 엑스코드같은 인기있는 툴은 아래와 같은 키워드의 블릿에 특별한 처리를 해준다. 

	|  [Attention](https://developer.apple.com/library/prerelease/mac/documentation/Xcode/Reference/xcode_markup_formatting_ref/Attention.html) | [Author](https://developer.apple.com/library/prerelease/mac/documentation/Xcode/Reference/xcode_markup_formatting_ref/Author.html)        | [Authors](https://developer.apple.com/library/prerelease/mac/documentation/Xcode/Reference/xcode_markup_formatting_ref/Authors.html)      | [Bug](https://developer.apple.com/library/prerelease/mac/documentation/Xcode/Reference/xcode_markup_formatting_ref/Bug.html)        |
	|------------|---------------|--------------|------------|
	| [Complexity](https://developer.apple.com/library/prerelease/mac/documentation/Xcode/Reference/xcode_markup_formatting_ref/Complexity.html) | [Copyright](https://developer.apple.com/library/prerelease/mac/documentation/Xcode/Reference/xcode_markup_formatting_ref/Copyright.html)     | [Date](https://developer.apple.com/library/prerelease/mac/documentation/Xcode/Reference/xcode_markup_formatting_ref/Date.html)         | [Experiment](https://developer.apple.com/library/prerelease/mac/documentation/Xcode/Reference/xcode_markup_formatting_ref/Experiment.html) |
	| [Important](https://developer.apple.com/library/prerelease/mac/documentation/Xcode/Reference/xcode_markup_formatting_ref/Important.html)  | [Invariant](https://developer.apple.com/library/prerelease/mac/documentation/Xcode/Reference/xcode_markup_formatting_ref/Invariant.html)     | [Note](https://developer.apple.com/library/prerelease/mac/documentation/Xcode/Reference/xcode_markup_formatting_ref/Note.html)         | [Parameter](https://developer.apple.com/library/prerelease/mac/documentation/Xcode/Reference/xcode_markup_formatting_ref/Parameter.html)  |
	| [Parameters](https://developer.apple.com/library/prerelease/mac/documentation/Xcode/Reference/xcode_markup_formatting_ref/Parameters.html) | [Postcondition](https://developer.apple.com/library/prerelease/mac/documentation/Xcode/Reference/xcode_markup_formatting_ref/Postcondition.html) | [Precondition](https://developer.apple.com/library/prerelease/mac/documentation/Xcode/Reference/xcode_markup_formatting_ref/Precondition.html) | [Remark](https://developer.apple.com/library/prerelease/mac/documentation/Xcode/Reference/xcode_markup_formatting_ref/Remark.html)     |
	| [Requires](https://developer.apple.com/library/prerelease/mac/documentation/Xcode/Reference/xcode_markup_formatting_ref/Requires.html)   | [Returns](https://developer.apple.com/library/prerelease/mac/documentation/Xcode/Reference/xcode_markup_formatting_ref/Returns.html)       | [SeeAlso](https://developer.apple.com/library/prerelease/mac/documentation/Xcode/Reference/xcode_markup_formatting_ref/SeeAlso.html)      | [Since](https://developer.apple.com/library/prerelease/mac/documentation/Xcode/Reference/xcode_markup_formatting_ref/Since.html)      |
	| [Throws](https://developer.apple.com/library/prerelease/mac/documentation/Xcode/Reference/xcode_markup_formatting_ref/Throws.html)     | [Todo](https://developer.apple.com/library/prerelease/mac/documentation/Xcode/Reference/xcode_markup_formatting_ref/Todo.html)          | [Version](https://developer.apple.com/library/prerelease/mac/documentation/Xcode/Reference/xcode_markup_formatting_ref/Version.html)      | [Warning](https://developer.apple.com/library/prerelease/mac/documentation/Xcode/Reference/xcode_markup_formatting_ref/Warning.html)    |


# 이름짓기
<a name="명확하게이름짓기"></a>
## 명확하게 이름짓기

* 코드를 이름을 읽는 사람이 **모호하지 않도록 필요로하는 모든 단어를 포함하라**.  
예를들어, 콜렉션 내에 주어진 위치에 있는 요소를 지우는 메소드를 생각해보자.

	```swift
	extension List {
		public mutating func remove(at position: Index) -> Element
	}
	employees.remove(at: x)
	```

	만약 메소드 이름에서 at을 빠뜨리면, 코드를 읽는 사람이 이 메소드를 x 위치에 존재하는 요소를 지우는 것이 아니라, x와 일치하는 요소를 지우는 기능으로 이해 할 수 있다.
	
	```swift
	employees.remove(x) // unclear: are we removing x?  
	```

* **불필요한 단어를 쓰지말자**.  
이름에 사용되는 단어들은 핵심정보만을 전달해야 한다.
의도나 의미를 명확하게 하기 위해 보다 많은 단어들이 필요할지 모르지만 읽는 사람이 이미 알고 있는 정보의 단어들은 쓰지말자. 특히 반복되는 타입 정보는 필요없다.

	```swift
	public mutating func removeElement(_ member: Element) -> Element
	allViews.removeElement(cancelButton)
	```
	
	이 경우, Element는 중요한 의미를 갖지 않는다. 이렇게 표현하는 것이 나을 것이다.


	```swift
	public mutating func remove(_ member: Element) -> Element?
	allViews.remove(cancelButton) // clearer
	```

	때론, 반복되는 타입 정보가 모호함을 없애기 위해 필요하기도 하지만, 일반적으로 타입 정보보다 매개변수의 역할을 설명하는 단어를 사용하는 것이 낫다. 자세한 설명은 다음을 참고하라.


* **변수, 매개변수, 그리고 관련 타입을 이름지을때**, 그들의 타입보다 **역할을 따르자**  
	
	```swift
	var string = "Hello"
	
	protocol ViewController {
		associatedtype ViewType : View
	}
	class ProductionLine {
		func restock(from widgetFactory: WidgetFactory)
	}
	```

	이런 식으로 이름 짓는 것도 명확하거나 좋은 표현이라 할 수 없다. 개체의 역할을 표현할 수 있게 노력하라.

	```swift
	var greeting = "Hello"
	
	protocol ViewController {
		associatedtype ContentView : View  	
	}
	class ProductionLine {
		func restock(from supplier: WidgetFactory)
	}
	```
	
	포로토콜 이름이 그 역할을 나타내는 프로토콜에 영향을 받는 관련타입은 그 타입의 이름에 Type을 붙여서 충돌을 피하라.
	
	```swift
	protocol Sequence {
		associatedtype IteratorType : Iterator
	}
	```

* 인자의 역할을 명확하게 하기위해 **약타입 정보를 보완하라**.  
특히 매개변수의 타입이 NSObject, Any, AnyObject 또는 Int, String 같은 기본 타입일 때, 타입 정보와 사용 상황은 의도를 완전히 전달하지 못할 수 있다. 예를 들어 이 선언은 명확할지 모르나, 사용이 모호하다.

	```swift	
	func add(_ observer: NSObject, for keyPath: String)
	grid.add(self, for: graphics) // vague
	```

	모호한 매개변수의 이름에 명사를 활용해 조금 **더 명확한 역할을 표현** 하라.
	
	```swift
	func addObserver(_ observer: NSObject, forKeyPath path: String)
	grid.addObserver(self, forKeyPath: graphics) // clear
	```

<a name="자연스러운이름짓기"></a>
## 자연스러운 이름짓기

* **영어 문장을 만드는 방식으로 메소드와 함수이름을 만들자**.  

	```swift	
	// Not Preferred:
	x.insert(y, at: z)          “x, insert y at z”
	x.subViews(havingColor: y)  “x's subviews having color y”
	x.capitalizingNouns()       “x, capitalizing nouns”
	
	// Preferred:
	x.insert(y, position: z)
	x.subViews(color: y)
	x.nounCapitalize()		
	```
	
	첫번째나 두번째 매개변수 이후 인자가 핵심 의미를 지니고 있지 않을 때는 다음 줄로 행분리를 해도 괜찮다.
	
	```swift	
	AudioUnit.instantiate(
	with: description, 
	options: [.inProcess], completionHandler: stopProgressBar)
	```

* **팩토리 메소드 이름은 &quot;make&quot;로 시작하라**. `x.makeIterator()`

* **이니셜라이저와 팩토리 메소드를 호출** 할 때 첫 번째 인자의 이름과 문법적으로 연결이 없도록 구성하라. `x.makeWidget(cogCount: 47)`
	
	예를 들어 이 구문은 첫 째 인자와 문법적으로 연결되지 않는 호출로 표현이 된다.
	
	```swift
	let foreground = Color(red: 32, green: 64, blue: 128)
	let newPart = factory.makeWidget(gears: 42, spindles: 14)
	```
	
	다음은, API를 만든 사람이 첫 째 인자가 문법적인 의미를 갖도록 만들었다.
	
	```swift
	let foreground = Color(havingRGBValuesRed: 32, green: 64, andBlue: 128)
	let newPart = factory.makeWidget(havingGearCount: 42, andSpindleCount: 14)
	```
	
	
	이 가이드라인은, 값을 보존하는 타입 변환을 하지 않는 한, 첫 째 인자가 레이블을 가져야 한다는 것을 의미한다.
	
	```swift
	let rgbForeground = RGBColor(cmykForeground)
	```

* **사이드 이펙트에 따라 함수와 메소드 이름을 짓자**.

* 사이드 이펙트가 없는 것들은 명사구로 읽혀야 한다.
`x.distance(to: y)`, `i.successor()`
* 사이드 이펙트가 있는 것들은 동사구로 읽혀야 한다. `print(x)`, `x.sort()`, `x.append(y)`

* **가변/불변 메소드 쌍**은 일관되게 이름짓자. 가변메소드는 종종 불변 메소드와 유사하지만 인스턴스를 갱신하기보다 새로운 값을 반환한다.

* 연산자가 동사로 설명될 때, 가변 메소드를 표현하기 위해 동사를 사용하고 접미사 "ed", "ing"를 붙여 불변 메소드를 표현하라.

	|    Mutating   | NonMutating          |
	|:-------------:|----------------------|
	| `x.sort()`    | `z = x.sorted()`     |
	| `x.append(y)` | `z = x.appending(y)` |
	
* 불변항목의 변형은 과거 분사(ed)를 사용하라.

	```swift
	/// Reverses `self` in-place.
	mutating func reverse()
	
	/// Returns a reversed copy of `self`.
	func reversed() -> Self
	...
	x.reverse()
	let y = x.reversed()
	```

* 과거 분사(ed)를 붙이는 것이 의미에 맞지 않을 경우, 현재 분사(ing)로 불변항목의 변형을 표현하라.

	```swift
	/// Strips all the newlines from `self`
	mutating func stripNewlines()
	
	/// Returns a copy of `self` with all the newlines stripped.
	func strippingNewlines() -> String
	...
	s.stripNewlines()
	let oneLine = t.strippingNewlines()
	```

* 연산자가 명사로 설명될때, 불변 메소드에 명사를 사용하라. 그리고 form 접두사를 가변 항목에 사용하라.

	|      Nonmutating     | Mutating              |
	|:--------------------:|-----------------------|
	| `x = y.union(z)`     | `y.formUnion(z)`      |
	| `j = c.successor(i)` | `c.formSuccessor(&i)` |	

* 불변항목일 때 Boolean 메소드와 프로퍼티는 수신자에 대해 단정하는 것처럼 읽혀야 한다. `x.isEmpty`, `line1.intersects(line2)`

* 무엇인지를 설명하는 프로토콜은 명사로 읽혀야 한다 `Collection`

* 가능성을 설명하는 프로토콜은 able, ible, ing 같은 접미사를 사용해 이름짓자. `Equatable`, `ProgressReporting`


* 위의 항목에 해당하지 않는 다른 종류의 타입, 프로퍼티, 변수, 상수 이름은 명사로 읽혀야 한다.

<a name="용어잘사용하기"></a>
## 용어 잘 사용하기

#### 전문용어: (명사) 특정 분야에서 정확하고 특별한 의미를 가지는 단어나 구.

* 일반적인 용어가 의미를 잘 전달한다면, **알기힘든 용어를 피하라**. 예를 들어, 피부라는 말이 목적에 맞다면 굳이 표피라는 단어를 쓰지말자. 전문용어는 필수적인 의사소통 도구이지만, 사용하지 않으면 전달하기 힘든 중요한 의미를 전달하는 경우에만 사용하라.

* 전문용어를 사용 할 때는 **정해진 의미를 지키자**.
일반용어가 아닌 기술용어를 사용하는 유일한 이유는 의미를 더 정확하게 표현할 수 있기 때문이다. 그러므로, API에는 정해진 의미에 맞게 용어를 사용하라.

* **전문가를 놀래키지 말자** : 어떤 용어에 새로운 의미를   부여한다면 원래 그 용어를 알고 있던 사람들은 놀라거나 화가 날 것이다.

* **초보자를 혼란스럽게 하지말자** : 용어를 배우는 이들은 인터넷 검색으로 기존에 쓰이던 의미를 찾을 것이다.

* **약어를 피하라**. 약어 특히 비표준 약어는 이해하기 어렵다. 왜냐하면 그것을 이해하기 위해서 정확하게 풀어쓴 형태로 바꿔야 하기 때문이다.

	> 약어의 의미는 인터넷에서 쉽게 찾아져야 한다

* **선례를 받아들이자**. 기존 문화가 가진 합리적인 부분을 해치면서까지 초보자에게 적합한 용어 사용을 하지 말자.
	
	초보자에게는 `List`가 `Array`보다 연속적인 데이터 구조를 받아들이기 더 좋은 용어이다. 하지만 현대 컴퓨팅의 기초이며, 모든 프로그래머가 알고있는 용어인 `Array`를  사용하는 것이 좋다. 프로그래머들에게 익숙한 용어를 사용해야 관련 정보를 구하고 질의하기 용이하기 때문이다.
	
	수학과 같은 특정 프로그래밍 영역에서 `sin(x)`처럼 널리 사용되어 온 용어는`verticalPositionOnUnitCircleAtOriginOfEndOfRadiusWithAngle(x)` 같이 설명식 문구보다 훨씬 낫다.
	
	이 경우, 널리 사용되어 온 용어를 사용하는 것이 약어를 피하는 것보다 중요하다. 완전한 용어는 sine이지만, sin(x)가 오랜 시간동안 프로그래머들과 수학자들에게 사용되어 훨씬 익숙하기 때문이다.


# 약속들
<a name="일반적인약속"></a>
## 일반적인 약속

* **O(1)이 아닌 계산 프로퍼티의 복잡도를 문서화하라**. 사람들은 프로퍼티에 접근할때는 상당한 계산이 요구되지 않는다고 생각하는데, 이는 프로퍼티를 단순하게 생각하기 때문이다. 이런 가정이 잘못된 것일 수 있음을 인지하게 하라.

* **그냥 함수보다는 메소드와 프로퍼티를 쓰자**. 그냥 함수는 오직 특별한 경우에만 쓰자.

	1. 명확한 `self`가 없을 때
		> min(x, y, z)
	
	2. 범용적 제네릭 함수일 때
		> print(x)
	
	3. 함수가 원래 있던 것일 때
		> sin(x)

* **대소문자 규칙을 지키자**. 타입과 프로토콜은 `UpperCamelCase`를 사용하고, 그 외 나머지는 `lowerCamelCase`를 사용한다.

	미국영어에서 모두 대문자로 쓰이는 약어나 이니셜은 일률적으로 대문자나 소문자로 나타낸다.	
	```swift
	var utf8Bytes: [UTF8.CodeUnit]  
	var isRepresentableAsASCII = true  
	var userSMTPServer: SecureSMTPServer
	```
	
	그 외 다른 약어는 보통단어처럼 취급한다.
	
	```swift
	var radarDetector: RadarScanner  
	var enjoysScubaDiving = true
	```
	
* 기본 의미를 공유하거나, 구분된 영역에서 사용되는 메소드들은 **이름을 공유 할 수 있다**.

	예를들어, 여기서 이 메소드는 본질적으로 같은 일을 하기 때문에 동일한 이름을 사용한다.  
	
	```swift
	extension Shape {
		/// Returns `true` iff `other` is within the area of `self`.
		func contains(_ other: Point) -> Bool { ... }
	
		/// Returns `true` iff `other` is entirely within the area of `self`.
		func contains(_ other: Shape) -> Bool { ... }
	
		/// Returns `true` iff `other` is within the area of `self`.
		func contains(_ other: LineSegment) -> Bool { ... }
	}
	```
	
	다음과 같이 별도의 영역에서 사용되는 경우도 괜찮다.
	
	```swift
	extension Collection where Element : Equatable {
		/// Returns `true` iff `self` contains an element equal to
		/// `sought`.
		func contains(_ sought: Element) -> Bool { ... }
	}
	```

	그러나 이 `index`는 다른 의미를 가지므로 다르게 이름지어야한다.
	
	```swift
	extension Database {
		/// Rebuilds the database's search index
		func index() { ... }
	
		/// Returns the `n`th row in the given table.
		func index(_ n: Int, inTable: TableID) -> TableRow { ... }
	}
	```
	
	마지막으로, 반환형 오버로딩을 하지말자. 왜냐하면 타입 추론을 어렵게 하기 때문이다.
	
	```swift
	extension Box {
		/// Returns the `Int` stored in `self`, if any, and
		/// `nil` otherwise.
		func value() -> Int? { ... }
	
		/// Returns the `String` stored in `self`, if any, and
		/// `nil` otherwise.
		func value() -> String? { ... }
	}
	```


## 매개변수
```swift
 func move(from start: Point, to end: Point)
```
	

* **문서 역할을 할 수 있는 매개변수 이름을 선택하라.** 비록 매개변수 이름이 함수나 메소드 호출시에 보이지 않더라도, 설명적인 측면에서 중요한 역할을 할 수있다.

	문서를 읽기 쉽게 만드는 이름을 선택하라. 예를들어 아래와 같은 이름은 쉽게 읽힐 수 있다.
	
	```swift
	/// Return an `Array` containing the elements of `self`
	/// that satisfy `predicate`.
	func filter(_ predicate: (Element) -> Bool) -> [Generator.Element]
	
	/// Replace the given `subRange` of elements with `newElements`.
	mutating func replaceRange(_ subRange: Range, with newElements: [E])
	```
	
	그러나 아래는 문서를 어렵게 만들고, 비문법적이다.
	
	```swift
	/// Return an `Array` containing the elements of `self`
	/// that satisfy `includedInResult`.
	func filter(_ includedInResult: (Element) -> Bool) -> [Generator.Element]
	
	/// Replace the range of elements indicated by `r` with
	/// the contents of `with`.
	mutating func replaceRange(_ r: Range, with: [E])
	```

* **매개변수의 초기값 설정은 API 사용을 단순화 한다.** 자주 사용되는 값이 매개변수 초기값으로 사용 될 수 있다.

	인자 초기값은 관련 없는 정보를 숨김으로써 가독성을 높인다.
	
	```swift
	let order = lastName.compare(
	royalFamilyName, options: [], range: nil, locale: nil)
	```
	
	더 단순해질수 있다.
	
	```swift
	let order = lastName.compare(royalFamilyName)
	```
	
	인자 초기값 사용은 일반적으로 (매개변수는 다르지만 같은 이름의) 다양한 메소드 집합보다 선호된다. 왜냐하면 사용자가 API를 이해하기 쉽기 때문이다.
	
	```swift
	extension String {
		/// ...description...
		public func compare(
		_ other: String, options: CompareOptions = [],
		range: Range? = nil, locale: Locale? = nil
		) -> Ordering
	}
	```
	
	위의 내용은 그리 간단하지 않아 보일 수있지만, 아래의 내용보다는 훨씬 간단하다.

	```swift
	extension String {
		/// ...description 1...
		public func compare(_ other: String) -> Ordering
		/// ...description 2...
		public func compare(_ other: String, options: CompareOptions) -> Ordering
		/// ...description 3...
		public func compare(
		_ other: String, options: CompareOptions, range: Range) -> Ordering
		/// ...description 4...
		public func compare(
		_ other: String, options: StringCompareOptions,
		range: Range, locale: Locale) -> Ordering
	}
	```
	
	메소드 집합의 모든 멤버는 개별적으로 문서화 되어야하고, 사용자에게 이해되어야 한다. 그리고 유사하지만 완전히 동일하지는 않은 멤버들의 - 예, `foo(bar: nil)`과 `foo()` -  미세한 차이를 찾아내 비슷한 문서를 작성하는 지루한 작업이 요구될 수있다. 그렇기 때문에 인자 초기값이 있는 단일 메소드를 사용하는 것이 좋다.

* **매개변수 목록의 끝 부분에 초기값이 있는 매개변수를 쓰자.** 초기값이 없는 매개변수들이 보통 메소드의 핵심 기능을 담당하며, 메소드가 호출될때 안정적인 초기 사용 패턴을 제공한다.

<a name="전달인자레이블"></a>
## 전달인자 레이블

```swift
 func move(from start: Point, to end: Point)  
 x.move(from: x, to: y) 
```
* **인자 레이블이  특별한 의미를 가지지 않는다면 생략하라**. `min(number1, number2)`, `zip(sequence1, sequence2)`

* **값을 보존하는 타입 변환 이니셜라이저에서 첫번째 인자 레이블은 생략하라**. `Int64(someUInt32)`  
첫번째 인자는 항상 변환의 원본이어야 한다.  
	
	```swift
	 extension String {  
	 // Convert `x` into its textual representation in the given radix  
	 init(_ x: BigInt, radix: Int = 10)   ← Note the initial underscore  
	}  
   
	 text = "The value is: "  
	 text += String(veryLargeNumber)  
	 text += " and in hexadecimal, it's"  
	 text += String(veryLargeNumber, radix: 16)  
	```
	
	타입의 크기가 작아지는 변환은 크기가 작아지는 것을 설명하는 레이블을 작성하는것이 좋다.
	
	```swift
	 extension UInt32 {  
		 /// Creates an instance having the specified `value`.  
		 init(_ value: Int16)            ← Widening, so no label  
		 /// Creates an instance having the lowest 32 bits of `source`.  
		 init(truncating source: UInt64)  
		 /// Creates an instance having the nearest representable  
		 /// approximation of `valueToApproximate`.  
		 init(saturating valueToApproximate: UInt64)  
	 }
	```
	> 값을 보존하는 타입 변환은 단사형(monomorphism)이다. 원본이 다르면 결과도 각각 상황에 따라 다 르기 때문이다. 예를 들어 `Int8`에서 `Int64`로 변환하는 것은 값을 보존한다. 왜냐하면 어떤 `Int8` 값이라도 `Int64` 값으로 변환되기 때문이다. 하지만 반대방향으로의 변환은 값을 보존할수 없다. `Int64`는 `Int8`로 표현할 수 있는 것보다 더 큰 값을 가지기 때문이다.
	>
	> * 참고: 원래 값을 찾는 기능은 값을 보존하는 변환인지와 상관없다.

* **첫 째 인자가 전치사구의 부분일때, 인자 레이블을 쓰자**.인자 레이블은 보통 전치사로 시작해야한다. `x.removeBoxes(havingLength: 12)`

	첫 두 인자가 동일한 추상화 개념을 지닐 때는 예외이다.
	
	```swift
	 a.move(toX: b, y: c).  
	 a.fade(fromRed: b, green: c, blue: d)
	```
	
	그런 경우, 전치사 다음에 인자를 시작하여 추상화를 명확하게 하라.
	
	```swift
	 a.moveTo(x: b, y: c)  
	 a.fadeFrom(red: b, green: c, blue: d)
	```

* **첫번째 인자가 문법적 구문의 일부분일때, 레이블을 생략하라**. 대신기본이름의 앞에 단어를 덧붙이자.

	이 가이드라인은, 만약 첫번째 인자가 문법적 구문의 일부분이 아니라면, 그 인자는 레이블을 가져야함을 의미한다
	
	```swift
	view.dismiss(animated: false)
	let text = words.split(maxSplits: 12)
	let studentsByName = students.sorted(isOrderedBefore: Student.namePrecedes)
	```
	
	그 구문이 정확한 의미를 전달하는 것이 중요하다. 아래의 내용은 문법적으로 옳으나 의미 전달이 잘못되었다.
	
	```swift
	view.dismiss(false)   Don't dismiss? Dismiss a Bool?
	words.split(12)       Split the number 12?
	```
	
	초기값을 지니는 인자들이 생략되었을 때 남아있는 첫번째 인자가 문법적 구문의 일부분이 되지 않으면  항상 레이블을 지녀야한다.

* **위의 예에 해당하지 않는 다른 인자들은 모두 레이블을 붙이자.**

<a name="특별한경우"></a>
# 특별한 경우

* **API에 나타나는 클로져 매개변수와 튜플 멤버에 인자 레이블을 붙이자**.

	이해하기 쉽고, 문서 주석에서 참조할 수 있으며, 튜플 멤버를 쉽게 사용 할 수 있게 해준다.

	```swift
	 /// Ensure that we hold uniquely-referenced storage for at least  
	 /// `requestedCapacity` elements.  
	 ///  
	 /// If more storage is needed, `allocate` is called with  
	 /// `byteCount` equal to the number of maximally-aligned  
	 /// bytes to allocate.  
	 ///  
	 /// - Returns:  
	 ///   - reallocated: `true` iff a new block of memory  
	 ///     was allocated.  
	 ///   - capacityChanged: `true` iff `capacity` was updated.  
	 
	 mutating func ensureUniqueStorage(  
	 	minimumCapacity requestedCapacity: Int,   
		allocate: (byteCount: Int) -> UnsafePointer<Void>  
	 ) -> (reallocated: Bool, capacityChanged: Bool)  
	```
	
	클로저 매개변수에 전달인자 레이블을 사용하는 경우 기술적으로는 인자 레이블이지만, 레이블을 선택해 매개변수 이름인 것처럼 사용해야한다. 함수 내부에서 클로저를 호출하는 경우에는 첫번째 인자를 포함하지 않는 함수와 동일하게 읽는다.
	> allocate(byteCount: newCount * elementSize)
	

* 오버로드 집합에서 모호함을 피하기 위해, **제약이 없는 다형성에 좀 더 주의를 기울이자** (예를들어, `Any`, `AnyObject` 그리고 제약이 없는 제너릭 매개변수)

	예를 들어 이 오버로드 집합을 보자.
	
	```swift
	struct Array {
		/// Inserts `newElement` at `self.endIndex`.
		public mutating func append(_ newElement: Element)
		
		/// Inserts the contents of `newElements`, in order, at
		/// `self.endIndex`.
		public mutating func append(_ newElements: S)
		where S.Generator.Element == Element
	}
	```
	
	두 메소드는 의미적으로 같은 유형이고, 처음에는 인자가 잘 구분된다. 하지만 `Element`가 `Any`일때, 하나의 엘리먼트가 엘리먼트들의 시퀀스와 같은 타입을 가질 수 있다.
	
	```swift
	var values: [Any] = [1, "a"]
	values.append([2, 3, 4]) // [1, "a", [2, 3, 4]] or [1, "a", 2, 3, 4]?
	```
	
	모호함을 없애려면, 두번째 오버로드를 더 명확하게 이름 지어야 한다.
	
	```swift
	struct Array {
		/// Inserts `newElement` at `self.endIndex`.
		public mutating func append(_ newElement: Element)
		
		/// Inserts the contents of `newElements`, in order, at
		/// `self.endIndex`.
		public mutating func append(contentsOf newElements: S)
		where S.Generator.Element == Element
	}
	```
	
	새롭게 지어진 이름 `append(contentsOf newElements: S)`은 `append(_ newElements: S)`에 비해 주석과 더 잘 맞고, 모호함을 줄여준다.  



**번역**: NEXT 131040 안덕우  
**오역 및 용어 수정**: 전미정

[스위프트 마크다운]:https://developer.apple.com/library/content/documentation/Xcode/Reference/xcode_markup_formatting_ref/
