| Aspect           | Horizontal Scaling                         | Vertical Scaling                     |
| ---------------- | ------------------------------------------ | ------------------------------------ |
| Scaling Strategy | Add more machines                          | Upgrade existing machine             |
| Load Balancing   | Required                                   | Not applicable                       |
| Fault Tolerance  | Resilient                                  | Single point of failure              |
| Communication    | Network calls (RPCs) → slower              | Inter-process communication → faster |
| Data Consistency | Weaker (requires distributed transactions) | Stronger and consistent              |
| Hardware Limits  | No hardware limit                          | Limited by machine capabilities      |

