# Call Center

Imagine you have a call center with three levels of employee: respondent, manager, and director. An incoming telephone call must be first allocated to a respondent who is free. If the respondent can't handle the call, he or she must escalate the call to a manager. If the manager is not free or not able to handle it, then the call should be escalated to a director. Design the classes and data structures for this problem. Implement a method dispatchCall() which assigns a call to the first available employee.

## Solution

Details can be seen in the code:

```java

public class Call{
    private Rank rank;
    private Caller caller;
    private Employee handler;
    
    public Call(Caller c){
        rank = Rank.Responder;
        caller = c;
    }
    
    public void setHandler(Employee e) { handler = e; }
    
    public void reply(String message) {...}
    public Rank getRank() { return rank; }
    public Rank incrementRank() { ... }
    public void disconnect() {...}
}

abstract class Employee{
    private Ca
}

```
