```mermaid
classDiagram
class User {
  +int id
  +string name
  +login()
}

class Order {
  +create()
}

User --> Order
```