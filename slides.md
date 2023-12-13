---
theme: seriph
background: https://source.unsplash.com/collection/94734566/1920x1080
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

---
layout: default
---

# Table of contents

<Toc maxDepth="1"></Toc>

---
transition: fade-out
---

# Background
Python Coding

Writing python for me is so much fun

<v-click>

- It is fast to write

</v-click>
<v-click>

- Fast to run

</v-click>
<v-click>

- Fast to change

</v-click>


<v-click>

But maintaining it? Might be not so much fun.

</v-click>

<v-click>

```py
def process_records(records):
    result = {}
  
    for record in records:
        name, age, country = record
        if country not in result:
            result[country] = []
        result[country].append((name, age))
  
    return result
```

</v-click>

<!--
In the past, in one of my python project I don't use type hints or not using it too much
When the time comes that I need to do some changes, it can take me a while to understand what goes where and what is of what type, etc
-->

---
transition: fade-out
layout: image-right
image: https://source.unsplash.com/collection/94734566/1920x1080
---

# Type Hints
PEP-0484 [^1]

[^1]: [PEP-0484](https://peps.python.org/pep-0484/)

---

# The Project
Multpiple Data Source - Single Repository

- Ada project belakangan yang mana itu simple CLI app
- Detailnya: dia punya 2 data source
- Pake type hint manual, perlu define 2 function dengan 2 signature yang berbeda

---

# Inspiration

Golang Interface

```go {all|1-3|5-12|14|17-18|20-21}
type CloudStorageRepository interface {
	Write(data []byte) error
}

type gcsRepository struct{}
func (g *gcsRepository) Write(data []byte) error {...}

type s3Repository struct{}
func (s *s3Repository) Write(data []byte) error {...}

type sqlRepository struct{}
func (s *sqlRepository) GetAllString() []string {...}

type Uploader struct { repo CloudStorageRepository } // TODO: change to func not struct

func main() {
	uploaderGCS := Uploader{&gcsRepository{}}
	uploaderS3 := Uploader{&s3Repository{}}

	// ERROR: cannot use &sqlRepository{} as CloudStorageRepository
	uploaderSQL := Uploader{&sqlRepository{}} 
}
```

<!--
Golang can infer that gcsRepository and s3Repository implements CloudStorageRepository interface while sqlRepository doesn't
-->

---
transition: fade-out
---

# First Approach: Abstract Base Class (ABC)
PEP-3119 [^1]

- First introduced in PEP 3119 (Python 3.0) [^2]
- runtime check

[^1]: [ABC](https://docs.python.org/3/library/abc.html)
[^2]: [PEP-3119](https://peps.python.org/pep-3119/)

---
transition: fade-out
---

# Protocol
More Pythonic Way [^1]

[^1]: [PEP-0544](https://peps.python.org/pep-0544/)

---
transition: fade-out
---

# My Use Case

---
layout: center
class: text-center
---

# Learn More

[Documentations](https://sli.dev) · [GitHub](https://github.com/slidevjs/slidev) · [Showcases](https://sli.dev/showcases.html)
