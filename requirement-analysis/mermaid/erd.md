```mermaid
erDiagram
USER ||--o{ ORDER : places
USER {
    int id
    string name
}
ORDER {
    int id
    decimal total
}
```