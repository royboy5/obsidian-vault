### Features of a System - Functional Requirements

- Describe the system behavior - what "the system must do"
- Easily tied to the objective of our system
- Does not determine its architecture

Example,
"When a **rider logs into the service mobile app**, *the system must* **display a map with nearby drivers within 5 miles radius**"

Input - Log in action
Output - view of a map with nearby drivers

"**When a rider is completed,** *the system will* **charge the rider's credit card and credit the driver, minus service fees*"

Input - When ride is complete
Output - Transfer of funds
### Quality Attributes - Non-Functional Requirements

- System properties that "**the system must have**"
- Examples
	- Scalability
	- Availability
	- Reliability
	- Security
	- Performance
	- etc
- The quality attributes dictate the software architecture of our system
### System Constraints - Limitations and boundaries

- Time Constraints - Strict deadlines
- Financial Constraints - Limited Budget
- Staffing Constraints - Small number of available engineers

---
### Requirements Gathering
- Use cases
	- Situation / Scenario in which our system is used
- User Flows
	- A step by step / graphical representation of each use case

Steps
1. Identify all the actors / users in our system
2. Capture and describe all the possible use-cases / scenarios
3. User Flow - expand each use case through flow of events.
	1. Each event contains
		1. action
		2. data

Example,
"Allow people to join drivers on a route, who are willing to take passengers for a fee"

Actors
1. Rider
2. Driver

Rider Use Cases
- Rider first time registration up
- Driver registration
- Rider login
- Driver login
- Successful match and ride
- Unsuccessful ride

Sequence Diagram - diagram that represents interactions between actors and objects
Part of the UML - standard for visualizing system design
UML diagrams are used mostly for software design
No real standard diagrams representing software architecture in the industry.
UML is not strictly followed in the industry
Sequence diagrams are frequently used to represent interactions between entities.



