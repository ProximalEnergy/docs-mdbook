# Soiling Sensors


|   |   |   |
|---|---|---|
| **Index** | **Physical Issue** | **Mitigation Strategy** |
| 1 | Some soiling measurement stations only take one measurement per day. These sensors by definition cannot characterize the IAM effect of the soiling on top of the module glass. The effect of incidence angle on soiling can be up to 10% relative to the absolute soiling amount. For example, a 30% soiled module can have an IAM effect up to around 3%. | Proximal currently does not model the additional IAM effect |
| 2 | Some soiling measurement stations report erroneous values such as 100% soiled for unknown reasons. | Proximal filters out values with greater than 90% reported soiling. |
| 3 | Optical soiling sensors cannot characterize the electrical effect of non-uniform soiling   | Proximal does not account for this   |
| 4 | Single active component sensors cannot characterize the electrical effect of non-uniform snow soiling | Proximal does not account for this |
