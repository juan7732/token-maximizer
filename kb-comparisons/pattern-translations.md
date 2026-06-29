# Pattern Translations

How the same pattern looks in each language.

## Iterate + transform
|        |                                   |
| ------ | --------------------------------- |
| Go     | `for i,v := range s` / manual map |
| Rust   | `iter().map().collect()`          |
| TS     | `arr.map()`                       |
| Python | comprehension `[f(x) for x in s]` |

## Error handling
|        |                             |
| ------ | --------------------------- |
| Go     | `if err != nil` returns     |
| Rust   | `Result` + `?`              |
| TS     | discriminated union / throw |
| Python | exceptions                  |

## DI
|        |                             |
| ------ | --------------------------- |
| Go     | interface arg + struct      |
| Rust   | generic bound / `dyn Trait` |
| TS     | constructor inject          |
| Python | callable/protocol arg       |

## Async unit
|        |                     |
| ------ | ------------------- |
| Go     | goroutine + channel |
| Rust   | tokio task + mpsc   |
| TS     | promise / async fn  |
| Python | asyncio task        |

**See also:** ecosystem-map.md.
