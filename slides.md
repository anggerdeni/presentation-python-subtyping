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

# Inspiration

Golang Interface

```go {all|1-3|5-6|8-9|11-13|16|18-21|all}
type CloudStorageRepository interface {
	Write(data []byte) error
}

type gcsRepository struct{}
func (g *gcsRepository) Write(data []byte) error {...}

type sqlRepository struct{}
func (s *sqlRepository) GetAllString() []string {...}

type Uploader struct {
	repo CloudStorageRepository
}

func main() {
	uploaderGCS := Uploader{&gcsRepository{}}

    // ERROR: cannot use &sqlRepository{}
    // (value of type *sqlRepository)
    // as CloudStorageRepository
	uploaderSQL := Uploader{&sqlRepository{}} 
}
```

<!--
Golang can infer that gcsRepository and s3Repository implements CloudStorageRepository interface while sqlRepository doesn't
-->

---
transition: fade-out
layout: image-right
image: https://source.unsplash.com/collection/94734566/1920x1080
---

# Type Hints [^1]

[^1]: [PEP-0484](https://peps.python.org/pep-0484/)

---
transition: fade-out
---

# First Approach: Abstract Base Class (ABC) [^1]

- First introduced in PEP 3119 (Python 3.0) [^2]

[^1]: [ABC](https://docs.python.org/3/library/abc.html)
[^2]: [PEP-3119](https://peps.python.org/pep-3119/)

---
transition: fade-out
---

# A New Discovery: Protocol [^1]

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
