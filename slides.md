---
theme: seriph
background: /images/cover1.jpg
class: text-center
highlighter: shiki
lineNumbers: false
info: |
  Python Subtyping: Unraveling the Twin Tools of Protocol & ABC
drawings:
  persist: false
transition: slide-left
title: Python Subtyping
mdc: true
---

# Python Subtyping

Unraveling the Twin Tools of Protocol & ABC

<!--<div class="uppercase text-sm tracking-widest">
Novian Deny
</div>-->

<div class="abs-bl mx-14 my-12 flex">
  <img src="/images/pyjogjalogo.jpg" class="h-10">
  <div class="ml-3 flex flex-col text-left">
    <div>Python Jogja</div>
    <div class="text-xs opacity-50">Dec. 16th, 2023</div>
  </div>
</div>


---
layout: default
---

# Table of contents

<Toc maxDepth="1"></Toc>

---
layout: two-cols
---

# The Project
Multpiple Data Source - Single Repository

- A simple service to collect metrics, do some calculation, and return it
- Multiple data source: hot & cold

::right::

<br>
```mermaid {theme: 'neutral', scale: 1}
graph TB

Input-->|Time Range|Service
Service-->|If recent|MetricsServer
Service-->|If old|ColdStorage
MetricsServer-->ProcessedData(Metrics Server Data)
ColdStorage-->ProcessedData(Some Calculation)
ProcessedData-->Output

classDef default fill:#f9f,stroke:#333,stroke-width:4px;
classDef storage fill:#fc0,stroke:#333,stroke-width:2px;
class Input,Output default
class Service,MetricsServer,ColdStorage,ProcessedData storage
```

---
layout: two-cols
transition: slide-up
level: 2
---

# First Approach
No special treatment

```py
class MetricsServerRepository:
    def get_data(time_range: TimeRange) -> List[Metrics]:
        return [...]

class ColdStorageRepository:
    def get_data(time_range: TimeRange) -> List[Metrics]:
        return [...]

def collect_and_process_metrics(self, time_range: TimeRange) -> List[Metrics]:
    if time_range.is_old():
        data = coldStorage.get_data(time_range)
    else:
        data = metricsServer.get_data(time_range)
    
    return some_processing(data)
```

::right::

<v-click>

<br><br>

- <span style="color: lightgreen">Pretty intuitive & straightforward</span>

</v-click>

<v-click>

<br>

- <span style="color: salmon">`collect_and_process_metrics` is directly dependent on repository classes</span>
- <span style="color: salmon">If new criteria is added or repository is changed, we need to modify the `collect_and_process_metrics` code</span>

</v-click>

---
layout: two-cols
transition: fade
level: 2
---

# Abstract Base Class
PEP-3119


```py {all|4-7|5-7|9-11,13-15|17-19|21|all}
from typing import List, Dict
from abc import ABC, abstractmethod

class Repository(ABC):
    @abstractmethod
    def get_data(self, time_range: TimeRange) -> List[Metrics]:
        pass

class MetricsServerRepository(Repository):
    def get_data(self, time_range: TimeRange) -> List[Metrics]:
        return [...]

class ColdStorageRepository(Repository):
    def get_data(self, time_range: TimeRange) -> List[Metrics]:
        return [...]

def collect_and_process_metrics(repository: Repository, time_range: TimeRange) -> List[Metrics]:
    data = repository.get_data(time_range)
    return some_processing(data)

collect_and_process_metrics(ColdStorageRepository(), time_range)
```

::right::

<br><br>

- <span style="color: lightgreen">`collect_and_process_metrics` is no longer directly dependent specific repository classes</span>
- <span style="color: lightgreen">Clear and Enforced Contract: it is clear which methods a subclass should implement (nominal subtyping / explicit)</span>
- <span style="color: lightgreen">Error Detection: Python raises a `TypeError` at instantiation time</span>

<v-click>

<br>

- <span style="color: salmon">Inflexibility: To make a class behave as an ABC, it must explicitly inherit from the ABC</span>

</v-click>

---
layout: two-cols
transition: zoom
level: 2
---

# Protocol
PEP-0544

```py {all|3-5|7-9,11-13|all}
from typing import Protocol

class Repository(Protocol):
    def get_data(self, time_range: TimeRange) -> List[Metrics]:
        ...

class MetricsServerRepository():
    def get_data(self, time_range: TimeRange) -> List[Metrics]:
        return [...]

class ColdStorageRepository():
    def get_data(self, time_range: TimeRange) -> List[Metrics]:
        return [...]

def collect_and_process_metrics(repository: RepositoryProtocol, time_range: TimeRange) -> List[Metrics]:
    data = repository.get_data(time_range)
    return some_processing(data)
```

::right::

<br><br>

- <span style="color: lightgreen">Flexibility: A class can satisfy a Protocol merely by providing the necessary methods or attributes, (structural subtyping / implicit)</span>
- <span style="color: lightgreen">Duck typing - "If it looks like a duck and quacks like a duck, then it's a duck"</span>
- <span style="color: lightgreen">Offers static type checking without runtime overhead</span>

<v-click>
<br>

- <span style="color: salmon">Lack of early error detection - without a type-checker tool, incorrect implementation of a Protocol may run without errors until a problem happens at runtime</span>

</v-click>

---
transition: fade-out
---

# ABC vs Protocol

|                     | **Abstract Base Classes**  | **Protocol**  |
|---------------------|----------------------------|---------------|
| **Typing**         | Nominal or Explicit        | Structural or Implicit  |
| **Flexibility**    | Less (Must explicitly inherit)| More (Only need to implement defined methods)  |
| **Error Detection**| Earlier (instantiation time)   | Later (mostly at runtime, or static-type checker)  |

---
layout: center
class: text-center
---

# EOFError

Thanks for your time and attention! #HappyCoding
