
# NETN-SMC
|Version| Date| Dependencies|
|---|---|---|
|1.0|2023-11-18|NETN-BASE|

The NATO Education and Training Service Management and Control (NETN-SMC) module provides a standard way to send control actions to a federated simulation. The control actions are interactions targeting the federation, an individual federate or an individual simulated entity.

In a federated distributed simulation, the participating systems (federates) provide services to model the synthetic environment. The services use information published in the federation as input and provides data updates and interactions as output. Use control actions to change or trigger service behaviour.

The NETN-SMC FOM module provide base classes for object and interactions to control and describe services in the federation. The provided control action classes are neither publishable nor subscribable but provide the basis for subclassing in other FOM modules.


## Object Classes

Note that inherited and dependency attributes are not included in the description of object classes.

```mermaid
graph RL
SMC_Service-->HLAobjectRoot
BaseEntity-->HLAobjectRoot
```

### SMC_Service

Represents a service provided by the referenced federate with additional information regarding supported control actions.

|Attribute|Datatype|Semantics|
|---|---|---|
|Federate|FederateName|Required: The federate providing the service.|
|SupportedActions|FederateControlActions|Required: Indicates which SMC control ations are supported by the referenced federate.|

### BaseEntity

A base class of aggregate and discrete scenario domain participants. The BaseEntity class is characterized by being located at a particular location in space and independently movable, if capable of movement at all. It specifically excludes elements normally considered to be a component of another element. The BaseEntity class is intended to be a container for common attributes for entities of this type. Since it lacks sufficient class specific attributes that are required for simulation purposes, federates cannot publish objects of this class. Certain simulation management federates, e.g. viewers, may subscribe to this class. Simulation federates will normally subscribe to one of the subclasses, to gain the extra information required to properly simulate the entity.

|Attribute|Datatype|Semantics|
|---|---|---|
|SupportedActions|EntityControlActions|Optional: Indicates what control actions are supported by an individual simulated entity.|

## Interaction Classes

Note that inherited and dependency parameters are not included in the description of interaction classes.

```mermaid
graph RL
SMC_FederationControl-->HLAinteractionRoot
SMC_FederateControl-->HLAinteractionRoot
SMC_EntityControl-->HLAinteractionRoot
SMC_Response-->HLAinteractionRoot
```

### SMC_FederationControl

Base class for all control actions applicable to all federates in the federation. The inherited NETN-BASE `UniqueId` parameter is used to match this interaction with a corresponding `SMC_Response`.


### SMC_FederateControl

Base class for all control actions directed to a specific federate. The inherited NETN-BASE `UniqueId` parameter is used to match this interaction with a corresponding `SMC_Response`.

|Parameter|Datatype|Semantics|
|---|---|---|
|Federate|FederateName|Required: The federate indented as the receiver of this control action.|

### SMC_EntityControl

Control action intended for a federate with primary modelling responsibility for the referened entity. The inherited NETN-BASE `UniqueId` parameter is used to match this interaction with a corresponding `SMC_Response`.

|Parameter|Datatype|Semantics|
|---|---|---|
|Entity|UUID|Reference to a simulation entity for which the control action is inteded.|

### SMC_Response

The response provide an indication if the related action was accepted or rejected/failed by a federate. A single response per sent action is expected.

|Parameter|Datatype|Semantics|
|---|---|---|
|Action|UUID|Required: Reference to the contol action this is a response to. The reference corresponds to the NETN-BASE `UniqueId` parameter of the control action interaction.|
|Status|HLAboolean|Required: Indicates success of failure of a corresonding control action.|

## Datatypes

Note that only datatypes defined in this FOM Module are listed below. Please refer to FOM Modules on which this module depends for other referenced datatypes.

### Overview
|Name|Semantics|
|---|---|
|EntityControlActionEnum|Control actions for entities.|
|EntityControlActions|A set of control actions relevant for individual entities in the simulation.|
|FederateControlActionEnum|SMC Control action enumeration.|
|FederateControlActions|A set of control actions for the federate implementing the service.|
        
### Enumerated Datatypes
|Name|Representation|Semantics|
|---|---|---|
|EntityControlActionEnum|HLAinteger32BE|Control actions for entities.|
|FederateControlActionEnum|HLAinteger32BE|SMC Control action enumeration.|
        
### Array Datatypes
|Name|Element Datatype|Semantics|
|---|---|---|
|EntityControlActions|EntityControlActionEnum|A set of control actions relevant for individual entities in the simulation.|
|FederateControlActions|FederateControlActionEnum|A set of control actions for the federate implementing the service.|
    