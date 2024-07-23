# FisoThemes' Common Library for TwinCAT

## Overview

A library for providing a set of common utilities and definitions that streamline and standardise the development of PLC applications. The library offers a set of reusable components that simplify everyday tasks and ensure consistency across projects.

## Features

- **Memory Management Utilities**: Functions for dynamic and static memory management, ensuring efficient memory usage and safety.
- **Hashing Functions**: Efficient hashing functions for data integrity and change detection.
- **Type Aliases**: Predefined type aliases for common data types to simplify code readability and maintenance.
- **Type Limits**: Constants defining the limits of various data types, helping to enforce value constraints and prevent errors.
- **Task Information Interfaces**: Interfaces and function blocks for retrieving and managing PLC task information, including detecting PLC cycle changes.
- **Processor Architecture Information**: Easy access to processor architecture details.
- **Value Change Detection**: Function blocks for detecting changes in variable values, enabling responsive and adaptive control logic.
- **Flag Resetting**: Functions for resetting boolean values, useful for state management in PLC programs.
- **Cycle Time Monitoring**: Tools for monitoring and responding to PLC cycle times, ensuring timely execution of control logic.
- **Random Number Generation**: A fast and memory-efficient function block for generating random integers and real numbers, including ranged random numbers.
- **Decimal Places Setting**: Rounding of real values to a specified number of decimal places.


## Usage

### Example: Memory Management

```js
VAR
    pDynamicMemory : POINTER TO BYTE;
END_VAR

// Allocate 100 bytes of memory
pDynamicMemory := F_AllocateMemory(100);

// Checks if the memory is in stack or heap and then deallocates memory
F_DeallocateMemory(pDynamicMemory);
```

### Example: Value Change Detection

```js
VAR
    fbValueChangeDetector   : FB_ValueChangeDetector;
    bValueChanged           : BOOL;
    nValue                  : INT;
END_VAR

// Value to watch for change
fbValueChangeDetector(Value := nValue);

// Check if the value has changed
IF fbValueChangeDetector.HasChanged THEN
    // Handle the value change
    END_IF
```

### Example: PLC Task Information

```js
VAR
    nCycleCount     : UDINT;
    tCycleTime,
    tLastExecTime   : LTIME;
END_VAR

// Get the cycle count of the current task
nCycleCount     := F_GetCurrentPlcTaskInformation().CycleCount;

// Get the task cycle time
tCycleTime      := F_GetCurrentPlcTaskInformation().CycleTime

// Get the execution time of the last cycle
tLastExecTime   := F_GetCurrentPlcTaskInformation().LastCycleExecutionTime;
```

### Example: Random Number Generation
```js
VAR
    fbGenRand       : FB_RandomNumberGenerator(0); // FB_RandomNumberGenerator(<seed>)
    nRand1, nRand2  : LINT;
    fRand1, fRand2  : LREAL;
END_VAR

// Generate random integers and real numbers
nRand1 := fbGenRand.NextInt();
nRand2 := fbGenRand.NextRangedInt(-10, 100);
fRand1 := fbGenRand.NextReal();
fRand2 := fbGenRand.NextRangedReal(5.8, 8.92);
```
#### Comparison with LabVIEW's Random Number Generator

![](./assets/imgs/labview-vs-fscommon-rand-gen-fp.jpg)

![](./assets/imgs/labview-vs-fscommon-rand-gen-bd.png)

### Example: PLC Task Cycle Change Detection

```js
METHOD DoSomething : BOOL
VAR_INST
    fbTaskCycleChanged : FB_PlcTaskCycleChangeDetector(1); // FB_PlcTaskCycleChangeDetector(<Task Index>)
END_VAR

// Check if the task cycle has changed
IF fbTaskCycleChanged.HasChanged THEN
    // Handle the task cycle change
    END_IF
...
END_METHOD
```

### Example: Setting Decimal Places

```js
VAR
    fOriginalValue  : LREAL := 123.456789;
    fRoundedValue   : LREAL;
    nDecimalPlaces  : USINT := 2;
END_VAR

fRoundedValue := F_SetDecimalPlaces(fOriginalValue, nDecimalPlaces);
// fRoundedValue will be 123.46
```

### Example: Get the name and type of the variable as a string
```js
VAR
    sVarName,
    sTypeName,
    sTypeNameByPath : STRING;
    stVariable      : ST_Variable;
END_VAR

// Output : 'stVariable'.
sVarName := F_ExtractVariableNameFromPath( F_GetVariablePath(stVariable) );
// Output : 'ST_Variable'.
sTypeName := F_GetTypeName(stVariable);
// Output : 'ST_Variable'.
sTypeNameByPath := 
    F_GetTypeNameByPath(
        F_GetVariablePathByAddress(ADR(stVariable), SIZEOF(stVariable)) );
```

### Example: Functions to get array bounds of common types
```js
VAR
    arValues : ARRAY[-22..GVL_TypeValueLimits.SINT_MAX] OF LREAL;
    nLower,
    nUpper   : T_Position;
END_VAR

nLower := F_GetLRealArrayLowerBound(arValues);
nUpper := F_GetLRealArrayUpperBound(arValues);
```

## Developer Notes
This project is still in development. There's a lot of work and testing ahead. Changes to functionality may occur in the future.

This is designed to be part of a larger framework that is still under development.