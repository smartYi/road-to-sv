1. Handle Ambiguity
	+ make assumptions & ask clarifying questions
	+ **who** is going to use it and **how** they are going to use it
	+ who, what, where, when, how, why
2. Define the core objects
	Suppose we are designing for a restaurant. Our core objects might be things like `Table`, `Guest`, `Party`, `Order`, `Meal`, `Employee`, `Server`, and `Host`.
3. Analyze Relationships
4. Investigate Actions

# Design Patterns

Singleton and Factory Method design patterns are widely used in intervies.

## Singleton Class

Ensures that a class has only on instance and ensures access to the instance through the application. It can be useful in cases where you have a "global" object with exactly one instance.

```java
public class Restaurant{
	private static Restaurant _instance = null;
	protected Restaurant() {...}
	public static Restaurant getInstance(){
		if (_instance == null){
			_instance = new Restaurant();
		}
		return _instance;
	}
}
```

## Factory Method

Offers an interface for creating an instance of a class, with its subclasses deciding which class to instantiate.

```java
public class CardGame {
	public static CardGame createCardGame(GameType type){
		if (type == GameType.Poker) {
			return new PokerGame();
		}
		else if (type == GameType.BlackJack) {
			return new BlackJackGame();
		}
		return null;
	}
}
```