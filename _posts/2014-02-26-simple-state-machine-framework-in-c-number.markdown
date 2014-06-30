---
layout: post
title: "Simple State Machine Framework in c#"
date: 2014-02-26 11:37:18 +0530
comments: true
categories: 
---

This blog post covers a very simple, light weight, yet flexible state machine framework in C# .Net.

[Souce code on Github](https://github.com/mubeenh/SSM)

[Download NuGet package](https://www.nuget.org/packages/SSM/)


Concept
--------

**State :** The state of a stateful entity. Represented by <code>IState</code>

**Stateful entity :** An entity that has a defined state. Represented by <code>IStatefulEntity</code>

**Context :** A context in which a state machine executes a transition. It is simply a set of configuration and data that governs the execution of a state machine. Represented by <code>IStateMachineContext</code>

**Transition handler :** A component that handles a transition of a stateful entity between a source state to target state. Represented by <code>ITransitionHandler</code>

Usage
------

Start by creating an entity that represents a state. 

```csharp

    public class ServiceTicketState : IState
    {
        public string Code { get; set; }
        public string Name { get; set; }
        // ...
    }
    
```

Now, create a stateful entity by implementing <code>IStatefulEntity</code> interface. For exmaple:

```csharp
    public class ServiceTicket : IStatefulEntity<ServiceTicketState>
    {
        public ServiceTicketState State { get; set; }
        public string Name { get; set; }
        // ...
    }
```

Next comes transition handlers. A transition hadler is created by implementing <code>ITransitionHandler</code> interface. Below is an example of a generic transition handler that can be used to abstract common functionalities in our Service Ticket example.

```csharp

    public class GenericTransitionHandler : ITransitionHandler<ServiceTicket, ServiceTicketState>
    {
        public virtual string TransitionKey { get { return ""; } }

        public virtual ServiceTicket Execute(ServiceTicket entity, ServiceTicketState nextState, IDictionary<string, object> argumentsMap = null)
        {
            entity.State = nextState;
            return entity;
        }

        public virtual ServiceTicket ValidateTransition(ServiceTicket entity, ServiceTicketState nextState, IDictionary<string, object> argumentsMap = null)
        {
            return entity;
        }

        public virtual void BeforeTransition(ServiceTicket entity, IDictionary<string, object> argumentsMap = null)
        {

        }

        public virtual void AfterTransition(ServiceTicket entity, IDictionary<string, object> argumentsMap = null)
        {

        }
    }
```

Next and final step is to create a 'context' by implementing <code>IStateMachineContext</code> interface. This interface declares following methods that need to be implemented to create a context.

```csharp
ICollection<StateTransition> GetTransitions();
IDictionary<string, ITransitionHandler<T,M>> GetTransitionHandlersMap();
ICollection<M> AllStates();
```

Implementation approach is left to the user. It could from just hard coding for simple cases to database backed storage and retrieval for more advanced cases.

Here is an example implementation (hard coded for simplicity) 

```csharp
        public ICollection<StateTransition> GetTransitions()
        {
            ICollection<StateTransition> map = new List<StateTransition>();
            // Define all possible transitions, and its source and target states
            map.Add(new StateTransition("?", "Transition_New", "New")); // ? means null or undefined state
            map.Add(new StateTransition("New", "Transition_Open", "Open"));
            map.Add(new StateTransition("Open", "Transition_Close", "Closed"));
            map.Add(new StateTransition("Closed", "Transition_ReOpen", "Open"));
            map.Add(new StateTransition("?", "Transition_Cancel", "Cancelled"));

            return map;
        }

        public IDictionary<string, ITransitionHandler<ServiceTicket, ServiceTicketState>> GetTransitionHandlersMap()
        {
            IDictionary<string, ITransitionHandler<ServiceTicket, ServiceTicketState>> map = new Dictionary<string, ITransitionHandler<ServiceTicket, ServiceTicketState>>();
            // create a map of all available transitions and their respective transition handlers
            map.Add("Transition_New", new NewTransitionHandler());
            map.Add("Transition_Open", new OpenTransitionHandler());
            map.Add("Transition_Close", new CloseTransitionHandler());
            map.Add("Transition_ReOpen", new OpenTransitionHandler());

            return map;
        }

        public ICollection<ServiceTicketState> AllStates()
        {
            List<ServiceTicketState> states = new List<ServiceTicketState>();
            // create a list of all possible states
            states.Add(new ServiceTicketState() { Code = "New", Name = "New Ticket" });
            states.Add(new ServiceTicketState() { Code = "Open", Name = "Ticket Open" });
            states.Add(new ServiceTicketState() { Code = "Closed", Name = "Ticket Closed" });
            states.Add(new ServiceTicketState() { Code = "Cancelled", Name = "Ticket Cancelled" });

            return states;
        }

```

Now <code>ServiceTicket</code> entity can be transitioned through state machine. Create an instance of <code>StateMachine</code> as follows.

```csharp
            IStateMachineContext<ServiceTicket, ServiceTicketState> context = new ExampleStateMachineContext();
            StateMachine<ServiceTicket, ServiceTicketState> StateMachine = new StateMachine<ServiceTicket, ServiceTicketState>(context);
```

And request a transition as shown below.

```csharp
            ServiceTicket serviceTicket = new ServiceTicket()
            {
                 Name = "An example service ticket"
            };

            StateMachine.RequestTransition(serviceTicket, "Transition_New");
```

And thats it. Your state machine is ready to handle transitions on the Service Ticket example entity.
