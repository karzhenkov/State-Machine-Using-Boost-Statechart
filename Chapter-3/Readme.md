# Chapter - 3 : The life cycle of a STATE

_A state is a functional unit of a state machine, so it makes sense to devote a chapter on life cycle of a state in a state machine for better understanding. A state in a state machine is constructed and deleted multiple times_

##  3.1 Construction of a State

_A STATE comes to life under following conditions

- When its start with the State Machine i.e. its the starting state
or
- When State Machine transit to a state from another state._

## 3.2 Destruction of a State

_A STATE is destructed under following conditions

- When the State Machine is terminated and the STATE was the last active state.
or
- When State Machine transit from the current state to any other State._

## 3.3 Visualizing the State Life Cycle

_Lets continue wit our earlier example with two States

- firstState
- secondState

and two events

- event_MoveToSecond
- event_MoveToFirst

for visualizing, we'll add prints in the constructors and destructors of the state and also add appropriate automated transitions_

```
struct firstState;
struct secondState;

struct statemachine : sc::state_machine<statemachine, firstState> {
};

struct event_MoveToSecond : sc::event<event_MoveToSecond> {};
struct event_MoveToFirst : sc::event<event_MoveToFirst> {};

struct firstState : sc::simple_state<firstState, statemachine> {
    firstState() { cout<<"In State => firstState => Construction"<<endl; }
    ~firstState() { cout<<"In State => firstState => Destruction"<<endl; }
    typedef sc::transition<event_MoveToSecond, secondState> reactions;
};

struct secondState : sc::simple_state<secondState, statemachine> {
    secondState() { cout<<"In State => secondState => Construction"<<endl; }
    ~secondState() { cout<<"In State => secondState => Destruction"<<endl; }
    typedef sc::transition<event_MoveToFirst, firstState> reactions;
};


```
_In the main code, we fire the events to move the state machine from first state to second state and then from second state to first state_

```
int main() {
    statemachine sm;
    sm.initiate();
    sm.process_event(event_MoveToSecond());
    sm.process_event(event_MoveToFirst());
    return 0;
}

```
- _The state_ __firstState__ _is created when the state machine is started and destroyed when the state machine moved to_ __secondState__.
- _Similarly_ __secondState__ _is created when the state machine is moved to it and destroyed when the state machine move back to_ __firstState__.
- _The_ __firstState__ _is created again when the state machine is moved to it and destroyed along with state machine._

## 3.4 Using Dynamic memory for the State machine

_Till now in our examples we're creating state machine instance in stack as_
```
statemachine sm;

```
_However, in real world we need to dynamically allocate the memory to keep it valid beyond the function. This can be done using_ __new__ _and_ __delate__
```
int main() {
    statemachine * sm = new statemachine();
    sm->initiate();
    sm->process_event(event_MoveToSecond());
    sm->process_event(event_MoveToFirst());
    delete sm;
    return 0;
}

```
_There is nothing wrong in it but from C++11 onwards, we may very well use_ __Smart Pointers__ _to dynamically allocate the memory to state machines. The Smart Pointers we can use are_ __shared_ptr__ _or_ __unique_ptr__.

```

shared_ptr<statemachine> sm = make_shared<statemachine>();
sm->initiate();
sm->process_event(event_MoveToSecond());
sm->process_event(event_MoveToFirst());
return 0;
```
## 3.4 Conclusion

_In this chapter, we've learnt about the life cycle of states in a state machine as well as about dynamically creating state machine instances using smart pointers._